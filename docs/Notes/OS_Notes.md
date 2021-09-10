*Added by Debasis Das (3-Sep-2021)*

*Note: These notes are taken from CPE-MCS-BP1-103 Course provided by Dr Ayan Banerjee (Arizona State University) and for my personal reference only.*


[Reference - Comparison of Scheduling Algorithm](https://www.youtube.com/watch?v=7rh7VnUutHY)

## Introduction to Operating System

**Interrupt**

- The CPU never sleeps, It is either executing our program or some background process or an assembly level instruction.
- To interact with the CPU we need events which internally rely on the interrupts.

**When CPU takes an Interrupt**

-  It pushes the PC to stack
-  Loads the IV (Interrupt Vector) into the Program Counter
-  Each Interrupt has a number , each number has an IV, Each item in the IV is associated with an address to the ISR(Interrupt Service Routine)

**Interrupt Steps**

* Interrupts helps in doing context switching by tracking time
* Loading program counter and status register of interrupt vector 
* Executing Interrupt Service Routines
* Pushing program counter and status register to stack 

**Controlling Program Behavior**

*  Let a program run for a limited time and then switch to others
*  Do not let programs write or read every part of the memory

**mechanism that helps with improving efficiency of resident monitor**

*  Buffering
*  Spooling
*  Caching


**advantage of privilege mode over user mode?**

* It can execute system calls. 
* It can execute any instruction. 
* It can read/write any location in memory. 

**time tracking happen in operating system?**
* Using CPU clock frequency

**approaches for memory protection?**

* Allow programs to only write in their own specific memory part
* Do not allow programs to write over resident monitor

**role of memory management unit**

- Checks programs request for writing into memory falls between base and base + bound

**Memory protection needed for CPU protection?**

- To ensure programs can not execute register 0 (R0) 

**CPU protection is fulfilled through**

- Using hardware enforcement for user and privilege modes 
- Making sure switching from user to privilege mode is supervised by resident monitor


## CPU Scheduling Concepts

**Process**

- Process are active, Act of executing a program
- Requires memory to be assigned to it

**Programs**

- Programs are passive
- One program can be run by multiple processes
- One process can run multiple programs
- A program can generate processes
- Program needs memory to be allocated for
	- Code
	- Global Data
	- Stack
	- Heap
- Programs are written in high level language

**Compiler**

- Converts a program written in high level language into Machine readable language/ Assembly language

**Relative Reference**

- Tag that corresponds to the address of a line of code

**Symbolic Reference**

- Are tags for Library Functions

**Linker**

- uses a relocation tables
- Uses a combination of object files and relocation table to identify loc that needs modification. 
- Converts to binary
- Linker uses memory addresses starting from 0 and incremented by 1. This address is called relative addresses.
- Output of the linker is the .exe file

**Loader**

- Symbol table contains relative addresses
- Loader loads the exe file, invokes the memory management unit, assigns physical memory addresses to the relative addresses

**Process states**

- Initial
- READY- The process is waiting to be assigned to a processor.
* RUN- Instructions are being executed.
* WAIT- The process is waiting for some event to occur(such as an I/O completion or reception of a signal). Process from the wait state can only move to the ready state.
* EXIT - The process has finished execution. Free up the unused memory, Clean up the stack.

**Process Memory State**

- Code
- Global Data 
- Stack (function frames (Local variables, stack pointer, instruction pointer))
- Heap (used for dynamic memory allocation)

Stack and Heap needs dynamic memory allocation.

Stack pointer and heap pointer are used to demarcate the start of stack and heap in memory

**Program Counter**

* Stores the address of the next instruction to be executed
* Program counter are stored in EIP (Instruction Pointer)

**Status Register**

- used to change mode (user vs privilege)

**MMU (Memory Management Unit)**

- Used to do memory protection by utilizing a base and bound

**Process Control Block**

- Holds the memory state of a process
- One PCB per process
- Contains registers, Program Counters, Stack Pointer, Heap Pointer, Process attributes, base , bound etc
- PCB is stored in the Kernel Data Structure stored in Kernel Memory space.
- Process ID, process priority
- Helps for switching between 2 processes.

**Scheduling Queues**

- There are queues for the process states (Initial, Ready, Run, Wait and Exit)
- Run Queue can only hold one process, every other queue can hold more than one process

**Bursts**

- Process contains a CPU and IO Bursts
- Process starts with a CPU Burst and should end with a CPU Burst
- If Sum of CPU Burst Time > Sum of  IO Burst times -> CPU Bound else IO Bound

**Preemptive Scheduling**

- CPU scheduler can stop processes in middle of running 
- Processes which gets pre-empted goes to the ready state

**Non Premptive Scheduling**

- Is used in embedded systems
- Cannot stop a process

**Context Switch**

- Save context of the current running process in its process control block (PCB) 
- Load context of the next running process from its process control block (PCB)

**CPU Scheduling Algorithm**

- FCFS (First come First Serve) or FIFO (first in first out)
- SJF (Shortest Job First)

Decides which process to take from the ready queue and move to the run queue.

**SJF scheduling**

- Process with shortest CPU burst time runs first

**CPU Scheduling metrics**

* ***CPU Utilization:*** The percentage of time that the CPU is busy – not idle.
* ***Throughput:*** The number of processes that are completed per time unit.
* ***Waiting time:*** The sum of the time spent in the ready queue during the life of the process. Time blocked, waiting for I/O, is not part of the waiting time.
* ***service time:***The amount of CPU time that a process will need before it either finishes or voluntarily exits the CPU, such as to wait for input / output.
* ***Turnaround time for a process:*** The amount of time between the time a process arrives in the ready state to the time it exits the running state for the last time
* ***response time:*** The time from first submission of the process until the first running.


## Concurrent Processes


**Race Condition**

- Read-Write
- Write- Write

**hardware solution for concurrency problem between threads**

- Atomicity

**Atomic Instruction**

- CPU will not context switch while executing this instruction

**progress property**

- A thread executing non critical section code, should not block a thread willing to execute critical section

**CPU fairness assumption?**

- In long term, threads should receive same number of CPU cycles

**software solutions for concurrency problem between threads such as Lamport-Bakery algorithm, can not be used in practice?**

- System does not know number of threads and there is no way to determine it 

**Readers-Writers problem**

* Multiple readers can read at the same time 
* Only one writer can write at a time 
* If writer is writing, readers can not read 
* If readers are reading, no writer can write 


## Memory Management

- Process requires memory for code, data, stack and heap
- code and data needs a static amount of data.
- Stack and heap are dynamic which change based on the input it receives. The OS Allocates doesnt know exact amount of data to stack and heap and thus provides a default amount of data. To increase the stack and heap memory a system call to the OS is required.
- Unused stack and heap memory goes to waste (Called as Internal fragmentation)

**Contiguous Memory**

- Continous block of memory that is assigned.

**Internal Fragmentation**

- Unused memory in the stack and heap allocated space

**External Fragmentation**

- Unused memory between process allocated space
- Memory is free but processes are waiting as they are expecting contigous memory allocation and not enough contigous memory is available.

**Solutions to Fragmentation**

- ***Compaction***: Reclaim the holes and make it contigous. Move code in memory

**Policies that can be used to improve efficiency of memory compaction**

* Memory Allocation Protocols 
	* first fit
	* best fit
	* worst fit (works best with large number of small processes)
* Dynamic Linking and Loading 
	* Library Functions and loaded and linked on demand
* Overlays 
	* dividing the programs into fragments, loading one fragment at a time, 
	* Fragment swapping is expensive
	* overlay second fragment on first	
* Swapping
	* Same as overlays but applies to programs


**advantage of contiguous allocation over paging?**

- For accessing lines of code, it requires less memory access

**Pages**

- Log S for frame and (S- Log S) for line numbers
- No need for contigous allocation

**page faults occur when**

- A program wants to start executing
- Accessing a page that was previously in memory, but has been replaced by another page

**Hit Ratio**

- Hit Ratio = Total Number of Hits / Total Number of accesses

**disadvantages of paging**

- Increased memory usage, due to saving page tables 
- Slow creation, due to logical to physical address conversion

**Page Fault**

- A page fault occurs when the MMU Doesnt find the Page table in the RAM

**demand paging**

- Virtual memory is based on using demand paging
- Pages will be moved to memory when they are accessed, and not before that. 
- Initially there will be no program pages in memory, and they will all be kept in hard disk.

**Program Locality**

- A page that is being used currently, has high chance of being used again. 
- Two successive page requests have good chance of asking for the same page.
- Very few pages will have high frequency usage


 