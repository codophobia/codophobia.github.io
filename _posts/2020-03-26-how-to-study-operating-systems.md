---
Layout: post
classes: wide
section-type: post
title: "A Proper Plan To Study Operating Systems Effectively"
category: operating system
description: "In this blog, we will discuss on how does one prepare for interviews or exams which focuses on opearting system concepts"
tags: ['operating system']
---
<a href="https://bit.ly/3gSLmGj" target="_blank"><img src="/assets/images/freecodeschool.png" alt="python tutorial" /></a>

Planning is the process of thinking about activities required to achieve a desired goal. Let's look at what can be the possible goals for which we need planning and what activities needed to be done to achieve these goals.

**Desired goal:**

1. Understand the basics of operating system
2. Score good marks in competitive exams like GATE
3. Pass your college semester exam of OS
4. Crack your interviews

**Activities:**

1. Follow the order of the topics as I mention below
2. Understand what to study vs what to not
    1. Studying for GATE vs studying for semester exam - If you are preparing for GATE, you may need to put effort than another student who is preparing for his/her semester exams.
    2. Theory vs Examples - Don't just read the theory, try to solve as many problems you coul* 
3. Ask questions whenever you have doubt - google it or ask in comments
4. Solve as much examples as you can
5. Pause the video and try to solve problem. Then watch my solution.
6. Go slow. Don’t aim for just completing the playlist. Exception exists for college students who are just studying for passing it .Otherwise, take your tim*
7. Read my notes on each topic

Let's now look at the syllabus of operating system. The syllabus can be broken down into five components which can be further broken down into smaller components:

1. Overview of operating system
2. Process management
3. Process coordination
4. Memory management
5. Storage management

{% include article_ad.html %}

Let's now look at the detailed syllabus in which we break down the above five topics into multiple subtopics. If you just focus on studying these subtopics, you would be doing great.

1. Overview of operating system
    * What is an operating system - OS vs kernel
    * Functions of an operating system
    * System calls in operating system
    * Interrupts in operating system
    * Signals in operating system - interrupts vs signals
2. Process Management
    * Process Concepts
        * What is a process - programs vs process
        * How does OS keeps track or represent a process - Proces Control Block(PCB)
        * Different states a process goes through
        * Scheduling queues in operating system
        * Schedulers in operating system
        * Context switch in operating system
        * Dispatcher in operating system
        * Process creation and lifecycle management with fork(), exec(), wait() and exit() system calls
        * Examples of process creation with fork()
        * Basics of interprocess communication
    * Multithreaded programming
        * What is a thread
        * Process vs Thread
        * User level threads vs kernel level threads
        * Multithreading models
    * Process scheduling
        * Already covered the scheduling queues and schedulers in process concepts section. If forgot, revisit it
        * CPU bound vs IO bound process
        * Preemptive vs nonpreemptive scheduling
        * CPU scheduling criteria
        * Scheduling algorithms with advantages, disadvantages and examples
        * Examples with different arrival time, IO and context switch
3. Process coordination
    * Process synchronisation
        * Race condition in operating system
        * Critical section and critical section problem
        * Software solution to critical section problem - Peterson solution and its drawbacks
        * Hardware solution to critical section problem - test and set vs swap instruction solution
        * Semaphore and mutex
        * Spin lock vs mutex vs binary semaphore vs counting semaphore
        * Producer consumer problem and its solution using mutex and semaphore
        * Reader writer problem and its solution using mutex and   semaphore
    * Deadlocks
        * What is deadlock and its characteristics
        * How to detect deadlock
        * Methods for handling and preventing deadlocks
4. Memory management
    * Memory management techniques
        * Contiguous memory management techniques
        * Fixed partitioning method vs variable partitioning
        * First fit, next fit and best fit algorithms
        * Logical address space vs physical address space
        * Memory mapping and protection in contiguous memory management
        * Noncontiguous memory management techniques
        * Paging in operating system
        * Pages, frames, page table concepts
        * TLB in operating system
        * Memory protection in paging
        * Segmentation memory management technique
    * Virtual memory management
        * What is virtual memory and virtual memory management
        * Demand paging in operating system
        * Page replacement techniques
        * FIFO vs Optimal vs LRU page replacement algorithm with examples
        * Frame allocation techniques and thrashing
5. Storage management
    * File representation and operation on files
    * Types of files
    * Access methods for files
    * Directory and its structure
    * File protection
    * How a file system is implemented
    * Disk scheduling
    * RAID levels

{% include article_ad.html %}