---
Layout: post
classes: wide
section-type: post
title: "What is an operating system and it's functions"
category: operating system
description: "In this blog, we will discuss what defines an operating system and what are the functions of the operating system"
tags: ['operating system']
---
{% include article_ad.html %}

## Definition 

We don't have a universal definition for defining an opertaing system. Some of the ways books define an operating system are as as follows:

* An operating system is a system program that manages the system's hardware.
* An operating system acts as an interface between the user and the hardware.

If you notice, hardware is mentioned in both the definitions. Let's try to combine these two definitions into one.

**An operating system is a program that acts as an interface betwwen the user and the hardware.**

We will later discuss in details what do we exactly mean by this definition.

<iframe width="560" height="315" src="https://www.youtube.com/embed/6FqUzzVwm4E" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Components of an operating system

An operating system includes all the programs that are associated with the operatin of the system.

1. Kernel - It is the heart of the operating system
2. Device Drivers - It is used to communicate with the devices
3. User interface - UI/CLI
4. System libraries like glibc
5. System utilities like disk formatters, antivirus, data recovery tools etc.

It is not always easy to differenciate between application programs and system programs. Microsoft in 1998 argues that Internet Explorer was as system software that cannot be uninstalled.

{% include article_ad.html %}

## What is a kernel in operating system

A kernel is the heart of the operating system. Kernel is nothing but a computer program that is always resident in main memory and running. If the kernel stops, the operating system dies.
The kernel code is a major part of operating system's code. 

Which code helps in interaction between the user/application and the hardware devices?
It's the kernel code.

When you look at the different functions of the kernel, you will understand how important a kernel is.

## Functions of kernel

1. Process management
    * Managing lifecyle of a process from creating a process to terminating it
    * Scheduling process - Among multiple processes in system, which process will get which CPU and for how much time?
    * Helps in coordination between multiple processes when they want to share data
2. Memory management
    * Keeping track of which parts of main meory are being used and which are free
    * Deciding which process/pages move out of main meory when it is full
    * Allocating required memory for processes
3. File management
    * Creating and deleting files and folders
    * Storage of files on the disk
    * Manipulation of files like renaming, copying, permissions etc.
4. Device management - With the help of interrupts and device drivers, the kernel communicates with the devices like keyboard, mouse, disk etc.
5. Protection and security - Allowing the acccess to processes and files only to the users who are allowded. Th kernel which runs in priviledged mode also helps in preventing attacks from  viruses.

## When does an operating system becomes a kernel?

In embedded devices, you don't require a user interface and system utilities. In these devices, an operating system code is just the kernel code. 

## Functions of an operating System

<iframe width="560" height="315" src="https://www.youtube.com/embed/IEL2smumOks" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

1. Since kernel is part of the operating system, all the functions of the kernel like process management, memory management, file management etc are also functions of an operating system.
2. Included GUI/CLI for intercation with the system
3. Includes sytem libraries and utilities

{% include article_ad.html %}

## Architecture diagram of an operating system

![operating system diagram]({{site.baseurl}}/assets/images/os-diagram.png)

One can see in the above diagram how operating system is acting as an interface between uses/applications and system hardware with the help of the kernel.
Let's understand what happens when a application wants to write to a file.

1. A programmer uses the fwrite() function in C to write to a file. This fwrite() function is a part of glibc library and its function prototypes are defined under "stdio.h" header file.
2. fwrite() is an API used for writing to file. When the fwrite() function is called, it makes a call to the function that is defined in the library.
3. The fwrite() library function then invokes a system call write(). If a user program wants to interact with the kernel, it is done through system calls.
4. Once the system call is made, the system call function definition is invoked. This function definition is under kernel code. 
5. The kernel then uses device drivers to talk to the device where the file is present. It then asks the device driver to write the contents into the file.

If you want to understand more about system calls, do watch the below video. System calls are very important to kernel.

<iframe width="560" height="315" src="https://www.youtube.com/embed/SPGUMdubEws" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Examples of popular operating system and kernel

| Operating system | Kernel |
| ---------------- | ------ |
| Microsoft windows | Windows NT |
| Mac OS | XNU |
| Ubuntu, redhat, centos | Linux |