## System calls
FILE DESCRIPTORS ARE UNIQUE WITHIN A SINGLE PROCESS.
 - Open()
	 1. Maps logical name onto numeric low level name identifying file control block
	2. Retrieves FCB from drive
	3. Adds it to process/system open file table, incrementing reference count
	4. Returns a process specific file handle(index into process file table)
	5. If file found in system wide file table, link per process entry to system wide file table
 - Close
	 - Closes an open file descriptor, removing its entry from the per-process file table
	 - Decrements reference count in system wide file table
	 - Returns 0 on success

## File access
Files are made of up of smaller 'blocks', smallest chunk of data can be accessed from file, numbered sequentially. Logical, which is what OS sees. 
### Sequential
Start at block 1, reading block in order until reach desired block. Suitable for text/log files. Important where order of data matters, e.g., batch processing. 

### Random access
Allows direct access to any block without needing to process preceding blocks. More efficient, and useful where data access is not guaranteed to be linear.


## File system implementations
### Contiguous allocation
Contiguous file systems are similar to dynamic partitioning in memory allocation. Each file is stored in single group of adjacent blocks on hard disk. E.g., 1KB blocks, 100KB file, need 100 contiguous blocks. Allocation of free space can be done using algorithms like first fit, best fit, next fit. 

#### Advantages
- Easy to implement - only location of first block, and length of file must be stored in on disk FCB.
- Optimal read/write performance: blocks are clustered, minimises seek time. 
- Used for CD-ROMs/DVDs, since write once, means external fragmentation is less of an issue.

#### Disadvantages
- Exact size of a file not always known beforehand
- Allocation algorithms needed to decide which free blocks to allocate. 
- Deleting file results in external fragmentation, requires de-fragmentation, which is slow. 
### Linked list
To avoid external fragmentation, file stored discontiguously in separate blocks, that are linked to one another. Only address of first is stored in FCB, each block contains a data pointer to next block. Finds free block anywhere in storage, allocates it, and links it to previous block. 
#### Advantages
- Easy to maintain, only first block(address) has to be maintained in FCB.
- Files can grow dynamically, new blocks are appended at the end
- No external fragmentation, just finds free block anywhere, 
- Sequential access is straightforward, more seek operations may be required however, non-contiguous. 
#### Disadvantages
- Internal fragmentation, the last half of the block is unused on average, this reduces for smaller block sizes
- May result in slow disk access, larger blocks containing multiple sectors will improve speed, but increase internal fragmentation. 
- The data within a block is no longer a power of 2, due to the addition of a pointer
- If one block is corrupted, access to the rest of the file is lost. 

### File allocation table
Stores linked-list pointers in separate index table, called File Allocation Table, which is loaded in memory, so that blocks/clusters do not have additional overhead of pointer to next block in chain. 


![[Pasted image 20250123012000.png]]

Keeps track of which clusters are used by which files on disk. E.g., fred.doc starts at block/cluster 9, then uses cluster 16, 17, 24, and 25(EOF) to store complete file. 

#### Advantages
- Block size remains power of 2, no space lost in block due to pointer
- Index table can be kept in memory, allowing random access (table access itself remains sequential)???

#### Disadvantages
- Size of FAT grows with number of blocks/disk size
- Approx. 200 million entries for 200GB disk, with a 1KB block size (800MB at 4 bytes per entry)
### i-nodes
Each file has a small data structure on disk, called i-node(index node)
that contains its attributes and block pointers. 
- In contrast to FAT, an i-node is only loaded when the file is open (stored in system wide open file table)
- If every i-node consists of n bytes, and at most k file can be open at any time, n x k bytes of main memory are required, at most. 
I-nodes are composed of direct block pointers, indirect block pointers or a combination thereof. 
Used in unix-like operating systems to store file metadata. 

#### Directories implementation with i-nodes

#### Lookups

#### Hard and soft links

#### Deleting files

### File system comparison