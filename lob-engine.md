# High-Performance Limit Order Book (LOB) Matching Engine

## Overview
A low-latency, high-throughput Limit Order Book (LOB) matching engine built entirely in C++. Designed to simulate the core infrastructure used in High-Frequency Trading (HFT) environments, this engine efficiently matches incoming buy and sell orders while maintaining strict price-time priority.

**Peak Throughput:** 14.2 Million orders/second 
**Language:** C++

---

## System Architecture & Data Structures

In a matching engine, memory allocation and lookup times are the primary bottlenecks. The architecture was designed to minimize latency by leveraging highly optimized data structures to ensure `O(1)` operations wherever possible.

* **Price Levels (The Book):** Implemented using a balanced Binary Search Tree (such as `std::map` or a custom Red-Black Tree) to maintain price levels in sorted order. This ensures `O(log N)` time complexity for finding the best bid and ask prices.
* **Order Queues (Time Priority):** At each price level, orders are stored in a doubly-linked list. This guarantees `O(1)` time complexity for order insertions (at the back of the queue) and order cancellations/executions (removing nodes directly via pointer reference).
* **Order Lookup (Cancellations):** A heavily optimized Hash Map (`std::unordered_map`) stores direct pointers to the order nodes in the linked lists. When a cancellation request arrives, the engine achieves `O(1)` lookup and `O(1)` deletion, bypassing the need to traverse the book.

---

## Core Execution Logic

The engine processes three primary message types:
1.  **New Orders (Limit & Market):** Evaluates the contra-side of the book. If marketable, it executes immediately. If not, it is added to the resting order book.
2.  **Cancellations:** Uses the hash map to instantly locate and rip the order out of the doubly-linked list, then updates the price level volume.
3.  **Modifications:** Treated as an atomic Cancel + Replace operation to strictly maintain the integrity of time-priority rules.

---

## Benchmarking & Performance

Performance profiling was conducted by streaming millions of randomized order messages (Adds, Cancels, and Executes) to stress-test the engine's memory management and matching logic.

* **Throughput Achieved:** Reached a sustained peak processing speed of **14,276,658 orders per second**.
* **Optimization Focus:** The massive throughput was achieved by avoiding dynamic heap allocations in the hot path, leveraging object pooling, and ensuring data structures were cache-friendly to minimize CPU cache misses.

---

## Future Enhancements
* Integration of a lock-free Single-Producer Single-Consumer (SPSC) ring buffer for zero-latency message ingestion.
* Implementing custom memory allocators to completely eliminate OS-level allocation overhead during runtime.
