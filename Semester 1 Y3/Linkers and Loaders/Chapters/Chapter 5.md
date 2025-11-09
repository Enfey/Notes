# Symbol Management
Symbol management is considered a linker's key function, encompassing symbol binding and resolution - the process in which a symbol reference in one file to a name in a second file is resolved to a machine address. 

Without some way to refer from one module to another, there wouldn't be much use for a linker's other capabilities.

It should be noted that the sheaf of tables that a compiler maintains for things like establishing context in type checking is mostly discarded. The compiler emits only externally visible symbols, potentially function-level symbols for debugging. ELF's `.symtab` is flat and global at the file level - there is no 'subtables' tied to lexical scopes.
## Binding and Name Resolution
Linkers handle a variety of kinds of symbols. Each input module includes a symbol table. The symbols include the following:
### Symbol types 
- Global symbols defined and perhaps referenced in the module
- Global symbols referenced but not defined in the module(externals)
- Segment/section names, considered to be global symbols defined to be at the beginning of the segment
- Nonglobal/function-level symbols, usually for debuggers and crash dump analysis, unneeded for linking process.
- Line number information, tell source language debuggers correspondence between source lines and object code. 

Linker reads all of symbol tables in input module and extracts necessary information such as size, value and info. Then builds the link-time symbol tables and uses those to guide the linking process. Depending on output format, linker may place some or all of the symbol information in the output file. 

Some formats such as ELF can have multiple symbol tables per file, ELF Shared libraries can have one symbol table with just info needed for dynamic linker, and separate, larger table useful for debugging and relinking. 
## Symbol table formats
Linker symbol tables generally similar to those in compilers, though usually simpler. Generally maintain a few distinct symbol tables; one lists the input files and library modules, keeping the per file information. A second table handles global symbols, the ones that the linker has to resolve among input files. A third table may handle intramodule debugging symbols, though more often that not the linker need not create a full-fledged symbol table for debug symbols, given it only needs to pass the debugging symbols through the input to the output file. 

The coalesced, in-memory symbol table for a linker is essentially a hash table with a separate chaining collision resolution mechanism. Each 'hash header' points to a linked list of symbols that hash to the same value. 

To locate a symbol in the table:
1. `hash(symname) = hashval`
2. `bucket = hashval % NBUCKET`
3. Traverse linked list `symhash[bucket]`
4. Compare names and other properties until a hash is found.

```C
struct sym *symhash[NBUCKET]; //array of NBUCKET elements, each elem is pointer to a sym struct

struct sym {
	struct sym *next;
	int         fullhash;
	char        *symname;  // string derived from st_name
}
```

`strcmp` for each symbol in a bucket is expensive and modern languages like C++ produce long, mangled names making comparison operations expensives, even more so with exaggerated collisions. 

The optimisation is seen above. Store the pre-modulo hash value first, and for each symbol in the chain, compare its stored hash value to the computed hash, if it matches, then do `strcmp()`. 

## Module Tables
Linker needs to track every input module seen during linking run. This includes both those linked explicitly and those extracted from libraries. This is the role of the **module table**. For most files, the key information is in the headers, so the table most often just stores a copy of the header.

For our purposes, this is no longer used; this is the forerunner to the methodology employed by modern linkers. 

In short, when a linker runs, it keeps track of every such input module during the link. Each module table entry represents one input file. Each entry holds roughly the following kind of data:

| Field                       | Description                                                                                   |
| --------------------------- | --------------------------------------------------------------------------------------------- |
| Header                      | A copy of the a.out file header, gives sizes and addresses of .text, .data, .bss, and tables. |
| Symbol Table Pointer        | Pointer to in-memory copy of the symbol table.                                                |
| String Table Pointer        | Pointer to the in-memory copy of symbol names.                                                |
| Relocation Table Pointer(s) | For .text and .data relocation entries.                                                       |
| Computed output offsets     | The positions of this module's text/data/bss in final output file after storage allocation.   |
| Flags                       | Indicate if the module was pulled from library, has unresolved symbols etc.                   |
| Next/previous pointers      | Chain modules together.                                                                       |


This table is used as follows:
First Pass:
	For each input file, read file header and symbol table
	Create new module table entry
	Copy the symbol table and string table into memory such that linker can utilise them
	It adjusts symbol entries such that each symbol name points to its currently in-memory string, (instead of a file offset)
Second pass:
	Walk through all module table entries
	Resolve definitions and references
	If an undefined symbol is found and can be satisfied by a library member, that member is read in - a new module table entry is created for it.
Third pass:
	Linker assigns output addresses to each module's sections
	Updates the computed offsets fields in each module table entry
Fourth pass:
	Applies relocations using data in the module table.
```
typedef struct Module {
    AOutHeader header;          // copy of a.out file header
    Symbol *symtab;            // pointer to in-memory symbol table
    char *strings;              // pointer to in-memory string table
    Reloc *text_relocs;         // relocations for .text
    Reloc *data_relocs;         // relocations for .data
    long text_offset;           // output offset for .text
    long data_offset;           // output offset for .data
    long bss_offset;            // output offset for .bss
    struct Module *next;        // next module in list
} Module;
```

### ELF linker equivalent
Of course, there is no single equivalent. But an internal data structure that suits the needs of modern ELF can be developed to act in a similar way to a module table. 

Initially, one parses the ELF header `ELF32_Ehdr`, and uses it to locate sections, symbol tables, and relocation entries in the file.

We use this to construct an entry into an ELF input representation semi-equivalent to a module table:
```C

#include <stdint.h>

typedef struct Relocation {
	uint64_t        offset;
	uint32_t        type;
	struct Symbol * symbol;
	int64_t          addend; //rela
} Relocation;

typedef struct Symbol {
	char *name;
	struct InputSection    *section;
	uint64_t value; //offset within section
	uint8_t size;
	uint8_t binding; 
	uint8_t type;                  
    uint8_t visibility;      
} Symbol;

typedef struct InputSection {
	char *name;
	uint8_t *data; //pointer to section contents 
	uint64_t size;
	uint64_t alignment;
	Relocation *relocs;
	uint32_t num_relocs;
	uint32_t flags;
    uint32_t type;
    
    uint64_t vaddr;
    uint64_t offset;
} InputSection;

typedef struct InputFile {
	char *filename;
	InputSection **sections;
	uint32_t num_sections;
	Symbol **symbols;
	uint32_t num_symbols;
} InputFile;
```
#### Parsing
1. Parse ELF File header
	Use ELF header `Elf32_Ehdr` to locate **section header table** and `.symtab`
2. Parse Sections
	For each section in the ELF file:
	- Allocate `InputSection` 
	- Fill in:
		- `name` -> from section header string table
		- `size, alignment` -> from section header
		- `flags`
		- `type`
		- `data` -> may be null for bss
	- If the section has relocations, parse them into Relocation structs:
		- `offset` -> where in section to apply the relocation
		- `symbol` -> will be resolved later
		- `type` -> type of relocation
		- `addend` -> `.rela{name}` only
3. Parse symbols
	For each entry in `.symtab`
	- Allocate a `Symbol`
	- Fill in
		- `name` -> symbol name from `.strtab`
		- `section` -> pointer to `InputSection` symbol belongs to(NULL if not defined)
		- `value` -> offset within section
		- `size, binding, type, visibility`
4. Create `InputFile` object
	Set `filename`
	Store arrays of `InputSection*` and `Symbol*`
	Store counts `num_sections` and `num_symbols`.
#### Name Resolution
1.  Build global symbol table (visible to all modules)
	For each `Symbol`
	- If `binding` is `STB_GLOBAL` or `STB_WEAK`, insert into global symbol table
	- Undefined symbols are marked as needing resolution
2. Resolve undefined symbols
	For each undefined symbol in all `InputFile` s:
	- Look up in the global symbol table.
	- If found
		- Update symbol `symbol -> section` and `symbol->value` to point to the defining section and offset
	- If not found
		- If using a library, scan it for object files defining the symbol, parse it into `InputFile` and repeat
	- Else, report linker error: undefined symbol.
3. Update relocation entries
#### Storage Allocation
Classify sections by runtime properties
	Determine `SHF_ALLOC`
	Determine access flags (RWX)
	Determine alignment requirement `sh_addralign` and size
	Determine whether loaded from file or can be zeroed in memory e.g., `.bss`
3. Form permission groups/segments(`PT_LOADS`)
	This is the initial grouping via *page permissions* according to headers:
	- Read+Execute - usually `.text`
	- Read Only - `.rodata`
	- Read + Write - `.data`, `.rel/a`, etc
	- Read + Write but zero initialised `.bss`
		- Avoid creating pages that are both W and X for security purposes. 
4. Order sections within each group
	The linker picks a conventional ordering of the sections of each permission group such that they can be arranged logically.
	- Text segment: ELF header & program headers, then `.interp`, `.init`, `.text`, `.rodata` etc
	- Data segment: remember mapped twice, at end of text too, so starts exactly one page after last text page, `.data`, small data sections.
	- BSS: right after data in memory. 
	- Once output address computed with respect to alignment for given section, update `Symbol -> value = section_base_address + symbol->value`
#### Applying relocations
- Walk through each `InputSection` with relocations
	- For each `Relocation
		- Compute relocation target address
			`uint64_t *location = (uint64_t *)(section->data + reloc->offset); 
			`*location = reloc->symbol->value + reloc->addend;`

	- Apply relocation based on type. 

Just write final `ET_EXEC` ELF output now.


#### Writing final ELF output

## Global Symbol Table

## Symbol Resolution

## Special Symbols



# Symbol Tables in ELF


