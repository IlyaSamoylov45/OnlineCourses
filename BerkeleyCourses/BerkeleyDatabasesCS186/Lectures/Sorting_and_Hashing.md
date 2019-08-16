Sorting And Hashing

Reading
  - 9.1, 13.1, 13.2, 13.3, 13.4.2

Why Sort?
  - Rendezvous
    - Eliminating duplicates
    - Summarizing groups of items
  - Ordering
    - Sometimes, output must be ordered
    - E.g. return results in decreasing order of relevance
  - Upcoming fundamentals
    - Sort-merge join algorithm involves sorting (rendezvous)
    - First step in bulk loading tree indexes (ordering)
  - Problem : sort 100 GB of data with 1GB of RAM
    - Why not virtual memory?

But First...
  - Important to know a little something about disks.

Disks and Files
  - A lot of databases still use magnetic disks.
    - Disks are a mechanical anachronism!
  - Major implications!
    - No "pointer derefs". Instead, an API:
      - READ: transfer "page" of data from disk to RAM.
      - WRITE: transfer "page" of data from RAM to disk.
    - Both API calls are expensive
      - Plan carefully!
    - An explicit API can be a good thing
      - Minimizes the kind of pointer errors you see in C.

Economics
  - For $1000 NewEgg offers:
    - 0.125TB of RAM
    - 2.65TB of Solid State Disk
    - 26.5TB of Magnetic Disk

The Storage Hierarchy
  - Main memory (RAM) for currently used data.
  - Disk for main database and backups/logs (secondary & tertiary storage)
  - The role of Flash (SSD) caries by deployment
    - Sometimes the DB
    - Sometimes a cache

Components of a Disk
  - Platters spin (say 7200 rpm)
  - Arm assembly moved in or out to position a head on a desired track
    - Tracks under heads make a cylinder (imaginary)!
  - Only one head reads/writes at any one time.
  - Block/page size is a multiple of (fixed) sector size.

Accessing a Disk Page
  - Time to access (read/write) a disk block:
    - seek time (moving arms to position disk head on track)
      - 2 - 4 msec on average
    - rotational delay (waiting for block to rotate under head)
      - 2 - 4 msec
    - transfer time (actually moving data to/from disk surface)
      - 0.3 msec per 64KB page

Arranging Pages on Disk
  - 'Next' block concept:
    - blocks on same track, followed by
    - blocks on same cylinder, followed by
    - blocks on adjacent cylinder
  - Arrange file pages sequentially on disk
    - minimize seek and rotational delay
  - For a sequential scan, pre-fetch
    - several pages at a time!

Notes on Flash (SSD)
  - Various technologies things still evolving
  - Read is smallish and fast
    - Single read access time: 0.03ms
    - 4KB random reads: about 500MB/sec
    - Sequential reads: about 525MB/sec
  - Write is slower for random
    - Single write access time: 0.03ms
    - 4KB random writes: about 120MB/sec
    - Sequential writes: about 480MB/sec
  - Some concern about write endurance
    - 2K - 3K cycle lifetimes?
    - 6 - 12 months?

Storage Pragmatics & Trends
  - Many significant DBs are not that big.
    - Daily weather, round the globe, 1929 - 2009: 20GB
    - 2000 US Census: 200GB
    - 2009 English Wikipedia: 14GB
  - But data sizes grow faster than Moore's Law
  - What is the role of disk, flash, RAM?
    - The subject of some debate!

Bottom Line (For Now!)
  - Very Large DBs: relatively traditional
    - Disk still the best cost/MB by orders of magnitude
    - SSDs improve performance and performance variance
  - Smaller DB story is changing quickly
    - Entry cost for disk is not cheap, so flash wins at the low end
    - Many interesting databases fit in RAM
  - Change brewing on the HW storage tech side
  - Lots of uncertainty on the SW/usage side
    - It's Big: Can generate and archive data cheaply and easily
    - It's Small: Many rich data sets have (small) fixed size
  - Hmmmmm....!
  - Many people will continue to worry about magnetic disk for some time yet.

Remember this slide?
  - Simple case: "Map".
    - Goal: Compute f(x) for each record write out the result.
    - Challenge minimize RAM, call read/write rarely
  - Approach
    - Read a sizable chunk from INPUT to an Input Buffer
    - Write f(x) for each item to an Output Buffer
    - When Input Buffer is consumed, read another chunk
    - When Output Buffer fills, write it to OUTPUT
  - Reads and writes not coordinated
    - E.g. if f() is Compress(), you read many chunks per write.
    - E.g. if f() is DeCompress(), you write many chunks per read.

Better Approach: Double Buffering
  - Main thread runs f(x) on one pair I/O bufs
  - 2nd I/O thread fills/drains unused I/O bufs
  - Main thread ready for a new buf? Swap!
  - Usable in any of the subsequent discussion
    - Assuming you have RAM buffers to spare!
    - But for simplicity we wont bring this up again!

Sorting & Hashing: Formal Specs
  - Given:
    - A file F:
      - containing a multiset of records R
      - consuming N blocks of storage
    - Two scratch disks
      - each with >> N blocks of free storage
    - A fixed amount of space in RAM
      - memory capacity equivalent to B blocks of disk
    - Sorting
      - Produce an output file Fs
        - with contents R stored in order by a given sorting criterion
    - Hashing
      - Produce an output file Fh
        - with contents R arranged on disk so that no 2 records that are incomparable (i.e. "equal" in sort order) are separated by a greater or smaller record.
        - i.e. matching records are always "stored consecutively" in Fh

Naïve Sorting: 2-way
  - Pass 0 (conquer)
    - read a page, sort it, write it..
    - only one buffer page is used
    - a repeated "batch job"
  - Pass 1, 2, 3, . . ., etc. (merge):
    - requires 3 buffer pages
      - note : this has nothing to do with double buffering
    - merge pairs of runs into twice as long
    - a streaming algorithm, as in the previous slide

Two - Way External Merge Sort
  - Conquer and Merge: sort subfiles and merge
  - each pass we read and write each page in file.
  - N pages in the file. So the number of passes is.
    - logN + 1
  - So the total cost is:
    - 2N[LogN + 1]

General External Merge Sort
  - More than 3 buffer pages. How can we utilize them?
  - To sort a file with N pages using B buffer pages:
    - Pass 0: use B buffer pages. Produce[N/B] sorted runs of B pages each.
    - Pass 1, 2, . . ., etc: merge B-1 runs.

Cost of External Merge Sort
  - Number of passes 1 + [log(B-1)(N/B)]
  - Cost = 2N * (# of passes)
  - E.g. with 5 buffer pages, to sort 108 page file:
    - Pass 0: [108/5] = 22 sorted runs of 5 pages each (last run is only 3 pages)
    - Pass 1: [22/4] = 6 sorted runs of 20 pages each (last run is with only 8 pages)
    - Pass 2: 2 sorted runs, 80 pages and 28 pages
    - Pass 3: Sorted file of 108 pages

Memory Requirement for External Sorting
  - How big of a table can we sort in two passes?
    - Each sorted run after Phase 0 is of size B
    - Can merge up to B-1 sorted runs in Phase 1
  - Answer B(B-1)
    - Sort N pages of data in about sqrt(N) space

Internal Sort
  - Quicksort is a fast way to sort in memory.
  - Alternative "tournament sort"
    - aka "heapsort", "replacement selection"

More on Heapsort
  - Fact: average length of a run 2(B-2)
    - The "snowplow" analogy
  - Worst Case:
    - What is min length of a run?
    - How does this arise?
  - Best Case:
    - What is max length of a run?
    - How does this arise?
  - Quicksort is faster, but . . . longer runs often means fewer passes!

Alternative: Hashing
  - Idea:
    - Many times we dont require order
    - E.g. removing duplicates
    - E.g forming groups
  - Often just need to rendezvous matches
  - Hashing does this
    - And may be cheaper than sorting!
    - But how to do it out-of-core?

Divide
  - Streaming Partition (divide):
    - Use a hash fn hp to stream records to disk partitions
      - All matches rendezvous in the same partition.
      - Streaming alg to create partitions on disk:
        - "Spill" partitions to disk via output buffers
  - ReHash(Conquer)
    - Read partitions into RAM hash table one at a time, using hash fn hf
      - Then go through each bucket of this hash table to achieve rendezvous in RAM.

Cost of External Hashing
  - 4N I/Os

Memory Requirement
  - How big of a table can we hash in two passes?
    - B-1 "partitions" result from Pass 1
    - Each should be no more than B pages in size
    - Answer: B(B-1)
      - We can hash a table of size N pages in about sqrt(N) space

Parallelize me! Hashing
  - Phase 1: partition data across machines
    - Streaming out to network as it is scanned
    - Which machine for this record?
      - Use (yet another) independent hash function hn
  - Receivers proceed with phase 1 as data streams in
    - from local disk and network

Parallelize me! Sorting
  - Pass 0: partition data across machines
    - streaming out of network as it is scanned
    - which machine for this record?
      - check value range (e.g. [-infinity, 10],[11,101],[101, infinity]);
  - Receivers proceed with pass 0
    - as data streams in
  - A wrinkle: how to ensure ranges are the same size?!
    - i.e. avoid data skew?

So which is better??
    - Simplest analysis:
      - Same memory requirements for 2 passes
      - Same I/O cost
      - But we can dig a bit deeper...
    - Sorting pros:
      - Great if input already sorted(or almost sorted) w/ heapsort
      - Great if need output to be sorted anyway
      - Not sensitive to "data skew" or "bad" hash functions
    - Hashing pros:
      - For duplicate elimination, scales with # of values
        - Not # of items! We'll see this again
      - Can simply conquer sometimes! (Think about that)

Summary
  - Sort/Hash Duality
    - Hashing is Divide  & Conquer
    - Sorting is Conquer & Divide
  - Sorting is overkill for rendezvous
    - But sometimes a win anyhow
  - Sorting sensitive to internal sort alg
    - Quicksort vs HeapSort
    - In practice, Quicksort tends to win
  - Don't forget double buffering
