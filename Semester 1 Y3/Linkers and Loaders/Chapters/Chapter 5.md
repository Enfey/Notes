# Symbol Management
Symbol management is a linker's key function, without some way to let modules refer to one another, there wouldn't much use for a linker's other facilities. 

## Binding and Name resolution
Linkers handle a variety of kinds of symbols, all linkers handle symbolic references from one module to another. Each input module includes a symbol table, often with regard to lexical scoping, this becomes a sheaf of tables. 

The symbols include the following:
- Global symbols defined and perhaps referenced within the module.
- Global symbols referenced but not defined in the module.
- Segment names which are also usually considered to be global symbols defined to be at the beginning of the segment. 
- Nonglobal symbols, usually for debuggers and crash dump analysis, aren't really needed for linking process.
- Line number information, tell source language debuggers correspondence between source lines and object code. 