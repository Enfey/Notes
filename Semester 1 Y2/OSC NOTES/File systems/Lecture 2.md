## File systems
- File system abstraction - logical file system is mapped onto physical arrangement of data, abstracting from physical level
- Abstraction from device - uniform view of very different underlying storage mechanism
- Concurrency - what if multiple processes access same file simulatenously
- Security - why is access denied
Key file system implementation:
1. How are drives layed out, modelled and used
2. How are (hierarchical) file systems implemented
3. How are files implemented? (mapped onto data blocks)
Key file system functionality:
- Store
- Locate
- Retrieve
- Allocate/reclaim storage
- Lower level interfaces
## Low level implementation - layers of abstraction
Shared layers 
- I/O control interacts with the device controller/registers (ISRs, device drivers)
- Basic file system instructs device drivers in 'blocks', schedules I/O, and manages buffers and caches for data
File system specific layers(different implementations possible)
- File organisations models logical blocks?? for files and free space(bitmap, linked list)
- Logical file system manages file control blocks, directory structures, and protection
Application programs define the structure of files
![[Pasted image 20250117042222.png]]

## Disk layout
Divided into logical blocks 512 bytes to 4KB, logical file system needs to be superimposed on physical device.

## Drive layout
Master boot record is located at start of the drive
- Used to boot the computer (BIOS reads and executes boot sector)
- Contains partition table at its end with active partition
- One partition is listed as active containing a boot block to load operating system
Drive commonly split into multiple partitions, a different file/OS may exist on each partition, and exact layout depends on file syste,, but some common blocks are included in partition.

### Partition layout
- Boot block - containing code to boot OS (for every partition irrespective of containing an OS)
- Super block (master file table) - containing stats about the partition(partition size, number of FCBs, free space, block size)
- Free space management - contains data structure to indicate free FCBs or data blocks
- Meta data or File control blocks e.g., i-nodes
- Data blocks - including root directory
- ![[Pasted image 20250117042756.png]]

## Boot process
1. BIOS loads MBR from first physical hard drive
2. MBR reads partition table on same hard drive as the MBR
3. MBR looks for primary partition as active(one has to be declared as active)
4. MBR loads the boot block from active primary partition
5. Boot block loads rest of OS etc

## File system hierarchy
- Single level
- Dual level
- Tree structure
- Acyclic graphs using links
### Directories
Directories are special files (at least on implementation level) that group files together and of which the structure is defined by the file system.
- A bit is set to indicate they are directories
- They map human readable 'logical' names onto unique identifiers e.g., index for file control blocks that detail physical locations and file attributes
Two approaches exist:
- All attributes are stored in the directory file(e.g., file name, disk address - windows)
	- ![[Pasted image 20250117043316.png]]
- A pointer to the data structure that contains the file attributes(unix)
	- ![[Pasted image 20250117043325.png]]
Directories are manipulated using system calls, similar to files.
- create/delete - new directory is created/deleted
- opendir, closedir - add/free directory from internal tables?
- readdir - return next entry in directory file
- rename, link, unlink, list-update
Common operations include creating deleting ,searching, listing, traversing
Directories can be implemented as linear lists, sorted lists, hash tables
### Files
Both windows and unix have regular files and directories
- Regular files contain user data in ASCII or binary format(well defined), format mostly defined by applications unless executable
- Directories group files together but are files on an implementation level
Unix also has character and block special files:
- Character special files are used to model serial I/O devices e.g., keyboards
- Block special files are used to model devices which handle data in blocks rather than bytes e.g., drives.
Files are sequential(read and written in order) random (direct ) access, indexed access (notes on this)

File control blocks are kernel level data structure, used to store information about a file. Contains name of file, type of file, size of file in bytes, pointer to logical file location, access permissions, timestamp, other attributes such as ownership.
- Allowing user applications to access them directly could compromise their integrity
- System calls enable user application to ask OS to carry out operation on its behalf
May reside in memory, kept in the per process and system wide open file table, an array of FCBs which retains a list of all currently open files, indexed using a file handle.
Sys calls for file manipulation:
- Open()
	1. Maps logical name onto numeric low level name identifying file control block
	2. Retrieves FCB from drive
	3. Adds it to process/system open file table, incrementing reference count
	4. Returns a process specific file handle(index into process file table)
- Close()
	1. Decrements reference count(number of processes accessing that file)
	2. Synchronise FCB with disk
	3. Removes FCB from process/file system tables (when ref count = 0)
Per process file table contains process specific information
- All files currently open to the process
- File handle points to index in process file table
- Read/write/current pointers
- Reference to relevant entry in system wide file table
System wide file table contains general information
- One entry per open file
- Location on disk
- Access times
- Reference count
- Indirectly references FCB
![[Pasted image 20250117045037.png]]
![[Pasted image 20250117045048.png]]
![[Pasted image 20250117045106.png]]


## FCB
Data structure in file system containing metadata about a file, stored on disk and loaded into memory when file opened.
Includes:
- File name
- File size
- File type
- RWX
- File owner and group
- Creation, access and modification timestamps
- Disk location of file