Libraries are collections of object files that are lazily included in a linked program. Every modern linker enables library support, both static and dynamic. Here we are concerned with traditional statically linked libraries.

## Purpose of libraries
A statically linked library is merely a collection of compiled object files usually with some added metadata bundled into what one calls an archive. 

The purpose of static libraries is to promote modularity and code reuse, without enforcing runtime dependencies. Linker's extract only the object modules that resolve currently unresolved symbols, thus the resultant executable contains all necessary code, becoming self-sufficient by satisfying dependencies early. Static libraries need not be PIC. They contain `ET_REL` ELF files for our purposes; the linker extracts the needed members and permanently relocates them into fixed addresses. 

Dynamic linking is often regarded as superior in multi-program OS targets, as multiple programs often have the same dependencies; we can leverage this to produce smaller binaries, and link-time relinking is not required for changes to `.so` files, whereas static executables would be re-linked to pick up fixes. Although there is the constraint that for dynamically linked libraries, they must be PIC. 

## Static Library Formats
### The Archive Container Format (.a)
This is the static library format using on $*nix$(and also Windows). It can be used for collections of any types of files though in practice it's rarely used outside of this domain.

It should be noted that all modern $*nix$ systems maintain this format although with slight variations - the output of an `ar` call was never standardised.
#### Archive Header.
Every archive begins with the magic ASCII signature:
```C
!<arch>\n // 21 3C 61 72 63 68 3E 0A
```
Which marks the file as an archive.

#### Archive Member Headers
Each archive member is preceded by a 60-bye fixed-width ASCII header.
```c
char name[16]; // Member name (space-padded or '/' offset)
char modtime[12] // Decimal alteration time (sec since 1970)
char uid[6]; // Decimal user ID
char gid[6]; // Decimal group ID
char mode[8]; // Octal file mode (permissions)
char size[10]; // Decimal file size in bytes
char eol[2]; // backtick + newline
```
Each field is ASCII to keep archives printable and endianness agnostic. 
The member's contents immediately follow according to `size`.

If `size` is odd, a newline padding byte follows the member's contents such that the next header starts on an even boundary. There is no requirement that the member's contents start on any particular boundary. 

The UID, GID, mode, time fields are preserved for completeness but have no linking relevance. 

#### Member names and the `//` long-name table
The name field is 16 bytes. Historically older systems limited filenames to 14 characters so this was fine. Modern systems provision a special member to store member names longer than 16 characters. 

The `//` member, the long-name table:
- If a name $\leq$ 16 characters, store directly in member header (padded)
- If longer, the name field holds `/NNN` where `NNN` is a *decimal offset* into `//`
- The `//` member stores all long filenames concatenated, separated by `/\n`($*nix$) or `\0`(windows).
The member need not exist on $*nix$ systems oftentimes, provided there are no long names, but typically placed after the **symbol directory**if it is required. 

#### Symbol Directory (and its variants)

#### Using AR

## Searching Libraries

## Performance issues

## Weak External Symbols


## Linked Behaviour With Archive Files Factored In