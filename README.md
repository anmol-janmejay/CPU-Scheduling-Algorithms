# CPU Scheduling Algorithms

A C++ implementation of classic CPU scheduling algorithms with support for timeline traces and statistical summaries. The project simulates process execution based on arrival time, service time, priority, and scheduling policy, making it useful for operating systems coursework, algorithm comparison, and scheduling behavior visualization.

## Features

- Simulates multiple CPU scheduling algorithms from a single input file.
- Supports both timeline output (`trace`) and performance output (`stats`).
- Includes ready-to-use testcase input and expected output files.
- Produces common scheduling metrics such as finish time, turnaround time, and normalized turnaround time.
- Uses a simple command-line workflow with `make` and standard input redirection.

## Supported Algorithms

| ID | Algorithm | Type | Parameter |
| --- | --- | --- | --- |
| `1` | First Come First Serve (FCFS) | Non-preemptive | None |
| `2-q` | Round Robin (RR) | Preemptive | Quantum `q` |
| `3` | Shortest Process Next (SPN) | Non-preemptive | None |
| `4` | Shortest Remaining Time (SRT) | Preemptive | None |
| `5` | Highest Response Ratio Next (HRRN) | Non-preemptive | None |
| `6` | Feedback Queue, quantum = 1 (FB-1) | Preemptive | None |
| `7` | Feedback Queue, quantum = 2^i (FB-2i) | Preemptive | None |
| `8-q` | Aging | Priority-based | Quantum `q` |

## Project Structure

```text
.
+-- main.cpp          # Scheduling algorithm implementations and output formatting
+-- parser.h          # Input parsing and shared simulation data structures
+-- makefile          # Build commands
+-- testcases/        # Sample inputs and expected outputs
+-- README.md         # Project documentation
```

## Requirements

- C++ compiler with C++ standard library support
- `make`

On Linux or WSL, install the required tools with:

```bash
sudo apt-get install g++ make
```

On Windows, you can build the project using MinGW, MSYS2, WSL, or any environment that provides `g++` and `make`.

## Build

Compile the project from the repository root:

```bash
make
```

This creates the executable:

```text
lab4
```

## Run

Run the program by providing input through standard input:

```bash
./lab4 < testcases/01a-input.txt
```

On Windows PowerShell, depending on your compiler environment, the executable may run as:

```powershell
Get-Content .\testcases\01a-input.txt | .\lab4.exe
```

## Input Format

The program expects the following input format:

```text
<operation>
<algorithm-list>
<last-instant>
<process-count>
<process-name>,<arrival-time>,<service-time-or-priority>
...
```

### Fields

| Line | Description |
| --- | --- |
| 1 | Operation mode: `trace` or `stats` |
| 2 | Comma-separated algorithm list, such as `1,2-4,3,4` |
| 3 | Last simulation instant shown on the timeline |
| 4 | Number of processes |
| 5+ | Process definitions |

For algorithms `1` through `7`, each process is described as:

```text
<process-name>,<arrival-time>,<service-time>
```

For Aging, algorithm `8`, each process is described as:

```text
<process-name>,<arrival-time>,<priority>
```

Processes should be sorted by arrival time in the input.

## Example

Input:

```text
trace
1
20
5
A,0,3
B,2,6
C,4,4
D,6,5
E,8,2
```

Command:

```bash
./lab4 < testcases/01a-input.txt
```

Output:

```text
FCFS  0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0
------------------------------------------------
A     |*|*|*| | | | | | | | | | | | | | | | | |
B     | | |.|*|*|*|*|*|*| | | | | | | | | | | |
C     | | | | |.|.|.|.|.|*|*|*|*| | | | | | | |
D     | | | | | | |.|.|.|.|.|.|.|*|*|*|*|*| | |
E     | | | | | | | | |.|.|.|.|.|.|.|.|.|.|*|*|
------------------------------------------------
```

In trace mode:

- `*` means the process is running.
- `.` means the process is waiting.
- Blank spaces mean the process has not arrived or has already completed.

## Statistics Mode

Use `stats` to print performance metrics instead of a timeline:

```text
stats
1,2-2,3,4,5
20
5
A,0,3
B,2,6
C,4,4
D,6,5
E,8,2
```

The statistics output includes:

- Arrival time
- Service time
- Finish time
- Turnaround time
- Normalized turnaround time
- Mean turnaround values

## Testcases

The `testcases` directory contains paired input and output files:

```text
01a-input.txt
01a-output.txt
02a-input.txt
02a-output.txt
...
```

You can compare program output with the expected output:

```bash
./lab4 < testcases/01a-input.txt
```

## Algorithm Notes

- FCFS executes processes in arrival order.
- Round Robin cycles through ready processes using the provided quantum.
- SPN selects the ready process with the shortest service time.
- SRT preempts the current process when another process has a shorter remaining time.
- HRRN selects the process with the highest response ratio.
- Feedback scheduling moves processes through priority queues as they consume CPU time.
- Aging increases waiting process priorities over time to reduce starvation.


