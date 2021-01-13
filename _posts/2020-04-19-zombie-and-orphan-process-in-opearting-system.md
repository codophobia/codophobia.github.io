---
Layout: post
classes: wide
section-type: post
title: "Zombie and Orphan Process in Operating System"
category: operating system
description: "In this post, I will discuss what is a zombie and orphan process in operating system with examples"
tags: ['operating system']
---
<a href="https://bit.ly/2UdnXHc" target="_blank"><img src="/assets/images/freecodeschool.png" alt="python tutorial" /></a>
 
In [this](https://shivammitra.com/operating%20system/fork-exec-wait-in-operating-system/) post, we discussed process creation and it's lifecycle.

<iframe width="560" height="315" src="https://www.youtube.com/embed/AyZeHBPKdMs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
 

A process begins its life when it is created. A process goes through different states before it gets terminated. The first state that any process goes through is the creation of itself.
Process creation happens through the use of fork() system call, which creates a new process(child process) by duplicating an existing one(parent process).
The process that calls fork() is the parent, whereas the new process is the child.
 
In most cases, we may want to execute a different program in the child process than the parent. The exec() family of function calls creates a new address space and loads a new program into it.
 
Finally, a process exits or terminates using the exit() system call. This function frees all the resources held by the process(except for pcb). A parent process can enquire about the status of a terminated child using wait() system call.
When the parent process uses wait() system call, the parent process is blocked till the child on which it is waiting terminates.
 
![process creation]({{site.baseurl}}/assets/images/fork.png)
 
{% include article_ad.html %}
 
## Possible States for Parent Process after creating the child
 
1. The parent process waits for the child process. This can be done by invoking wait() system call by the parent process. The parent process then transitions from running state to waiting state.
2. The parent process doesn't wait for the child process and executes along with the child.
   1. The parent keeps on executing some other code along with the child. The parent terminates after the child process.
   2. The parent process gets killed before the child completes its execution.
 
## What happens when a parent process waits for the child process
 
```c
pid = fork();
// Both child and parent will now start execution from here.
if(pid < 0) {
   //child was not created successfully
   return 1;
}
else if(pid == 0) {
   // This is the child process
   // Child process code goes here
}
else {
   // Parent process code goes here
   wait(NULL);
}
```
 
{% include article_ad.html %}
 
**Reaping the child process by the parent process**
 
* The wait() system call suspends execution of the calling thread until one of its children terminates.
* The parent process will be suspended or it will enter into a waiting state after invoking wait() system call.
* When the child terminates, the kernel sends SIGCHLD signal to the process and by default the parent process ignores this signal.
* Even after the child process terminates, the process control block of the child process still remains in main memory.
* But when a parent is waiting for the child process using wait(), the parent gets the status of the child when it gets terminated and asks the kernel to clean the PCB of the terminated child. This is also called reaping the child process.
 
To summarise it, when a parent process uses wait() system call, the process control block of the child process gets cleaned after it terminates.
 
## What happens when a parent process doesn't wait for the child process and executes along with it
 
In this case, we are assuming that the parent process terminates after the completion of the child process.
 
* In this case, when the child process terminates, SIGCHLD signal is sent to the parent process by the kernel.
* The SIGCHLD signal is ignored by the parent process by default and the process control block of the child process remains in the main memory.
* This PCB of a child is wasting the precious main memory space since the process has already terminated.
 
**Zombie Process**
 
Zombie process is that process which has terminated but whose process control block has not been cleaned up from main memory because the parent process was not waiting for the child.
If there are a large number of zombie processes, their PCBs IN WORST CASE can occupy the whole RAM.

<iframe width="560" height="315" src="https://www.youtube.com/embed/L3YQDUuDjoo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% include article_ad.html %}

## What happens when a parent process terminates before the completion of the child process
 
**Orphan Process**
 
Orphan process is a process that doesn't have a parent process. This can happen when the parent process terminates due to some reasons before the completion of the child.
 
## What happens with a orphan process
 
**Reparenting**
 
An orphan process gets a new parent. The kernel detects that a process has become orphan and tries to provide a new parent to the orphan process.
In most cases, the new parent is the INIT process, one with the PID 1. The new parent waits for the completion of the child(orphan process) and then asks the kernel to clean the PCB of the orphan process.
 
{% include article_ad.html %}
 
## What happens to zombie processes when its parent terminates
 
Assume that a process created multiple child processes and then didn't use wait() system call. So, all child processes will become zombie after they terminate.
Now, what happens to these zombie processes when the parent process terminates?
 
Zombie processes then also becomes an orphan process. Their PCB still exists in the main memory and they also do not have a parent. These zombie process turned orphan process will then get reparented.
The new parent will then collect their status and ask the kernel to clean their PCB.
 
## Adverse affects of zombie processes
 
**Fork() Bomb**
 
```c
int main()
{
   while(1) 
      fork();    
   return 0;
}
```
 
* The above program will create an infinite number of zombie processes as the parent is not waiting for the child process.
* Number of PIDs is limited in the operating system. Once all PIDs are taken by the zombie process, no new process can be created.
* Since no new process can be created, one cannot even run a command on the command line.
* The only option to recover from a fork() bomb is to reboot the system.
 
{% include article_ad.html %}

<iframe width="560" height="315" src="https://www.youtube.com/embed/tD3yNx7zlwc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Code for Zombie Process
 
```c
#include <stdlib.h>
#include <unistd.h>
int main()
{
   int pid = fork();
    if (pid > 0) {
       // Parent process code
       sleep(60);
   }
   if(pid == 0) {
       //child process code    
       exit(0);
   }
}
```
{% include article_ad.html %}
 
## Code for orphan process
 
```c
#include <unistd.h>
#include <stdlib.h>
int main()
{      
   int pid = fork();
    if (pid > 0) {
       // parent process code
       exit(0); // Parent terminates before the child process
   }
   if(pid == 0) {
       // child process code
       sleep(60);
   }
 
   return 0;
}
```
 
{% include article_ad.html %}

