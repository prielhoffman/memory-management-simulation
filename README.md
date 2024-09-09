# Memory Management Simulation System

## Overview

The `memory-management-simulation` repository contains a simulation system that mimics the behavior of a multi-process memory management environment. The simulation involves two CPU processes, a Memory Management Unit (MMU), a Hard Disk (HD), and associated modules that manage memory access, page faults, and memory eviction in a simulated environment.

## Modules Description

### Process 1
Simulates a CPU process running in an endless loop. The loop sequence involves:
1. Waiting for `INTER_MEM_ACCS_T` nanoseconds.
2. Performing a memory access, which can be a write operation with probability `WR_RATE` or a read operation otherwise.
3. Sending a memory access request to the Memory Management Unit (MMU).
4. Waiting for an acknowledgment from the MMU.
5. Repeating from step 1.

This process discards data and virtual addresses for simplicity. It informs the MMU about the memory access request type (read or write) only.

### Process 2
Identical to Process 1. Two processes are used to simulate parallel execution and the potential for simultaneous memory access requests.

### Memory Management Unit (MMU)
Manages a memory array of `N` pages, where each page can be "empty" (invalid) or "used" (valid). It contains three threads:

- **Main Thread**: Handles memory access requests from Processes 1 and 2. It distinguishes between a memory hit or miss and performs appropriate actions based on the access type and hit/miss status.
- **Evicter Thread**: Manages page eviction when memory is full, following a FIFO clock scheme. It continues evicting pages until the number of used slots is below `USED_SLOTS_TH`.
- **Printer Thread**: Periodically takes snapshots of the memory status, ensuring no reads/writes occur during the snapshot for consistency. The memory snapshot format indicates the validity and cleanliness of each page.

### Hard Disk (HD)
Handles requests from the MMU for reading and writing pages to disk. Each operation takes `HD_ACCS_T` nanoseconds to complete, after which an acknowledgment is sent to the requester.

## Simulation Termination

- The simulation runs for `SIM_TIME` seconds.
- Upon successful completion, the message "Successfully finished sim" is printed, and the simulation terminates.
- In case of errors or upon termination, all mutexes are destroyed, dynamically allocated memory is released, and all processes and threads are terminated properly.

## Additional Requirements

- Proper error handling is implemented for system calls like `fork()`, `pthread_create()`, `msgsnd()`, etc.
- Mutexes and condition variables are initialized to avoid deadlocks and race conditions.
- Minimal critical sections ensure mutual exclusion without unnecessary overhead.
- No additional output is printed beyond the specified requirements.

## How to Run the Simulation

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/your-username/memory-management-simulation.git
   cd memory-management-simulation
   ```
   
2. **Compile the Code:**
   ```bash
   make
   ```

3. **Run the Simulation:**
   ```bash
   ./simulate
   ```
4. **Modify Simulation Parameters:** Edit the 'config.h' file to modify parameters like 'INTER_MEM_ACCS_T', 'WR_RATE', 'MEM_WR_T', 'HIT_RATE', 'HD_ACCS_T', 'SIM_TIME', 'USED_SLOTS_TH', etc.
