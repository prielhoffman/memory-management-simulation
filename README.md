# OS Memory Management Simulator

A systems programming project in **C** that simulates a simplified **operating system memory management environment** with multiple processes, an MMU, a simulated hard disk, page faults, eviction, and synchronized memory snapshots.

This project demonstrates how core virtual memory concepts can be modeled using **processes**, **threads**, **inter-process communication**, and **synchronization primitives**.

## Tech Stack

- **Language:** C
- **Concurrency:** processes, threads, mutexes, condition variables
- **System Concepts:** memory management, page faults, eviction, MMU behavior, disk access simulation, synchronization

## What the Project Does

The simulator models a simplified memory-management system made of:

- **two CPU processes** that generate memory access requests
- a **Memory Management Unit (MMU)** that handles hits, misses, and page replacement
- a simulated **Hard Disk (HD)** that serves read/write requests
- periodic **memory snapshots** that show the current state of memory

The goal of the project is to simulate how a memory subsystem behaves under concurrent access and limited memory capacity. :contentReference[oaicite:1]{index=1}

## Main Features

- multi-process simulation of CPU memory access
- MMU logic for handling hits and misses
- simulated hard-disk read/write operations
- page eviction when memory becomes full
- FIFO clock-style eviction policy
- printer thread for consistent memory snapshots
- synchronization using mutexes and condition variables
- configurable timing and workload parameters

## System Architecture

The simulation includes four main components:

### 1. CPU Process 1

The first CPU process runs in a loop and periodically generates memory access requests.

Its behavior includes:
- waiting for a configured time interval
- choosing between read and write access
- sending a request to the MMU
- waiting for acknowledgment
- repeating continuously during the simulation window

### 2. CPU Process 2

The second CPU process behaves the same way as the first one.

Using two CPU processes makes it possible to simulate concurrent memory access and contention between multiple request sources. :contentReference[oaicite:2]{index=2}

### 3. Memory Management Unit (MMU)

The MMU is the central component of the system.

It manages a memory array of pages and is responsible for:
- handling incoming memory access requests
- determining whether each access is a **hit** or a **miss**
- triggering disk access when needed
- marking pages as valid, invalid, clean, or dirty
- invoking page eviction when memory pressure becomes too high

The MMU includes three internal threads:

- **Main Thread** – processes incoming access requests
- **Evicter Thread** – frees memory using a FIFO clock-style policy
- **Printer Thread** – periodically captures consistent snapshots of memory state :contentReference[oaicite:3]{index=3}

### 4. Hard Disk (HD)

The hard disk module simulates secondary storage.

It handles page read/write requests from the MMU and responds after a configured access delay, representing the slower behavior of disk compared to memory. :contentReference[oaicite:4]{index=4}

## Simulation Flow

A typical simulation flow looks like this:

1. CPU processes generate read/write requests
2. requests are sent to the MMU
3. the MMU checks whether the target page is already in memory
4. on a hit, the MMU responds directly
5. on a miss, the MMU interacts with the simulated hard disk
6. if memory is full, the evicter thread removes pages according to the policy
7. the printer thread periodically prints a memory snapshot
8. the simulation ends after the configured runtime

## Memory Model

The simulator models memory as an array of pages.

Each page may have a status such as:
- valid or invalid
- clean or dirty

This allows the simulation to represent key memory-management events such as:
- page insertion
- page replacement
- dirty-page write-back
- memory pressure and eviction behavior :contentReference[oaicite:5]{index=5}

## Eviction Policy

When the number of used slots grows too high, the evicter thread begins reclaiming memory.

The project uses a **FIFO clock-style eviction policy**, which balances simplicity with realistic page replacement behavior in the simulation. :contentReference[oaicite:6]{index=6}

## What I Implemented

This project focused on simulating operating-systems behavior rather than building an end-user application.

Key implementation areas included:

- concurrent CPU request generation
- MMU request handling logic
- hit/miss processing
- simulated hard-disk interaction
- eviction control under memory pressure
- printer thread synchronization for safe snapshots
- process/thread coordination and cleanup
- careful synchronization to avoid race conditions and deadlocks

## Why This Project Matters

This project demonstrates practical understanding of:

- virtual memory concepts
- page faults and eviction
- concurrency with processes and threads
- synchronization in systems programming
- coordination between compute and storage components
- simulation of operating-systems behavior in C

It is a strong systems project because it combines multiple low-level concepts in one design: memory management, concurrency, synchronization, and inter-component communication.

## How to Run

Clone the repository:

```bash
git clone https://github.com/your-username/memory-management-simulation.git
cd memory-management-simulation
```

Compile the project:

```bash
make
```

Run the simulation:

```bash
./simulate
```

Modify simulation parameters in `config.h`, including values such as:

- `INTER_MEM_ACCS_T`
- `WR_RATE`
- `MEM_WR_T`
- `HIT_RATE`
- `HD_ACCS_T`
- `SIM_TIME`
- `USED_SLOTS_TH` :contentReference[oaicite:7]{index=7}

## Core Concepts Practiced

- memory management
- MMU simulation
- page faults
- page replacement
- dirty vs. clean pages
- processes and threads
- mutexes and condition variables
- synchronization
- systems programming in C

## Key Takeaways

Through this project, I strengthened my understanding of:

- how memory subsystems behave under concurrent access
- how MMU-style logic handles hits, misses, and disk access
- how eviction policies affect system behavior
- how to coordinate processes and threads safely
- how to design systems simulations that reflect operating-system concepts

## Future Improvements

Possible next steps for the project:

- add more eviction policies for comparison
- collect and visualize hit/miss statistics
- log page-fault and eviction events in more detail
- extend the simulator to model virtual addresses more explicitly
- add performance measurements across different parameter settings
- document the internal message flow between components more clearly
