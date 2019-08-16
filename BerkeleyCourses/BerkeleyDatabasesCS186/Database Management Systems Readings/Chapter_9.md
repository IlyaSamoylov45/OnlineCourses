Chapter 9.1 THE MEMORY HIERARCHY
  - Memory in a computer system is arranged in a hierarchy
  - At the top, we have primary storage, which consists of cache and main memory and provides very fast access to data.
  - Then comes secondary storage, which consists of slower devices, such as magnetic disks.
  - Tertiary storage is the slowest class of storage devices; for example, optical disks and tapes.
  - Since buying enough main memory to store all data is prohibitively expensive, we must store data on tapes and disks and build database systems that can retrieve data from lower levels of the memory hierarchy into main memory as needed for processing.
  - Data must be maintained across program executions. This requires storage devices that retain information when the computer is restarted (after a shutdown or a crash); we call such storage nonvolatile.
  - Primary storage is usually volatile (although it is possible to make it nonvolatile by adding a battery backup feature), whereas secondary and tertiary storage are nonvolatile.
  - The main drawback of tapes is that they are sequential access devices. We must essentially step through all the data in order and cannot directly access a given location on tape.

Chapter 9.1.1 Magnetic Disks
  - Magnetic disks support direct access to a desired location and are widely used for database applications.
  - Data is stored on disk in units called disk blocks. A disk block is a contiguous sequence of bytes and is the unit in which data is written to a disk and read from a disk.
  - Blocks are arranged in concentric rings called tracks, on one or more platters. Tracks can be recorded on one or both surfaces of a platter; we refer to platters as single-sided or double-sided, accordingly.
  - The set of all tracks with the same diameter is called a cylinder, because the space occupied by these tracks is shaped like a cylinder; a cylinder contains one track per platter surface.
  - Each track is divided into arcs, called sectors, whose size is a characteristic of the disk and cannot be changed.
  - An array of disk heads, one per recorded surface, is moved as a unit; when one head is positioned over a block, the other heads are in identical positions with respect to their platters. To read or write a block, a disk head must be positioned on top of the block.
  - Current systems typically allow at most one disk head to read or write at any one time. All the disk heads cannot read or write in parallel--this technique would increase data transfer rates by a factor equal to the number of disk heads and considerably speed up sequential scans. The reason they cannot is that it is very difficult to ensure that all the heads are perfectly aligned on the corresponding tracks.
  - A disk controller interfaces a disk drive to the computer. It implements commands to read or write a sector by moving the arm assembly and transferring data to and from the disk surfaces.
  - A checksum is computed for when data is written to a sector and stored with the sector. The checksum is computed again when the data on the sector is read back.
  - While direct access to any desired location in main memory takes approximately the same time, determining the time to access a location on disk is more complicated.
  - The time to access a disk block has several components.
    - Seek time is the time taken to move the disk heads to the track on which a desired block is located. As the size of a platter decreases, seek times also decrease, since we have to move a disk head a shorter distance.
    - Rotational delay is the waiting time for the desired block to rotate under the disk head; it is the time required for half a rotation all average and is usually less than seek time.
    - Transfer time is the time to actually read or write the data in the block once the head is positioned, that is, the time for the disk to rotate over the block.

Chapter 9.1.2 Performance Implications of Disk Structure
  - Performance Implications of Disk Structure:
    - Data must be in memory for the DBMS to operate on it.
    - The unit for data transfer between disk and main memory is a block; if a single item on a block is needed, the entire block is transferred. Reading or writing a disk block is called an I/O (for input/output) operation.
    - The time to read or write a block varies, depending on the location of the data: ```access time = seek time + rotational delay + transfer time```
  - These observations imply that the time taken for database operations is affected significantly by how data is stored on disks.
  - In current disk designs, all the data on a track can be read or written in one revolution. After a track is read or written, another disk head becomes active, and another track in the same cylinder is read or written.
  - Sequential access minimizes seek time and rotational delay and is much faster than random access.
