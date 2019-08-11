What: Is a DBMS
  - A Database Management System is software that stores, manages or facilitates access to databases.
  - Traditionally term used narrowly: Relational databases with transactions
  - Now market and terms in rapid transition

What: Is an OS a DVMS
  - Data can be stored in RAM
    - every programming offers this
    - RAM is fast, and random access
    - isn't this heaven??
  - Every OS includes a File System
    - manages files on a persistent databases
    - allows open, read, seek, close on a file
    - allows protections to set on a file
    - drawbacks relative to RAM.

What: Database Systems
  - What more could we want than a file system?
    - Clear API contracts regarding data
      - concurrency control, replication, recovery
    - Simple, efficient, well-definced ad hoc queries
    - Efficient scalable bulk processing
    - Benefits of good data modeling
    - S.M.O.P? (Small Matter Of Programming)
      - Not really...

What: Current market  
  - Relational DBMSs still anchor the software industry
    - Elephants: Oracle, Microsoft, IBM, Teradata, HP, EMC, ...
    - Open source: MySQL, PostgreSQL
    - Emerging Variants: In-Memory, Column-oriented
  - Open Source "NoSQL" is growing
    - Analytics: Hadoop, MapReduce, Spark
    - Key-value stores: Cassandra, Mongo, Couch, ...
  - Search SW is an important special case
    - Google and Bing Solr, Lucene
  - Cloud services are expanding quickly
    - Amazon Redshift/ElasticSearch/EMR, MS Azure, Heroku, ...

Basic Patterns for Big Data
  - Streaming
  - Divide and Conquer

Simplifying Assumption
  - Unordered collections of data items
  - Corollary: can reorder handling of items!
  - Opposite of Von Neumann.
    - unordered handling of unordered data.

Disorder is a friend of Scaling
  - We can order things to our liking
    - For cache locality, rendezvous, etc.
  - We can work on things in batches
    - Pick batch sizes to fit our memory hierarchy
    - Pick batch contents based on data affinities
    - OK to postpone data that doesn't fit nicely in the current batch
  - We can tolerate non-deterministic orders
    - The result of parallel execution
    - For efficiency

Streaming through RAM
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

UNIX Pipes
  - STDIN and STDOUT streams
  - streaming UNIX utilities get/put lines
  - Connect them up with |
    - OS can do chunking for you

Rendezvous
  - Streaming: one chunk at a time. Easy.
  - But some algorithms need certain items to be co-resident in memory.
    - Not guaranteed to appear in the same input chunk
  - Time-space rendezvous
    - In the same place (RAM) at the same time
  - There may be many combos of such items.

Divide and Conquer
  - Out of core algorithms orchestrate rendezvous
  - Typical RAM Allocation:
    - Assume B chunks worth of RAM available
    - Use 1 chunk of RAM to read into
    - Use 1 chunk of RAM to write into
    - B-2 chunks of RAM as space for rendezvous
