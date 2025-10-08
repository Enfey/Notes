## Rotational and solid state drives
### Rotational hard drives
- Made of aluminium/glass platters covered with magnetisable material
	- Read/heads fly just above the surface, connected to single disk arm controlled by single actuator
	- Data is stored on both sides
	- Common diameers range from 1.8 to 3.5 inches
	- They rotate at constant speed
- Disk controller abstracts this low level interface
- Rotational hard drives are approx 4 orders of magnitude slower than main memory
![[Pasted image 20250116192935.png]]
#### Low level format
Organised in terms of:
- Tracks - a concentric circle on a single platter side
- Sectors - segments of a track, usually 512B or 4KB in size, usually equal number of bytes, containing (preamble, data, error correcting code (ECC))![[Pasted image 20250116193235.png]]
- Cylinders - a group of tracks that are in the same position on each plater, relative to the spindle
The number of sectors increases from the inner side of the disk to the outside, due to increase in circumference. 
![[Pasted image 20250116193338.png]]
Cylinder skew is an offset applied to sectors in adjacent tracks. Offset accounts for seek time required to move read/write head between tracks. Without this adjustment, data transfer may be interrupted, as disk head wouldn't have enough time to align with next sector after moving between tracks. The starting position of sectors is adjusted on adjacent tracks to give head enough time to align with the new track. 
In past disk sectors were interleaved(arranged non-consecutively) to account for transfer time. This was done to allow sufficient time for system to process data from one sector before moving onto next one - consecutive would have been too fast. 


##### Access time
Access time = seek time + rotational delay + transfer time
- Seek time - time needed to move arm to the track(dominant)
- Rotational latency - Time before the sector appears under the head
- Transfer time - time to transfer the data
![[Pasted image 20250116195140.png]]
Multiple requests may be happening at same time(concurrently), thus access time may be increased by a queueing time
Dominance of seek time leaves room for optimisation by carefully considering order of read operations
![[Pasted image 20250116195550.png]]
###### Seek time
Estimated seek time to move the arm from one track to another is approximated by 
- Ts = n x m + s
- n =  number of tracks to be crossed
- m = crossing time per track
- s = any additional startup delay
Assume that a disk rotations at 3600 rpm
One rotation takes - 16.7ms (60000/3600)
###### Transfer time
Assume that a disk rotations at 3600 rpm
One rotation takes - 16.7ms (60000/3600)
Average rotational latency = half a rotation on average = 8.3 ms
Let b denote the number of bytes transferred, N the number of bytes per track, and rpm the rotation speed in revolutions per minute
Then transfer time Tt is given by
- N bytes take 1 revolution - bytes per track
	- b contiguous bytes to transfer takes b/n revolutions 
Tt = b/n x ms per revolution
DEAL WITH THIS NEXT WEEK
![[Pasted image 20250116210528.png]]

## Access times for hard drives

## Disk scheduling
OS/Hardware must position/organise files and sectors strategically and optimise disk requests to minimise overhead from seek time and rotational delays.
I/O requests happen over time and go through system calls and are queued:
- they are kept in a table of requested sectors per cylinder
- this allows the operating system to intercept and re-sequence them
Disk scheduling algorithms determine the order in which disk requests are processed to minimise overhead and reduce total disk head movement
- Commonly use heuristic approaches, that is, none of the algorithms used are optimal algorithms
**Assume a disk with 36 cylinders numbered 1 to 36**

#### First come, First served
Process the requests in the order that they arrive, not prioritising any specific requests
Consider following sequence of disk requests(cylinder locations)
![[Pasted image 20250116211052.png]]
Total movement  = sum of absolute differences between cylinder positions(as already done some movement)
![[Pasted image 20250116211254.png]]

#### Shortest seek time first
Selects request that is closest to current head position, results in approx. 50% less movement of disk head for the above sequence
![[Pasted image 20250116211742.png]]
Continously arriving requests for the same location could however starve other regions. The arm stays in the middle of the disk in case of heavy load also, ensuring edge cylinders are poorly served.

#### SCAN
Improves disk performance. Aka lift algorithm. Disk head moves in one direction e.g., toward higher cylinder numbers, servicing all pending requests in its path. When it gets to last cylinder, it reverses direction and services all pending requests. The upper limit on 'waiting time' is 2 x the number of cylinders, ensuring no starvation.
![[Pasted image 20250116212404.png]]
![[Pasted image 20250116212415.png]]
The middle cylinders are favoured if the disk is heavily used(max wait time is N tracks, 2N for cylinders on edge). Starvation can occur for requests that consistently fall at extremes depending on direction reversal timing.
##### SCAN variations

###### C-SCAN
Disk head moves in one direction, e.g., from innermost cylinder to outermost cylinder, servicing pending requests. Once it reaches the end, moves back to starting position without servicing any requests during return trip, and then reverses direction again and services requests. Behaves like a circular queue. This is based on the notion that requests at the other end of the disk have been waiting the longest. This algorithm is fairer and equalises response times across a disk in comparison to SCAN. 
###### LOOK SCAN
Similar to SCAN, but instead of moving all way to final/first cylinder, moves to the cylinder containing first/last request (depending direction). Aims to reduce unnecessary head movement.

###### F Scan
Uses two queues, while one is handled with SCAN, the other one is filled, swapped over when all requests serviced.


Disk scheduling algorithm performance is dependent on the requests/load of the disk, one request at a time e.g., FCFS will perform equally as well as any other algorithm. 


### Driver caching
For most modern drives, the time required to seek a cylinder is more than the rotational time(remember pre-paging).
It makes sense, to read more sectors than actually required, read sectors during rotational delay that actually pass by. 
Modern HDD controllers read multiple sectors when asked for the data from one sector, track-at-a-time caching.

## SSD
Advanced secondary storage that use non-volatile flash memory (electric circuits use to store data in blocks). Have no delays due to no moving parts, and can store data using multiple types of NAND flash memory including single-level which stores 1 bit per cell (SLC), Multi-Level cell which stores 2 bits per cell (MLC), triple level cell which store 3 bits per cell, making trade off between speed and capacity. Suffer from wearness and disturbance. ??
Organised in terms of cells, pages, blocks, banks, planes and die, and package, and other components
- Cell - Smallest unit of storage, storing electrical charges/bit, depending on type of NAND flash cell
- Page - smallest unit of data that can be read or written in SSD, typically 4KB to 16KB
	- Data written page by page, but erased by block
		- This is because bits can only be changed from 1 to 0 during write operation, so erase all bits by setting all bits to 1, and then applying voltage to write to a page
- Block - A block is a larger unit that contains multiple pages, e.g., 128 pages per block, typically 256 KB to 4MB
	- Data is erased block by block
- Bank - provide parallelism for memory access by grouping memory elements, typically use multiple
- Volatile Cache - used to accelarate operations by storing frequently accessed data e.g., buffering or mapping tables
- Controller - manages R/W operations, ECC, wear-leveling(ensuring even distribution of writes across memory cells to prolong SSD life)
- Interface - connects SSD to host system
- ![[Pasted image 20250116214632.png]]
**Flash translation layer** provides a logical to physical address mapping, allowing SSD to function like traditional storage device. Maps logical blocks onto physical pages supporting the following operations:
- Read - uniformly random access to any page any location(10s of microseconds)
- Erase - entire blocks containing multiple pages
- Program = wrote a page, block must be erased first
-![[Pasted image 20250116215231.png]]
![[Pasted image 20250116215256.png]]











SOME CONTEXT FOR FTL
Hard drives have typically been the traditional way to store data and use fixed data sizes called sectors at 512 bytes in size. Sectors are located via logical block addresses through a linear addressing methodology that use an integer represented by a hexadecimal number. For example, the first sector is LBA0x0, the second is LBA0x1, and so on. As SSDs and managed flash devices replaced hard drive storage, they needed to support the same traditional addressing schemes for compatibility. NAND flash memory organizes data by rows and columns comprised of blocks, and within a block are several pages. The blocks and pages access the data written to NAND flash memory. Accessing data from a logical block address to a physical block address requires the creation of an entry or record that assigns a logical block address to the NAND block address.
