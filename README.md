# SILK Code hacking by sg20180546
 
## 1. Object :  PUT/GET first, Compaction Later : User Request Tail Latency get faster

## 2. Resource constraints : Disk I/O Bandwidth (assume 200mb/s in the paper)

## 3. Bottleneck : When C_m, MemTables filling up
1) Slow Flush : I/O BandWidth engrossed by _trivial L_n compaction_

![image](https://user-images.githubusercontent.com/81512075/197934915-062e59e4-f5d6-47e5-a8c3-c1c35606bafb.png)


2) Full L0 : If L0 filled up , C_m cannot flush


![image](https://user-images.githubusercontent.com/81512075/197935142-b2bbed41-0a62-4b90-9ace-60955f831c92.png)

## 4.remedy

### 1) User Request preempt I/O, then compaction

 (1) LongPeakTest
![image](https://user-images.githubusercontent.com/81512075/197940581-42b25038-8b24-4e0c-b879-4fb77ed66438.png)

 (2) RateLimiter flow
![image](https://user-images.githubusercontent.com/81512075/197938181-bf7743b9-de2f-49b0-abe0-0484a4a8129e.png)


### 2) Scheduling : Flush -> L0 Compaction -> Ln Compaction

 (1) MaybeScheduleFlushOrCompaction

![image](https://user-images.githubusercontent.com/81512075/197941992-85e27410-c98e-456e-aacd-9cbd4ce0a825.png)






---------------------------------------------
## SILK: Preventing Latency Spikes in Log-Structured Merge Key-Value Stores

This is the SILK prototype for the USENIX ATC'19 submission. https://www.usenix.org/conference/atc19/presentation/balmau

SILK is built as an extension of RocksDB https://rocksdb.org/

Useful files for running benchmarks:

The benchmrks are run through the standard RocksDB db_bench https://github.com/facebook/rocksdb/wiki/Benchmarking-tools 

db_bench_tool.cc is the main db_bench file. Contains all the tests, including the YCSB benchmark and LongPeakTest, which fluctuates load peaks and valleys.

Useful files for SILK implementation:
compaction_picker.cc
db.h
db_impl.cc
db_impl.h
db_impl_compaction_flush.cc
thread_status_impl.cc


