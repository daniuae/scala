# scala

Code-Level Optimizations
Technique	Explanation	✅ Pros	⚠️ Cons	When to Use	When NOT to Use
Avoid collect()	Brings all data to driver	Easy debugging	OOM on large data	Small (<100MB) debug	Large datasets
mapPartitions > map	Runs per partition	Faster (less setup)	Risk: big partition memory	External DB/API calls	If partitions are huge
Broadcast Joins	Send small table to all nodes	Avoid shuffle, fast	OOM if table too big	Small lookup (<100MB)	Both tables large
Cache/Persist	Keep data in memory/disk	Saves recomputation	Uses memory, GC issues	Reused multiple times	One-time dataset
Avoid UDFs	Use built-in SQL funcs	Catalyst optimized	UDFs slower	Use upper(), substring()	Complex custom logic
Partition Writes	Save by column (e.g., year)	Faster reads	Small files risk	Low-cardinality columns	High-cardinality columns
Tune Shuffle Partitions	Controls shuffle splits	Avoids stragglers	Wrong tuning = slow jobs	~128MB per partition	Default (200) for big jobs
Filter Early / Column Pruning	Push down filters	Reduces data read	None (safe)	Always best practice	Rare cases needing all cols
Repartition vs Coalesce	Repartition = shuffle, Coalesce = reduce	Balanced parallelism	Repartition expensive	Repartition → scaling up, Coalesce → reducing files	Unnecessary repartition
Cluster-Level Optimizations
Technique	Explanation	✅ Pros	⚠️ Cons	When to Use	When NOT to Use
Executor/Core Tuning	Balance memory & cores	Efficient resource use	Wrong config = waste	Trial-and-error tuning	Defaults only
Dynamic Allocation	Auto adjust executors	Saves cluster resources	Executor churn delays	Shared clusters	Latency-critical jobs
File Formats (Parquet/ORC)	Columnar + compression	Fast, compact	Not universal format	Analytics	If consumers need CSV/JSON
Data Locality	Run close to data	Less network traffic	Cloud storage ignores	HDFS/YARN	Cloud (S3, GCS)
Adaptive Query Execution (AQE)	Auto-optimize joins/shuffles	Handles skew	Spark 3+ only, overhead	Large joins/agg	Small jobs
Avoid Small Files	Merge tiny files	Faster scans	Coalesce reduces parallelism	After big writes	Streaming jobs
Kryo Serialization	Faster serialization	Efficient shuffles	Need class registration	Big RDD workloads	Small jobs
Shuffle/Spill Tuning	Prevent disk spill	Faster agg/join	Needs memory tuning	Heavy joins/groupBy	Small datasets
⚡ Quick Rules of Thumb:
* Filter early, select fewer cols.
* Prefer Parquet/ORC over CSV/JSON.
* Use broadcast for small tables, repartition for scaling, coalesce for reducing.
* Tune shuffle partitions (default 200 often inefficient).
* Enable AQE in Spark 3.x.
* Avoid small files – aim for ~128MB per partition.
* Don’t use collect() on large data.

👀 Can they talk about data ingestion at scale? 
👀 What are their strategies for vector database synchronization? 
👀 Do they understand the ETL process for training data, or are they just talking about the shiny front end?
