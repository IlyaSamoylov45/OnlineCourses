13.1 WHEN DOES A DBMS SORT DATA?
  - Sorting is required in a variety of situations, including the following important ones:
    - Users may want answers in some order; for example, by increasing age
    - Sorting records is the first step in bulk loading a tree index
    - Sorting is useful for eliminating duplicate copies in a collection of records
    - A widely used algorithm for performing a very important relational algebra operation, called join, requires a sorting step
  - Although main memory sizes are growing rapidly the ubiquity of database systems has lead to increasingly larger datasets as well.
  - When the data to be sorted is too large to fit into available main memory, we need an external sorting algorithm. Such algorithms seek to minimize the cost of disk accesses.

Chapter 13.2 A SIMPLE TWO-WAY MERGE SORT
  - Even if the entire file does not fit into the available main memory, we can sort it by breaking it into smaller subfiles, and then sorting these subfiles, and then merging them using a minimal amount of main memory at any given time.
  - In the first pass, the pages in the file are read in one at a time. After a page is read in, the records on it are sorted and the sorted page (a sorted run one page long) is written out.
  - Quicksort or any other in-memory sorting technique can be used to sort the records on a page.
  - In subsequent passes, pairs of runs from the output of the previous pass are read in and merged to produce runs that are twice as long.
  - Algorithm
    - If the number of pages in the input file is 2^k , for some k, then:
    - Pass 0 produces 2^k sorted runs of one page each,
    - Pass 1 produces 2^(k- 1) sorted runs of two pages each,
    - Pass 2 produces 2^(k- 2) sorted runs of four pages each, and so on, until
    - Pass k produces one sorted run of 2^k pages.
  - In each pass, we read every page in the file, process it, and write it out. Therefore we have two disk I/Os per page, per pass.
  - The number of passes is [LogN] + 1, where N is the number of pages in the file. The overall cost is 2N( LogN + 1) I/Os.

Chapter 13.3 EXTERNAL MERGE SORT
  - The intuition behind the generalized algorithm that we now present is to retain the basic structure of making multiple passes while trying to minimize the number of passes.
  - There are two important modifications to the two-way merge sort algorithm:
    - In Pass 0, read in B pages at a time and sort internally to produce N/B runs of B pages each (except for the last run, which may contain fewer pages)
    - In passes i = 1,2, ... use B-1 buffer pages for input and use the remaining page for output; hence, you do a (B - 1)-way merge in each pass.
  - The first refinement reduces the number of runs produced by Pass 0 to N1 = [N/B], versus N for the two-way merge.
  - The second refinement is even more important. By doing a (B - 1)-way merge, the number of passes is reduced dramatically including the initial pass, it becomes [Log(B-1)(N1)]+1 versus [LogN] + 1 for the two-way merge algorithm presented earlier. Because B is typically quite large, the savings can be substantial.
  - External Merge Sort:
    ```
    proc extsort (file)
    // Given a file on disk, sorts it using three buffer pages
    // Produce runs that are B pages long: Pass 0
       Read B pages into memory, sort them, write out a run.
    // Merge B-1 runs at a time to produce longer runs until only
    // one run (containing all records of input file) is left
    While the number of runs at end of previous pass is > 1:
      // Pass i = 1,2, ...
      While there are runs to be merged from previous pass:
        Choose next B - 1 runs (from previous pass).
        Read each run into an input buffer; page at a time.
        Merge the runs and write to the output buffer;
        force output buffer to disk one page at a time.
    endproc
    ```
  - Example : Suppose that we have five buffer pages available and want to sort a file with 108 pages.
    - Pass 0 produces 108/5 = 22 sorted runs of five pages each, except for the last run, which is only three pages long.
    - Pass 1 does a four-way merge to produce 22/4 = six sorted runs of 20 pages each, except for the last run, which is only eight pages long.
    - Pass 2 produces 6/4 = two sorted runs; one with 80 pages and one with 28 pages.
    - Pass 3 merges the two runs produced in Pass 2 to produce the sorted file.
  - Runtime : In each pass we read and write 108 pages; thus the total cost is 2 * 108 * 4 = 864 I/Os. Applying our formula, we have N1 108/5 = 22 and cost = 2 * N * (Log(B-1)(N1) + 1) = 2 * 108 * (Log(4)(22) + 1) = 864.

Chapter 13.3.1 Minimizing the Number of Runs
  - In Pass 0 we read in B pages at a time and sort them internally to produce N/B runs of B pages each
  - With a more aggressive implementation, called replacement sort, we can write out runs of approximately 2. B internally sorted pages on average.
  - Replacement Sort:
    - We begin by reading in pages of the file of tuples to be sorted, say R, until the buffer is full, reserving (say) one page for use as an input buffer and one page for use as an output buffer. We refer to the B - 2 pages of R tuples that are not in the input or output buffer as the current set.
    - Suppose that the file is to be sorted in ascending order on some search key k. Tuples are appended to the output in ascending order by k value.
    - The idea is to repeatedly pick the tuple in the current set with the smallest k value that is still greater than the largest k value in the output buffer and append it to the output buffer.
    - For the output buffer to remain sorted, the chosen tuple must satisfy the condition that its k value be greater than or equal to the largest k value currently in the output buffer; of all tuples in the current set that satisfy this condition, we pick the one with the smallest k value and append it to the output buffer.
    - Moving this tuple to the output buffer creates some space in the current set, which we use to add the next input tuple to the current set.
    - When all tuples in the input buffer have been consumed in this manner, the next page of the file is read in. Of course, the output buffer is written out when it is full, thereby extending the current run.
    - When every tuple in the current set is smaller than the largest tuple in the output buffer, the output buffer is written out and becomes the last page in the current run.
    -  We then start a new run and continue the cycle of writing tuples from the input buffer to the current set to the output buffer.
  - Replacement sort has not been implemented in commercial database systems because managing the main memory available for sorting becomes difficult with replacement sort, especially in the presence of variable length records

Chapter 13.4.2 Double Buffering
  - It is desirable to keep the CPU busy while an I/O request is being carried out; that is, to overlap CPU and I/O processing.
  - In the context of external sorting, we can achieve this overlap by allocating extra pages to each input buffer.
  - Suppose a block size of b = 32 is chosen:
    - The idea is to allocate an additional 32-page block to every input (and the output) buffer.
    - Now, when all the tuples in a 32-page block have been consumed, the CPU can process the next 32 pages of the run by switching to the second, 'double,' block for this run.
    - Meanwhile, an I/O request is issued to fill the empty block.
    - Thus, assuming that the time to consume a block is greater than the time to read in a block, the CPU is never idle! On the other hand, the number of pages allocated to a buffer is doubled
    - This technique, called double buffering, can considerably reduce the total time taken to sort a file.
