# Parallel Matrix Multiplication

使用 C 與 POSIX Threads 實作矩陣乘法，從單執行緒版本逐步加入效能量測、多執行緒平行化與動態工作分配。

## Implementation

### mm1.c — Sequential Baseline
使用單執行緒實作基本 Matrix Multiplication，作為後續效能分析與平行化的基準版本。

↓

### mm2.c — Performance Measurement
使用 `gettimeofday()` 量測不同執行階段的時間，包括 Memory Allocation、Initialization、Computation、I/O 與 Teardown。

↓

### mm3.c — Cyclic Row Assignment + pthread
使用 POSIX Threads (`pthread`) 將矩陣 Row 分配給多個 Worker Threads。

採用 Cyclic Row Assignment，例如：

Thread 0 → Row 0, 4, 8, ...
Thread 1 → Row 1, 5, 9, ...
Thread 2 → Row 2, 6, 10, ...

透過 `pthread_create()` 建立 Worker Threads，並使用 `pthread_join()` 等待所有運算完成。

↓

### mm4.c — Dynamic Work Assignment + Mutex
改為 Dynamic Work Assignment，由 Worker Thread 動態取得下一個待處理 Row。

由於多個 Threads 可能同時存取與修改 Shared Row Index，因此可能產生 Race Condition。

使用 `pthread_mutex` 保護 Critical Section，確保每個 Row 能被正確分配。

## Key Concepts

- POSIX Threads (pthread)
- Multithreading
- Cyclic Row Assignment
- Dynamic Work Assignment
- Race Condition
- Mutex / Synchronization
- Performance Measurement

## Tech Stack

C / POSIX Threads / Linux
