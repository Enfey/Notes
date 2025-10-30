# Obj file summary
Output of compilers and assemblers, create object files containined generated binary code and data and often metadata for a given source file. 
Linkers, combine multiple object files into one file. Loaders, take coalesced object files and load into memory.

## What goes into object file?
1. Header Information - Overall metadata e.g., name of source file, code size
2. Object Code - binary/hex instructions and data generated.
3. Relocation Information - list of places in object code that need to be modified to produce valid binary exec, along with how to do
4. Symbols - global symbols, symbols to be imported, local symbols, in table(s)
5. Debugging information - not required for linking, but useful to a debugger e.g., line number information, local symbols, descriptions of data structures used by object code e.g., C struct definitions. 
## Types of Object Files
Design is compromise driven by use case of file.
1. **Linkable** - used as input by a link editor, contains extensive symbol and relocation information needed by linker along with object code. Often divided up into many small logical segments that will be treated differently by the linker.
2. **Executable** - Contain object code, page aligned to permit file mapping, little or no relocation info, no symbol info unless doing run-time dynamic linking. Single large segment, or set of small segments,that directly reflect hardware execution environment (read only vs read/write pages)
3. **Loadable** - Capable, loaded into memory as a library, along with a program. 
# a.out
Precede ELF. Associated, systems that create new process, empty address space, thus link to load at fixed address with no load-time relocation. 

Consists of **small header**, followed by executable code aka text, and initial values for static data. 

I and D spaces on some architectres, separate address spaces for instructions and data, 2 section object files, to double program size, could also share data among other programs. 

## a.out Header
$$
\begin{gathered}
\text{int a\_magic //magic number} \\
\text{int a\_text //text segment size} \\
\text{int a\_data //initialised data size}  \\
\text{int a\_bss //uninitialised data size} \\
\text{int a\_syms //symbol table size} \\
\text{int a\_entry //entry point} \\
\text{int a\_trsize //text relocation size} \\
\text{int a\_drsize//data relocation size} \\
\end{gathered}
$$
Magic num indicates what kind of executable file this is. 
Text and data segment sizes are sizes in bytes of read only code and read/write data that follow header. 

UNIX automatically initialises newly allocated memory to zero, any data with an initial content of 0 or whose contents don't matter need not be present in the $a.out$ file. 

The entry field gives starting virtual address, while syms, tr_size, and dr_size specify how much symbol table and relocation information follows the data segment in the file. Programs that have been linked and are ready to run need neither symbol nor relocation information, so fields are zero in runnable files. 

## NMAGIC
Process for starting simple 2 segment a.out file is straightforward:
1. Read header, get segment sizes
2. Check to see if exists shareable code segment for this file. If so, map into process's address space. If not, map text segment from file into process image. 
3. Create private data segment large enough for combined data and bss, and map into process, read data segment from file into data segment, zero out the bss segment, placed directly after text segment, not page-aligned
4. Create and map in a stack segment, often at top address as stack grows downward. 
5. Set registers, jump to starting address.
![[Pasted image 20251024192610.png]]

When say map here mean copying really. 
Used until paging became prevalent. 
## ZMAGIC
Improvement to NMagic, leverage virtual memory and paging, enable file mapping. ZMagic structural changes included page alignment for segments:
- a.out header expanded to match page size
- text segment size rounded up to next page boundary
- data segment, no rounding, as followed by BSS segment which is zeroed.
OS allocated virtual address space for program according to header size info. Instead of copying program content immediately, uses a.out file as paging disk.  Mark pages in mapped segments as not present, so page faults generated, page faults handled by paging system by backing up pages with the necessary 4KB aligned chunks of the file on disk, moving them into corresponding physical page frames. Text pages mapped as read-only, data mapped as copy-on-write.

Speeds up program startup, reduce unneeded paging, wastes memory space by expanding header, and gap between text and data is wasting 2KB on average.
## QMAGIC
Compact pageable format, developed to supersede ZMagic drawbacks of wasting disk and memory space due to expanded headers and padding.

Considers header to be part of text segment. Kernel maps program into memory such that header + beginning of text segment are mapped starting at virtual address $pagesize$, first virtual page unmapped, null pointer dereferences now trigger fault.

Also round up data segment to full page.  Last page of data segemtn padded out with zeros for bss data; if more bss data exists than fits in padding area, header contains size of remaining bss area to allocate. 

## Relocatable a.out
Linkable version of a.out

Includes several sections necessary for linking process detailed in header:
$$
\begin{gathered}
\text{int a\_magic //magic number} \\
\text{int a\_text //text segment size} \\
\text{int a\_data //initialised data size}  \\
\text{int a\_bss //uninitialised data size} \\
\text{int a\_syms //symbol table size} \\
\text{int a\_entry //entry point} \\
\text{int a\_trsize //text relocation size} \\
\text{int a\_drsize//data relocation size} \\
\end{gathered}
$$
$a\_syms$, $a\_trsize$, $a\_drsize$. Follow main text and data sections in the file. 
![[Pasted image 20251026220946.png]]
### Relocation Entries
Serve 2 functions:
1. Mark places in code that must be modified due to segment relocation
	Linkers, perform storage allocation, initial assumption is all segments logically follow from address 0 or some other base, linker's first major task, read input files, group corresponding segments/sections to reflect memory layout and alignment. Means segments must be relocated to new, calculated, non-zero base addresses. Affects both absolute addresses and inter-segment program references. 
2. Marking references to undefined external symbols, so linker knows where to insert final address value. 

#### Format of Relocation Entry
- *Address* - Specifies byte offset within section where fixup should be applied. 
- *r_length* - Specifies width of relocatable item. 
- *r_pcrel* - Specifies whether relocation should be computed relative to program counter (instruction address) or absolute.
- *r_extern* - Controls interpretation of the index field;
	- If off, index tells which segment(text, data, bss) item is addressing
	- If on, this is a reference to an external symbol, index is symbol number in symbol table.
![[Pasted image 20251026222454.png]]
![[Pasted image 20251026223323.png]]

### Symbols and Strings
Final section(s) of a.out file is symbol table. 
Each entry = 12 bytes, describes single symbol.
Unix compilers permit arbitrarily long identifiers, so name strings are all in string table following symbol table.
- Symbol table: array of symbol records, each record contains - symbol name offset, type, debug info, spare value
- String table: blob of NUL-terminated strings containing actual symbol names, pointed to by offset in symbol table. 
![[Pasted image 20251026223154.png]]
First item is offset/index into string table.
Type item is a byte long; if low bit set, symbol is global and visible to other modules, non-global if not, non-global, non-external symbols not needed here technically, but used by debuggers. Rest of bits specify symbol type(n_type):
- text, data, bss - if any of these set, tells where symbol resides
- **abs** - absolute unrelocatable symbol
- **undefined** - symbol not defined in this module. 

# ELF 
Superseded a.out for $*nix$ systems. 
ELF file, varying format can be relocatable files, executable files, or shared objects.

Unusual dual nature. Treated as set of logical sections by compilers, assemblers, linkers, described by a section header table.

Loader treats file as set of segments, described by program header table. A single segment usually consists of several sections. 

Relocaable files have section header tables, executable files have program header tables, shared objects have both. Sections intended for further processing by linker, segments intended to be mapped into memory. 

![[Pasted image 20251027002523.png]]
## ELF Header
In all types, there is leading ELF header, designed to be decodable regardless of endianness. 
![[Pasted image 20251027172335.png]]
`e_ident[16]` contains magic identification number, contains `EI_CLASS` denoting word size, `EI_DATA` denoting endianness, `EI_VERSION` denoting ELF version, `EI_OSABI` etc. Everything 1 byte each, endianness doesnt affect; linker parses and determines how to intepret following data. Bytes 9 to 16 are padding. 



## Relocatable
A relocatable (or shared object) file is considered to be a collection of sections, defined according to section header table, containing necessary data to combine file with other object files to form final executable. Table is array of `Elf32_Shdr`.

ELF header entry `e_shoff`, byte offset from beginning of file to section header  table, `e_shentsize` size in bytes of each entry, `e_shnum` tells how many entries in table. 

### Section Header
Defined as follows:

| SH field       | Description                                                                                                                                                                                                   |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sh_name`      | Specifies the name of the section, given as an index into the section header string table section, yielding a NUL-terminated string.                                                                          |
| `sh_type`      | Categorises the sections contents and semantics.                                                                                                                                                              |
| `sh_flags`     | Holds 1 bit flags describing miscellaneous attributes e.g., occupies memory during process execution, special ordering requirements for link editors, data in section may be merged to eliminate duplication. |
| `sh_addr`      | If the section appears in the process's memory image i.e., is to be loaded, this member gives the virtual address where the first byte should reside.                                                         |
| `sh_offset`    | The byte offset from the beginning of the file to the first byte in the section.                                                                                                                              |
| `sh_size`      | Section's size in bytes                                                                                                                                                                                       |
| `sh_addralign` | Specifies address alignment constraints; the value of `sh_addr` must be congruent to 0, modulo this alignment value.                                                                                          |
| `sh_entsize`   | gives size in bytes of each entry if section holds a table of fixed-size entries, such as a symbol table. Contains 0 if no table.                                                                             |
#### Section types `sh_type`
Categorise semantics of a given sections contents, appear in its header.
Common types:

| Section Type            | Description                                                                                                                                                                                                               |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SHT_NULL`              | Marks section header as inactive; does not have associated section.                                                                                                                                                       |
| `SHT_PROGBITS`          | The section holds information defined by the program, such as executable code, read-only data; format and meaning determined solely by the program.                                                                       |
| `SHT_SYMTAB/SHT_DYNSYM` | These sections hold a symbol table. Typically, `SHT_SYMTAB` provides symbols for link editing. Consequently, an object file may also contain a `SHT_DYNSYM` section, which holds a minimal set of dynamic linking symbol. |
| `SHT_STRTAB`            | The section holds a string table. An object file may have multiple string table sections.                                                                                                                                 |
| `SHT_RELA`              | Section holds relocation entries with explicit addends. An object file may have multiple relocation sections.                                                                                                             |
| `SHT_DYNAMIC`           | Section holds information for dynamic linking.                                                                                                                                                                            |
| `SHT_NOTE`              | Section holds information that marks the file in some way.                                                                                                                                                                |
| `SHT_NOBITS`            | A section of this type occupies no space in the file, but otherwise resembles `SHT_PROGBITS`. The `sh_offset` field in header contains conceptual offset from beginning of file, though occupies no bytes.                |
| `SHT_REL`               | Section holds relocation entries without explicit addends.                                                                                                                                                                |
| `SHT_INIT_ARRAY`        | This section contains an array of pointers to initialisation functions. Each pointer in the array is taken as a parameterless procedure with a void return.                                                               |
| `SHT_FINI_ARRAY`        | Section contains an array of pointers to termination functions. Each pointer in the array is taken as a parameterless procedure with a void return.                                                                       |
| `SHT_PREINIT_ARRAY`     | Section contains an array of pointers to functions that are invoked before all other initialisation functions. Each pointer in the array taken as a parameterless procedure with a void return.                           |
| `SHT_GROUP`             | Defines a section group; a set of sections that are related and must be treated specially by the linker. Only appear in relocatable objects, that is, objects with the ELF header member `e_type` set to `ET_REL`.        |
##### Relocation types: `Rel` vs `Rela`
REL entries within `.rel{name}` section of type `SHT_REL` are relocation entries without explicit addends. Standard struct:
```C
typedef struct {
	Elf32_Addr r_offset;
	Elf32_Word r_info;
} Elf32_Rel;
```
`r_offset` Gives location at which to apply relocation action. For relocatable file, this is offset from beginning of section to unit affected by relocation. For executable or shared object, value is virtual address.

`r_info` Gives both symbol table index with respect to which relocation must be made, and type of relocation to apply. E.g., call instruction relocation entry hold symbol table index of function being called. Relocation types are processor specific(ARM7).

```C
typedef struct {
	Elf32_Addr  r_offset;
	Elf32_Word  r_info;
	Elf32_Sword r_addend;
} Elf32_Rela;
```
`r_addend` - Specifies constant used to compute value to be stored into relocatable field. 

Typical application of ELF relocation, determine referenced symbol value, extract addend(either from field to be relocated or addend field in relocation record), apply expression implied by relocation type, extract desired portion of result, place in field to be relocated. 
#### Section flags `sh_flag`
Member of section header of type `Elf32_Word`, contains 1 bit flags describing attributes of given section:


| Flag             | Value  | Description                                                                                                                          |
| ---------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| `SHF_WRITE`      | `0x1`  | Section contains data that should be writable during process execution                                                               |
| `SHF_ALLOC`      | `0x2`  | Section occupies memory during process execution. Some control sections do without this flag thus do not reside in the memory image. |
| `SHF_EXECINSTR`  | `0x4`  | Section contains executable machine instructions.                                                                                    |
| `SHF_MERGE`      | `0x10` | The data in this section may be merged to eliminate duplication.                                                                     |
| `SHF_STRINGS`    | `0x20` | The data elements consist of null-terminated character strings. This is often used alongside `SHF_MERGE`                             |
| `SHF_LINK_ORDER` | `0x40` | Imposes special ordering requirements for link editors if the section is combined with others.                                       |
| `SHF_GROUP`      | —      | Indices the section is a member of a section group.                                                                                  |

If the flag bit is set in `sh_flags` the attribute is 'on' for that section. 

### Sections
In a relocatable elf file `e_type = ET_REL`, a section contains all the information in the file bar the ELF header, section table header, and optional program header table.

Each section occupies one non-overlapping contiguous sequence of bytes within the file.

Every section in an object file has exactly one section header describing it in the section header table.

Various sections hold program and control info. Following detials sections used by system, types and attributes noted:


| Section                       | Description                                                                                                                                                                                                                                                      | Type             | Flags                           |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- | ------------------------------- |
| `.bss`                        | Holds uninitialised data that contributes to the program's memory image. Occupies no file space.                                                                                                                                                                 | `SHT_NOBITS`     | `SHF_ALLOC`<br>+`SHF_WRITE`     |
| `.comment`                    | Holds version control information.                                                                                                                                                                                                                               | `SHT_PROGBITS`   | none                            |
| `.data/.data1`                | Hold initialised data that contribute to program memory image.                                                                                                                                                                                                   | `SHT_PROGBITS`   | `SHF_ALLOC`<br>+`SHF_WRITE`     |
| `.text/1`                     | Holds the executable instructions of a program.                                                                                                                                                                                                                  | `SHT_PROGBITS`   | `SHF_ALLOC`<br>+`SHF_EXECINSTR` |
| `.debug`                      | Holds info for symbolic debugging.                                                                                                                                                                                                                               | `SHT_PROGBITS`   | none                            |
| `.rodata/1`                   | Hold read only data that typically contribute to non-writable segment in process image.                                                                                                                                                                          | `SHT_PROGBITS`   | `SHF_ALLOC`                     |
| `.rel{name}`<br>`.rela{name}` | Hold relocation information as defined by `Elf32_Rel`. If file has a loadable segment that includes relocation, section's attribute will include `SHF_ALLOC` bit; otherwise off. Conventionally, name is supplied by the section to which the relocations apply. | `SHT_REL/A`<br>  | `SHF_ALLOC` (see desc)          |
| `.interp`                     | Holds path name of program interpreter.                                                                                                                                                                                                                          | `SHT_PROGBITS`   | `SHF_ALLOC`, depends            |
| `.symtab`                     | Holds `Elf32_Sym` table providing symbol information for link editing. If file had loadable segment that includes `.symtab`, sections attributes will include `SHF_ALLOC bit`; otherwise off.                                                                    | `SHT_SYMTAB`     | `SHF_ALLOC`, (see desc)         |
| `.dynsym`                     | Holds dynamic linking symbol table.                                                                                                                                                                                                                              | `SHT_DYNSYM`     | `SHF_ALLOC`                     |
| `.strtab`                     | Holds strings, most commonly the strings that will represent the names associated with symbol table entries. If the file has a loadable segment that includes `.strtab`, section's attributes will include `SHF_ALLOC` bit, otherwise off.                       | `SHT_STRTAB`     | `SHF_ALLOC` (see desc)          |
| `.dynstr`                     | Holds strings needed for dynamic linking, most commonly strings that represent the names associated with symbol table entries.                                                                                                                                   | `SHT_STRTAB`     | `SHF_ALLOC`                     |
| `.got`                        | Holds the global offset table.                                                                                                                                                                                                                                   | `SHT_PROGBITS`   |                                 |
| `.init`                       | Holds executable instructions that contribute to process initialisation code. When program runs, system arranges to execute code in `.init` before calling entry point.                                                                                          | `SHT_PROGBITS`   | `SHF_ALLOC`<br>+`SHF_EXECINSTR` |
| `.init_array`                 | Holds array of function pointers that contribute to a single initialisation array.                                                                                                                                                                               | `SHT_INIT_ARRAY` | `SHF_ALLOC` + `SHF_WRITE`       |
| `.fini`                       | Holds executable instructions that contribute to process termination code. When program exits normally, system arranges to execute code in this section afterwards.                                                                                              | `SHT_PROGBITS`   | `SHF_ALLOC`<br>+`SHF_EXECINSTR` |
| `.fini_array`                 | Holds array of function pointers that contribute to single termination array for the executable or shared object containing the section within a segment.                                                                                                        | `SHT_FINI_ARRAY` | `SHF_ALLOC` + `SHF_WRITE`       |
| `.line`                       | Holds line number information for symbolic debugging.                                                                                                                                                                                                            | `SHT_PROGBITS`   | none                            |
`.interp` in practice used to call run-time dynamic linker to load the program and link in any required shared libraries.

#### Symbol Table
Similar to `a.out` symbol table, consists of array of entries `Elf32_Sym`
```C
typedef struct {
	Elf32_Word    st_name;
	Elf32_Addr    st_value;
	Elf32_Word    st_size;
	unsigned char st_info;
	unsigned char st_other;
	Elf32_Half    st_shndx;
} Elf32_Sym;
```

| Field      | Description                                                                                                                                                                                                                                                                                            |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `st_name`  | Holds index into object file symbol string table - yields NUL-Terminated representation of symbol name.                                                                                                                                                                                                |
| `st_value` | Gives value of associated symbol; depending on context, may be absolute value, an address etc. Holds file interpretation in linkable format, holds memory interpretation/virtual address in executable format.                                                                                         |
| `st_size`  | Gives size of associated symbol; 0 if no size or unknown size                                                                                                                                                                                                                                          |
| `st_info`  | Specifies symbol's type and binding attributes; type is normally a data object(`STT_OBJECT`) or function(`STT_FUNC`). A binding determines object file scope. Local = visible to object file only, global = visible to all, weak resemble global symbols, but their definitions have lower precedence. |
| `st_other` | Specifies symbol visibility.                                                                                                                                                                                                                                                                           |
| `st_shndx` | Every symbol table entry defined in relation to some section; this member holds the relevant section header table index.                                                                                                                                                                               |
#### Section arrangement in file
![[Pasted image 20251027220549.png]]

## Executable ELF files
`ET_EXEC`, another type of object file specified by ELF format. Holds a program ready for execution, specifies how system creates process image, primarily uses execution view. 

Contains **program header table**; array of `Elf32_Phdr` structures with each describing segment or other info system needs to prepare program for execution. Only meaningful for executable and shared object files. ELF file specifies sizes via `e_phentsize` and `ephnum`. 

An object file segment contains one or more sections as described by a section header table. 
	![[Pasted image 20251027225035.png]]

### Program Header
```C
typedef struct {
	Elf32_Word	p_type;
	Elf32_Off	p_offset;
	Elf32_Addr	p_vaddr;
	Elf32_Addr	p_paddr;
	Elf32_Word	p_filesz;
	Elf32_Word	p_memsz;
	Elf32_Word	p_flags;
	Elf32_Word	p_align;
} Elf32_Phdr;
```


| Field      | Description                                                                                                                                                                                                                                                                                                                                                            |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `p_type`   | Denotes what kind of segment this array element describes.                                                                                                                                                                                                                                                                                                             |
| `p_offset` | Gives the offset from the beginning of the file at which the first byte of the segment resides.                                                                                                                                                                                                                                                                        |
| `p_vaddr`  | This member gives the virtual address at which the first byte of the segment resides in memory. Where to map segment.                                                                                                                                                                                                                                                  |
| `p_paddr`  | On systems for which physical addressing is relevant, this member is reserved for the segment's physical address.                                                                                                                                                                                                                                                      |
| `p_filesz` | This member gives the number of bytes in the file image of the segment; it may be 0.                                                                                                                                                                                                                                                                                   |
| `p_memsz`  | This member gives the number of bytes in the memory image of the segment; it may be 0.                                                                                                                                                                                                                                                                                 |
| `p_flags`  | This member gives flags relevant to the segment.                                                                                                                                                                                                                                                                                                                       |
| `p_align`  | This member gives the value to which the segments are aligned in memory and in the file. Values 0 and 1 mean no alignment is required. Otherwise, should be a positive power of 2, and `p_vaddr` should equal `p_offset` modulo `p_align`. A segment can start and end at arbitrary file offsets, but the virtual starting address is subject to the above constraint. |
#### Segment Types
| Name         | Value | Description                                                                                                                                                                                                                                                                                         |
| ------------ | ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `PT_NULL`    | 0     | Array element is unused; lets program header table have ignored entries.                                                                                                                                                                                                                            |
| `PT_LOAD`    | 1     | Specifies loadable segment, described by `p_filesz` and `p_memsz`. Bytes from the file are memory mapped to the beginning of the memory segment. If the segments memory size is larger than the file size, the extra bytes are defined to hold the value and follow the segment's initialised area. |
| `PT_DYNAMIC` | 2     | Specifies dynamic linking information.                                                                                                                                                                                                                                                              |
| `PT_INTERP`  | 3     | Specifies location and size of null-terminated path to invoke an interpreter. If present, must precede any loadable segment entry, to load this instead of main program.                                                                                                                            |
| `PT_NOTE`    | 4     | Specifies location and size of auxilary info.                                                                                                                                                                                                                                                       |
| `PT_SHLIB`   | 5     | Reserved but has unspecified semantics.                                                                                                                                                                                                                                                             |
| `PT_PHDR`    | 6     | Specifies location and size of program header table itself; segment type may not occur more than once in a file.                                                                                                                                                                                    |
#### Loading process
Execution view segments arranged such that all loadable sections packed into appropriate segments to minimise loading operations. E.g., mapped text segment has ELF header, program header, read only-text.

Exec files, almost all relocation and symbols resolved, apart from dynamic. Linked to load at fixed addresses in simple case, thus relocation unnecessary unless rely on PIC code or dynamic linking. 

Segment can start and end at arbitrary file offsets, but virtual starting address is subject to following constraint. `p_vaddr = (p_offset % p_align).`

File chunks are memory mapped, system read program header, map file pages associated with loadable segments into process' address space according to `p_vaddr` and `p_memsz`, with appropriate perms for mapped ranges of pages e.g., map text and read-only data as RO, read/write data as copy-on-write. 

May be overlap between contiguous segments on file, when mapped into process image, saves space and preserves alignment e.g., map .rodata if small into last .text segment page prior to load by assigning `p_vaddr` that reflects this. 

The BSS section holds uninitialised data that should be initialised to zero by the system when the program begins to run. Occupies memory(`SHF_ALLOC`) but uses no space in the file itself (`SHT_NOBITS`). BSS is logically contiguous with read/write section in data segment, begins immediately after last byte of initialised data.  Writable data segments typically mapped copy-on-write by OS. BSS is initialised when program starts, writes zeros to this segment, thus triggers COW system, making private copy of that page for the process, ensuring process can write data without corrupting the shared file-backed memory page used by other processes.

## ELF Shared Object
Contains all baggage of relocatable view and executable view of ELF file. Since intended to be run, must contain program header table. Since intended to be linked with other files, it holds relocation information, symbol tables, other data, defined according to section header table.

Thus has ELF header, program header table, loadable segments(sections(unsure if coalesced yet - probably maintains view of both)), non-loadable information e.g., `.symtab` and section header table. Usually ordered like this.

# ARM7TDMI 
The ARM7TDMI architecture imposes specific restrictions on the processing i.e., linking and loading of ELF object files. In $*nix$ environments such as $NetBSD$ and $Linux$, special requirements concerning instruction set support, addressing constraints, alignment, so on and so forth must be reflected in the ELF structures used by linkers and loaders.

## ELF Header
`Elf32_Ehdr ` modified according to architecture.
The  `e_machine ` member must specify  `EM_ARM(decimal 40)` to identify required architecture. 

ARM7TDMI is a 32 bit microprocessor must reflect this by setting:
	`e_ident[EI_CLASS] ` to  `ELFCLASS32 `

ARM7TDMI is bi-endian, swapped according to hardware signal  `BIGEND`, according to whether this signal is high or low, words in memory are treated as Little endian or Big Endian, respectively. The ELF format is designed to be decodable regardless of target endianness due to  `e_ident[16]`.  `e_ident[EI_DATA] ` is set to  `ELFDATA2LSB ` when the target is set to  `little-endian` or  `ELFDATA2MSB` if the target is big-endian. Parsed to determine how to interpret rest of header.
## Linking and Relocation
ARM7TDMI implements both 32 bit ARM instruction set and the 16 bit Thumb instruction set. 

ELF relocation entries must account for this in the type of relocation implied; relocation types are thus instruction set specific with respect to Thumb and ARM modes.

ELF notes two sorts of relocation directives, `Elf32_Rel` and`Elf32_Rela`, each residing in the corresponding `.rel{name}` and `rela{name}` sections at link time, respectively. 

In the `r_info` member of the `Elf32_Rel/a` structures, the relocation type implied by the relocation entry is designated. ARM specifies a set of processor relocation types that can be used. 
![[Pasted image 20251030121933.png]]
Fields being relocated, especially those concerning instructions and data, must conform to alignment rules. ARM instructions are aligned to 4 bytes, and Thumb instructions are 2 byte aligned. Where the relocation action is applied `r_offset`, fields of 8, 16, and 32 bits are aligned on 1, 2, and 4 byte boundaries, respectively. 

$A$  denotes the addend
$P$ denotes the place(offset)
$S$ denotes the value of the symbol whose index is given in `r_info`

For example, type 1, `R_ARM_PC24` is used for 32 bit branch or branch with link instructions, where the offset is typically PC relative. 

The offset would point to the start of the branch instruction, the info would contain the symbol index, and the relocation type `R_ARM_PC24`. We are to relocate the address field in this instruction, via $S-P + A$, yielding a byte offset which is stored in the instruction as a signed offset, which is then right shifted to be divided into a word offset, and placed into the 24 bit field. The necessary pipeline adjustment (often +8 bytes) in ARM state is often factored into the final encoded offset value by the hardware at runtime. If the calculated offset exceeds the range representable by the 24 bit address field, the relocation will fail.  

A 32-bit word commonly requiring relocation is a **data** pointer or address constant stored within the `.data` section, which is typically handled by the relocation type `R_ARM_ABS32`. This process patches the 32 bit memory location with the calculated absolute virtual address (S+A) of a referenced symbol, ensuring the pointer accurately points to the symbol's final location. 

A thumb state relocation can get slightly more complicated. 
`R_ARM_THM_PC22` is a relocation type targeting a thumb long branch with link `BL`. This operation consists of two consecutive 16 bit instructions. Thumb instructions are obviously constrained by their size, thus to address a distant target, the full bit offset (22 bits) is split across both instruction words. The linker computation (S - P+A) determines total offset needed, but the application of the relocation requires extracting the 11 most significant bits and encoding them into bits 0-10 of the first instruction word, and encoding the least 11 significant bits into bits 0-10 of the next instruction word in the pair. 

A literal pool is a block of constants embedded in .text used by ARM and Thumb instructions that can only load PC-relative data. 
ARM's `LDR Rd, [PC, #imm12]` can reach roughly -/+4KB (byte offsets in PC relative addressing instructions). Thus the constant must be stored closed to the constant if too large to fit in the offset. 


One must implement veneer generation, a trampoline stub inserted when a branch cannot reach its target and in other situations. 

    BL far_function

    BL veneer_for_far_function
    veneer_for_far_function:
    LDR pc, =far_function
Linker may also generate veneer if object A (arm) calls object B(thumb), executes BX target with correct low bit set, since a plain BL cannot change mode. 



### Specific Constraints
The ARM ELF consumer does not need to interpret the instruction word to determine how to relocate it; the required subfield and unit of relocation evident from relocation entry and relocation type. 

Multiple relocation of a field is possible, but should not be exploited to generate a compound relocation at link-time as an intermediate step may overflow, cannot clip the result either, less the semantics of the program will be violated.


## Loading and Alignment and Memory protection
Enforces alignment for all memory accesses at hardware level, which translates into constraints enforced by ELF files & section headers.

Words aligned to 4 bytes, halfwords aligned to 2 bytes, byte quantities on any byte boundaries. 

Sections are configured accordingly in `Elf32_Shdr`. `sh_addralign` must be a positive integral power of two for sections containing loadable(`SHF_ALLOC`). The sections virtual address must be congruent to 0, modulo the alignment value. 

For statically linked ARM executables, the program header describes segments where the alignment must be a power of 2 greater than or equal to 4, as all ARM and Thumb segments are at least word-aligned. 

Configure instruction and data fetching via the $MAS[1:0]$ bus. ARM state signals word accesses, Thumb state signals halfword accesses, $TBIT$ signals operating state. 

The Thumb mode bit handling is a critical interoperability point. The LSB of a function pointer at runtime is often used to indicate Thumb state when branching via registers; this LSB is best maintained accordingly in `st_value`, and ensure it is preserved during relocations. 
## Symbol management
`.symtab` or `.dynsm` consist of `Elf32_Sym` entries. `st_value` holds the value of associated symbol, either virtual address or section offset.
`st_info` provides symbol classification i.e., type, and linkage visibility (binding). 
	type = symbols typically classified as data object `STT_OBJECT` or a function `STT_FUNC`.
	binding = visibility, `STB_LOCAL`, `STB_GLOBAL`, or `STB_WEAK` representing global symbols with lower definition precedence to global.
Index 0 designates the undefined symbol index. Relocation entries refer to a symbol table index to retrieve `st_value`, if this index is `STN_UNDEF(0)` then the relocation uses 0 as the symbol value. 
`st_size` gives the size of the associated symbol. 



# Exercises
