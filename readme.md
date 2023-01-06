# Project Teatree

Teatree is a set of experimennts that illustate the use of Rust, [DuckDB](https://duckdb.org/), [Apache Arrow](https://arrow.apache.org/), and the [pola.rs dataframe library](https://www.pola.rs/) to build distributed compute engines. 

## Problem Statement 
Most ML pipeline are slow-moving and not real-time, primarily due to the research nature of many ML projects using Jupyter Notebooks. 
We want to build a high-performance compute data pipeline that ingests data, runs computation on the ingested data, and output results in near-realtime. 
- When input data, algorithms that operate on the knowing data models, and output data formats are known.




## Compare and contrast the following options

1. Rust with DuckDB
2. Rust, Pola.rs and Apache Arrow 

### Disadvantages of using DuckDB
- The [Rust DuckDB API](https://duckdb.org/docs/api/rust.html) is not as complete as the [C++ DuckDB API](https://duckdb.org/docs/api/cpp.html)
- Your algorithm is mainly in SQL versus Rust so you have limited observability in the performance and operation of your algorithm. 
- DuckDB is a single process, single file database. 
- You can only have a single read-write connection to a DuckDB file 
- DuckDB means you are writing lots of complex SQL. 

### Advantage of using DuckDB
- DuckDB is open to langauges  DuckDB support Python, Julia, Rust, Java, C++, R and more.
- DuckDB is open to formats. DuckDB can process Parquet, Arrrow, CSV, native DuckDB files and even Postgresql SQL data. 
- DuckDB is SQL so many more client side data engineers can create data transformations and simple analysis and algorithms in SQL. 
- We can keep an entire data model within a single DuckDB file, since DuckDB is a complete database. This allows us to version and ship a single DuckDB file versus multiple disparate datasets.
- You can maintain multiple connections to a read-only DuckDB file 
- DuckDB supports out of core memory-mapped datasets 


### Disadvantage of Pola.rs Apache Arrow dataframes
- With Pola.rs we are managing a filesystem of Apache Arrow files that represent our data model versus a single package. 
- Pola.rs documentation is sparse 
- Apache Arrow are immutable structures. If we have a mutation we need to write the entire frame back to disk. 

### Advantages of using Pola.rs Apache Arrow dataframes
- The entire solution is in Rust 
- We can add Open Telemetry tracing for each step of the algorithm written in Rust
- Pola.rs supports out of core memory-mapped datasets 


## Solution 
Lets support both options with our solution. 

### tea_tree_db_manager
- written as a Rust Server
- supports managing a directory of Apache Arrow datasets using Rust and Pola.rs
- supports managing (read-write) via a single connection a DuckDB file 
- support creating of duckdb versions that can be shipped to local edge points such as a user's laptop for analysis 
- supports a durable queue for inbound request services that result in mutating data as a set of functional intent-based services, for example updating data or invoking a compute job. 
- support a direct connection for read-requests









