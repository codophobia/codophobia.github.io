---
Layout: post
classes: wide
section-type: post
title: "First Readers Writers Problem Solution in C using Semaphore and Mutex"
description: "In this blog, we look at a solution for first readers writers problem using semaphore and mutex"
tags: ['operating system','c programming','semaphore','mutex']
---
{% include article_ad.html %}

**Similar Posts You May Be Interested In:**

* [Producer Consumer Problem Code in C](https://shivammitra.com/c/producer-consumer-problem-in-c/)
* [FCFS Scheduling Algorithm Code](https://codophobia.github.io/operating%20system/fcfs-scheduling-program/)
* [Nonpreemptive Priority Scheduling Code](https://shivammitra.com/operating%20system/nonpreemptive-priority-scheduling/)
* [Preemptive Priority Scheduling Code](https://shivammitra.com/operating%20system/preemptive-priority-program/)
* [SJF Scheduling Code](https://shivammitra.com/operating%20system/sjf-scheduling-program/)
* [SRTF Scheduling Code](https://shivammitra.com/operating%20system/srtf-scheduling-program/)
* [Round Robin Scheduling Code](https://shivammitra.com/operating%20system/roundrobin-scheduling-program/)

**IMPORTANT**

If you or your friends are interested for live classes on youtube for operating system topics which can help you either in interviews, competitive exams like GATE or your college exams, do fill the form and mention your reason. In case, you also need 1-1 meetings for clearing some of your doubts and mentorship, do mark it in the form. Do share this with your friends :)

**Form link**: [https://bit.ly/2LVLWWy](https://bit.ly/2LVLWWy)

Do subscribe to my **youtube channel** also for live classes: [https://www.youtube.com/c/codophobia](https://www.youtube.com/c/codophobia)

## What is readers writers problem?

Suppose that a database is to be shared among several concurrent processes.
Some of these processes may want only to read the database, whereas others
may want to update (that is, to read and write) the database. We distinguish
between these two types of processes by referring to the former as readers
and to the latter as writers. Obviously, if two readers access the shared data
simultaneously, no adverse effects will result. However, if a writer and some
other process (either a reader or a writer) access the database simultaneously,
chaos may ensue.

To ensure that these difficulties do not arise, we require that the writers
have exclusive access to the shared database while writing to the database. This
synchronization problem is referred to as the readers-writers problem. 

The readers-writers problem has several variations, all involving
priorities. The simplest one, referred to as the first readers-writers problem,
requires that no reader be kept waiting unless a writer has already obtained
permission to use the shared object. In other words, no reader should wait for
other readers to finish simply because a writer is waiting.

<iframe width="560" height="315" src="https://www.youtube.com/embed/-DLCGdiRdMM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SjUSnpsJEQs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Pseudocode Solution using Semaphore and Mutex

**Initialization**

```c
Semaphore wrt = 1; // A binary semaphore that will be used both for mutual exclusion and signalling
Mutex mutex; // Provides mutual exclusion when readcount is being modified
int readcount = 0; // To keep count of the total readers
```

**Writer**

```c
wait(wrt);
// Perform write operation
signal(wrt);
```
{% include article_ad.html %}
**READER**

```c
wait(mutex);
readcount++;
if(readcount == 1) {
    wait(wrt);
}
signal(mutex);
// Perform read operation

wait(mutex);
readcount--;
if(readcount == 0) {
    signal(wrt);
}
signal(mutex);
```

* Writer can only write when the readcount is zero or there are no readers waiting.
* If the first reader executes wait(wrt) operation before the writer does, then writer gets blocked.
* Only when the last reader exits, it calls the signal(wrt) operation signalling writer to continue
* Similarly, when a writer starts writing(readcount=0) then the first reader gets blocked on wait(wrt) and this blocks all the readers.

## Solution in C using Semaphore and Mutex

Do read this blog to understand about usage of semaphore and mutex in c: [http://faculty.cs.niu.edu/~hutchins/csci480/semaphor.htm](http://faculty.cs.niu.edu/~hutchins/csci480/semaphor.htm)

Code github link: [https://github.com/codophobia/readers-writers-solution-in-c](https://github.com/codophobia/readers-writers-solution-in-c)

{% include article_ad.html %}
```c
#include <pthread.h>
#include <semaphore.h>
#include <stdio.h>

/*
This program provides a possible solution for first readers writers problem using mutex and semaphore.
I have used 10 readers and 5 producers to demonstrate the solution. You can always play with these values.
*/

sem_t wrt;
pthread_mutex_t mutex;
int cnt = 1;
int numreader = 0;

void *writer(void *wno)
{   
    sem_wait(&wrt);
    cnt = cnt*2;
    printf("Writer %d modified cnt to %d\n",(*((int *)wno)),cnt);
    sem_post(&wrt);

}
void *reader(void *rno)
{   
    // Reader acquire the lock before modifying numreader
    pthread_mutex_lock(&mutex);
    numreader++;
    if(numreader == 1) {
        sem_wait(&wrt); // If this id the first reader, then it will block the writer
    }
    pthread_mutex_unlock(&mutex);
    // Reading Section
    printf("Reader %d: read cnt as %d\n",*((int *)rno),cnt);

    // Reader acquire the lock before modifying numreader
    pthread_mutex_lock(&mutex);
    numreader--;
    if(numreader == 0) {
        sem_post(&wrt); // If this is the last reader, it will wake up the writer.
    }
    pthread_mutex_unlock(&mutex);
}

int main()
{   

    pthread_t read[10],write[5];
    pthread_mutex_init(&mutex, NULL);
    sem_init(&wrt,0,1);

    int a[10] = {1,2,3,4,5,6,7,8,9,10}; //Just used for numbering the producer and consumer

    for(int i = 0; i < 10; i++) {
        pthread_create(&read[i], NULL, (void *)reader, (void *)&a[i]);
    }
    for(int i = 0; i < 5; i++) {
        pthread_create(&write[i], NULL, (void *)writer, (void *)&a[i]);
    }

    for(int i = 0; i < 10; i++) {
        pthread_join(read[i], NULL);
    }
    for(int i = 0; i < 5; i++) {
        pthread_join(write[i], NULL);
    }

    pthread_mutex_destroy(&mutex);
    sem_destroy(&wrt);

    return 0;
    
}
```
{% include article_ad.html %}
