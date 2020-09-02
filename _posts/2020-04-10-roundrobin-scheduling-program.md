---
Layout: post
classes: wide
section-type: post
title: "Round Robin scheduling program in C++ with explanation"
category: operating system
description: "In this post, we implement round robin scheduling in operating system using c programming language"
tags: ['operating system','process scheduling']
---
<a href="https://bit.ly/3gSLmGj" target="_blank"><img src="/assets/images/freecodeschool.png" alt="python tutorial" /></a>

**Similar Posts You May Be Interested In:**

* [Reader Writer Problem Code in C](https://shivammitra.com/reader-writer-problem-in-c/)
* [FCFS Scheduling Algorithm Code](https://codophobia.github.io/operating%20system/fcfs-scheduling-program/)
* [Nonpreemptive Priority Scheduling Code](https://shivammitra.com/operating%20system/nonpreemptive-priority-scheduling/)
* [Preemptive Priority Scheduling Code](https://shivammitra.com/operating%20system/preemptive-priority-program/)
* [SJF Scheduling Code](https://shivammitra.com/operating%20system/sjf-scheduling-program/)
* [SRTF Scheduling Code](https://shivammitra.com/operating%20system/srtf-scheduling-program/)
* [Round Robin Scheduling Code](https://shivammitra.com/operating%20system/roundrobin-scheduling-program/)
 
## What is Round Robin Scheduling Algorithm

* Round robin scheduling algorithm is a kind of preemptive FCFS.
* A time quantum is associated with the algorithm. Each process gets CPU for some units of time which is decided by time quantum value and then again enter ready queue if they have burst time remaining.
* The processing is done in FIFO order.

If you want to understand more about round robin algorithm with example, watch the below video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/kfYXMbc-Tok" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Implementing SRTF Algorithm in C++

**Input**

1. Number of processes
2. Arrival time of each process. If all process arrives at the same time, this can be set to 0 for all processes.
3. Burst time of each process
4. Time quantum

**Output**

1. Start Time(ST), Completion Time(CT), Turnaround Time(TAT), Waiting Time(WT) and Response Time(RT) for each process.
2. Average turnaround Time, average waiting time and average response time.
3. Throughput and cpu utilization.

{% include article_ad.html %}

**Algorithm**

1. Stable sort the processes in order of arrival time in ascending order.
2. We will be using a FIFO queue to implement this algorithm
3. We will first push the first process from the sorted list into queue.
4. We will use a array to check if the process is in the queue or not.
5. Keep track of the time using a variable - current_time
6. If the process is getting CPU for the first time, record its start time as current_time.
7. Give quantum unit of time to the process that is at front in the queue and pop this process from the queue.
8. If the burst time of this process becomes 0, calculate CT, TAT, WT and RT for it.
9. If some process has arrived when this process was executing, insert them into the queue.
10. If the current process has burst time remianing, push the process into queue again.
11. If the queue is empty, pick the first process from the list that is not completed.
12. Keep doing this till all processes are completed.


```
TAT = CT - AT
WT = TAT - BT
RT = ST - AT
```

If you want to understand how these formulas are derived, watch the below video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/maosvQi-uWQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% include article_ad.html %}

```c++
#include <iostream>
#include <algorithm> 
#include <iomanip>
#include <queue> 
using namespace std;

struct process {
    int pid;
    int arrival_time;
    int burst_time;
    int start_time;
    int completion_time;
    int turnaround_time;
    int waiting_time;
    int response_time;
};

bool compare1(process p1, process p2) 
{ 
    return p1.arrival_time < p2.arrival_time;
}

bool compare2(process p1, process p2) 
{  
    return p1.pid < p2.pid;
}

int main() {

    int n;
    int tq;
    struct process p[100];
    float avg_turnaround_time;
    float avg_waiting_time;
    float avg_response_time;
    float cpu_utilisation;
    int total_turnaround_time = 0;
    int total_waiting_time = 0;
    int total_response_time = 0;
    int total_idle_time = 0;
    float throughput;
    int burst_remaining[100];
    int idx;

    cout << setprecision(2) << fixed;

    cout<<"Enter the number of processes: ";
    cin>>n;
    cout<<"Enter time quantum: ";
    cin>>tq;

    for(int i = 0; i < n; i++) {
        cout<<"Enter arrival time of process "<<i+1<<": ";
        cin>>p[i].arrival_time;
        cout<<"Enter burst time of process "<<i+1<<": ";
        cin>>p[i].burst_time;
        burst_remaining[i] = p[i].burst_time;
        p[i].pid = i+1;
        cout<<endl;
    }

    sort(p,p+n,compare1);

    queue<int> q;
    int current_time = 0;
    q.push(0);
    int completed = 0;
    int mark[100];
    memset(mark,0,sizeof(mark));
    mark[0] = 1;

    while(completed != n) {
        idx = q.front();
        q.pop();

        if(burst_remaining[idx] == p[idx].burst_time) {
            p[idx].start_time = max(current_time,p[idx].arrival_time);
            total_idle_time += p[idx].start_time - current_time;
            current_time = p[idx].start_time;
        }

        if(burst_remaining[idx]-tq > 0) {
            burst_remaining[idx] -= tq;
            current_time += tq;
        }
        else {
            current_time += burst_remaining[idx];
            burst_remaining[idx] = 0;
            completed++;

            p[idx].completion_time = current_time;
            p[idx].turnaround_time = p[idx].completion_time - p[idx].arrival_time;
            p[idx].waiting_time = p[idx].turnaround_time - p[idx].burst_time;
            p[idx].response_time = p[idx].start_time - p[idx].arrival_time;

            total_turnaround_time += p[idx].turnaround_time;
            total_waiting_time += p[idx].waiting_time;
            total_response_time += p[idx].response_time;
        }

        for(int i = 1; i < n; i++) {
            if(burst_remaining[i] > 0 && p[i].arrival_time <= current_time && mark[i] == 0) {
                q.push(i);
                mark[i] = 1;
            }
        }
        if(burst_remaining[idx] > 0) {
            q.push(idx);
        }

        if(q.empty()) {
            for(int i = 1; i < n; i++) {
                if(burst_remaining[i] > 0) {
                    q.push(i);
                    mark[i] = 1;
                    break;
                }
            }
        }


    }

    avg_turnaround_time = (float) total_turnaround_time / n;
    avg_waiting_time = (float) total_waiting_time / n;
    avg_response_time = (float) total_response_time / n;
    cpu_utilisation = ((p[n-1].completion_time - total_idle_time) / (float) p[n-1].completion_time)*100;
    throughput = float(n) / (p[n-1].completion_time - p[0].arrival_time);

    sort(p,p+n,compare2);

    cout<<endl;
    cout<<"#P\t"<<"AT\t"<<"BT\t"<<"ST\t"<<"CT\t"<<"TAT\t"<<"WT\t"<<"RT\t"<<"\n"<<endl;

    for(int i = 0; i < n; i++) {
        cout<<p[i].pid<<"\t"<<p[i].arrival_time<<"\t"<<p[i].burst_time<<"\t"<<p[i].start_time<<"\t"<<p[i].completion_time<<"\t"<<p[i].turnaround_time<<"\t"<<p[i].waiting_time<<"\t"<<p[i].response_time<<"\t"<<"\n"<<endl;
    }
    cout<<"Average Turnaround Time = "<<avg_turnaround_time<<endl;
    cout<<"Average Waiting Time = "<<avg_waiting_time<<endl;
    cout<<"Average Response Time = "<<avg_response_time<<endl;
    cout<<"CPU Utilization = "<<cpu_utilisation<<"%"<<endl;
    cout<<"Throughput = "<<throughput<<" process/unit time"<<endl;


}

```
{% include article_ad.html %}

[https://github.com/codophobia/process-scheduling-algorithms](https://github.com/codophobia/process-scheduling-algorithms)