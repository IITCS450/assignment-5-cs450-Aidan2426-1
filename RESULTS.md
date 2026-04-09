# CS450 Assignment 5 - Results

## Context-Switching Approach
Each thread owns a fixed-size stack (4096 bytes) allocated with malloc().
The uswtch x86 assembly routine saves callee-saved registers (edi, esi,
ebx, ebp) and the stack pointer onto the current stack, then loads the
next thread saved stack pointer and restores its registers.
New threads start at thread_stub, which calls the user function and marks
the thread ZOMBIE on return, then yields.

## Scheduler
Round-robin over a table of MAX_THREADS (16) slots. thread_yield finds
the next RUNNABLE thread and calls uswtch to switch to it.

## Mutex
Cooperative spin-lock. mutex_lock yields until the lock is free. Safe
without atomics because only one thread runs at a time.

## Limitations
- Maximum 16 concurrent threads
- Stack size fixed at 4096 bytes
- No preemption, threads must call yield voluntarily
- No priority scheduling
