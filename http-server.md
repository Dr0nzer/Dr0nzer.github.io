# C++ HTTP Web Server (Winsock2)

## Overview
A from-scratch, single-threaded HTTP web server built entirely in C++ using the Winsock2 API. This project was developed to gain a deep, low-level understanding of network programming, socket lifecycles, and the HTTP/1.1 protocol without relying on high-level web frameworks.

**Language:** C++
**Networking API:** Winsock2
[🔗 View Full Source Code on GitHub](https://github.com/Dr0nzer/cpp-web-server)
---

## Core Features & Architecture

The current implementation successfully manages the foundational aspects of client-server communication over TCP/IP:

* **Socket Management:** Handles socket creation, binding, listening, and accepting client connections directly through the Windows API.
* **Request Parsing:** Manually parses incoming raw HTTP GET requests to extract requested file paths and headers.
* **Content Serving:** Capable of dynamically reading and serving both text (HTML/CSS) and binary data (images, favicons) directly from the local file system.
* **Header Construction:** Generates compliant HTTP response headers, including appropriate MIME types and status codes (`200 OK`, `404 Not Found`).

---

## Technical Challenges Overcome

* **Binary Data Transmission:** Initially, serving non-text files resulted in corrupted data due to incorrect standard string handling. This was resolved by implementing binary-safe buffer reads (`std::vector<char>` and `std::ifstream::read`) to ensure exact byte-for-byte transmission over the socket.
* **Manual Memory Management:** Ensured strict resource cleanup by explicitly closing sockets and cleaning up the WSA environment (`WSACleanup()`) to prevent memory leaks and zombie connections.

---

## Phase 2 Roadmap: Multithreading

The server is currently single-threaded, meaning it blocks while processing a single request. The immediate next phase of development focuses on scaling the architecture:

1.  **Thread Pool Implementation:** Introducing a fixed-size pool of worker threads to handle incoming connections concurrently.
2.  **Task Queue Synchronization:** Utilizing mutexes and condition variables (`std::mutex`, `std::condition_variable`) to safely push accepted sockets into a queue and wake up idle worker threads.
3.  **Throughput Benchmarking:** Measuring the performance delta in requests-per-second (RPS) before and after the transition to a multithreaded architecture.
