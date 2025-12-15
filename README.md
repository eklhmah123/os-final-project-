FreeRTOS ARM Cortex-M3 Operating System Concepts Demo
Project Information
Course: Operating Systems
Submitted By: Aqba Ejaz, Hafsah Batool, Nagina Naveed, Fazeela Iqbal, Shaiza Imran, Mexab

Project Overview
This project demonstrates five fundamental operating system concepts using FreeRTOS running on an ARM Cortex-M3 processor emulated in QEMU. We implemented a complete embedded system that showcases how modern operating systems handle multitasking, synchronization, resource management, and scheduling in a constrained environment.

Concept 1: Basic Multitasking
We created two independent tasks (Task1 and Task2) that run concurrently on a single processor. Each task maintains its own state with a counter variable and voluntarily yields the CPU using vTaskDelay(). This demonstrates how an operating system manages multiple processes sharing a single CPU through context switching and time slicing. The tasks print their execution count at different intervals, showing true concurrent execution despite having only one physical processor.

Concept 2: Mutex Synchronization
We implemented mutual exclusion using FreeRTOS mutexes to protect a shared resource. Two tasks (Mutex1 and Mutex2) compete for access to a critical section. Only one task can enter the critical section at any time, while the other task waits. This demonstrates how operating systems prevent race conditions and ensure data consistency when multiple threads access shared resources. Each task prints messages before entering, during, and after leaving the critical section to show proper synchronization.

Concept 3: Deadlock Detection and Recovery
We created a classic deadlock scenario where two tasks (Deadlock1 and Deadlock2) attempt to acquire two mutexes (MutexA and MutexB) in opposite orders. To prevent permanent deadlock, we implemented a timeout mechanism. If a task cannot acquire the second mutex within 500ms, it releases the first mutex and retries later. This demonstrates real-world deadlock handling strategies used in operating systems, showing both the problem and a practical solution.

Concept 4: Dynamic Memory Management
We implemented dynamic memory allocation and deallocation using FreeRTOS's heap management system. A dedicated memory task periodically allocates memory using pvPortMalloc(), uses it, and then frees it with vPortFree(). The task tracks the number of active allocations and handles allocation failures gracefully. This demonstrates how operating systems manage heap memory, handle fragmentation, and provide memory isolation between tasks in embedded systems.

Concept 5: Priority-Based Scheduling
We created three tasks with different priority levels: High (priority 3), Medium (priority 2), and Low (priority 1). The high-priority task runs every second, medium every 2.5 seconds, and low every 4 seconds. This demonstrates preemptive priority scheduling where higher-priority tasks can interrupt lower-priority tasks. The output clearly shows the high-priority task running more frequently, illustrating how operating systems use priority levels to ensure critical tasks get CPU time.

Directory Structure
Our FreeRTOS ARM Cortex-M3 project follows a well-organized directory structure that separates source code, configuration files, and build artifacts. The main project resides in the freertos-arm-demo/ directory, which contains all necessary components for building and running the operating system demonstration.

Source Code Directory (src/)
The src/ directory contains all our application source code files that implement the five operating system concepts. This separation keeps our main application code organized and separate from configuration and build files.

main.c - This is the heart of our project, containing the complete implementation of all five operating system concepts. The file defines ten FreeRTOS tasks that demonstrate basic multitasking, mutex synchronization, deadlock handling, memory management, and priority scheduling. Each task is carefully designed to showcase specific OS principles while working together in a cohesive system. The main() function initializes mutexes, creates all tasks, and starts the FreeRTOS scheduler.

startup.c - This critical file handles ARM Cortex-M3 processor initialization before our main application runs. It contains the vector table that defines interrupt handlers, copies data from flash to RAM, clears the BSS section, and calls SystemInit() to configure hardware peripherals. Most importantly, it configures the SysTick timer that FreeRTOS uses for its tick interrupts, making the real-time scheduling possible.

minilib.c - Since embedded systems have limited resources, we created this minimal I/O library specifically for QEMU emulation. It implements basic printf() functionality and character output to QEMU's virtual UART, replacing the standard C library which would be too large for our embedded environment. This allows us to see debug output and program status messages.

minilib.h - This header file declares the functions and constants used by our minimal library. It provides the interface for the I/O functions while keeping implementation details hidden, following good software engineering practices.

Configuration Files
FreeRTOSConfig.h - This configuration file customizes the FreeRTOS kernel for our specific hardware (QEMU's ARM Cortex-M3). It defines critical parameters like the CPU clock speed (8MHz), tick rate (1000Hz for 1ms ticks), heap size (30KB), maximum priorities (5), and enables specific features like mutex support and stack overflow checking. Proper configuration here was essential to make FreeRTOS work correctly with our emulated hardware.

Makefile - Our build automation file defines how to compile and link all source files into the final executable. It specifies the cross-compiler (arm-none-eabi-gcc), compiler flags for Cortex-M3 architecture, source file dependencies, and linking instructions. The Makefile allows us to build the entire project with a single make command, handling all the complex compilation steps automatically.

linker.ld - This linker script defines the memory layout of our embedded system. It specifies where code should go in flash memory (starting at 0x00000000) and where data should reside in RAM (starting at 0x20000000). The script also sets the stack pointer location and ensures proper alignment for Cortex-M3 requirements, which is crucial for the processor to function correctly.

Technical Implementation Details
We developed this project for QEMU's lm3s6965evb (ARM Cortex-M3) virtual machine. The system includes:

startup.c: ARM Cortex-M3 initialization code with vector table setup

FreeRTOSConfig.h: Custom configuration for 8MHz clock, 1ms tick rate, and 30KB heap

linker.ld: Memory layout defining Flash and RAM regions

Makefile: Automated build system using arm-none-eabi-gcc cross-compiler

minilib.c: Minimal I/O library for QEMU UART output

Development Challenges
We encountered several technical challenges:

SysTick Configuration: Initially, QEMU showed "Timer with period zero" errors. We resolved this by properly configuring configSYSTICK_CLOCK_HZ in FreeRTOSConfig.h.

Hard Faults: The system crashed with stack overflows. We fixed this by increasing stack sizes and enabling stack overflow checking.

Build Issues: Linking failures occurred due to incorrect memory regions. We created a proper linker script for Cortex-M3 architecture.
Expected Output
The program displays all five OS concepts working together:

Basic tasks counting independently

Mutex tasks taking turns in critical sections

Deadlock tasks detecting timeouts and recovering

Memory task allocating and freeing memory

Priority tasks showing different execution frequencies

Learning Outcomes
Through this project, we gained practical experience in:

Cross-compilation for embedded ARM systems

Real-time operating system configuration

Concurrent programming with proper synchronization

Debugging embedded systems using QEMU

Memory management in constrained environments

Academic Relevance
This project covers key operating system concepts including process synchronization, deadlock handling, memory management, and CPU scheduling. It provides hands-on experience with real-time operating systems in an embedded context, connecting theoretical concepts with practical implementation.

Project Statistics
Total Tasks: 10 concurrent FreeRTOS tasks

Concepts Demonstrated: 5 core OS concepts

Code Size: Approximately 500 lines

Binary Size: ~15KB (optimized for embedded)
