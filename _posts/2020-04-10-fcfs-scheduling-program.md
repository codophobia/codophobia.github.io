---
Layout: post
classes: wide
section-type: post
title: "FCFS Scheduling Program in C++ with explanation"
category: operating system
description: "In this post, we implement fcfs scheduling in operating system using c programming language"
tags: ['operating system','process scheduling']
---
<a href="https://bit.ly/2UdnXHc" target="_blank"><img src="/assets/images/freecodeschool.png" alt="python tutorial" /></a>

**Similar Posts You May Be Interested In:**

* [Reader Writer Problem Code in C](https://shivammitra.com/reader-writer-problem-in-c/)
* [FCFS Scheduling Algorithm Code](https://codophobia.github.io/operating%20system/fcfs-scheduling-program/)
* [Nonpreemptive Priority Scheduling Code](https://shivammitra.com/operating%20system/nonpreemptive-priority-scheduling/)
* [Preemptive Priority Scheduling Code](https://shivammitra.com/operating%20system/preemptive-priority-program/)
* [SJF Scheduling Code](https://shivammitra.com/operating%20system/sjf-scheduling-program/)
* [SRTF Scheduling Code](https://shivammitra.com/operating%20system/srtf-scheduling-program/)
* [Round Robin Scheduling Code](https://shivammitra.com/operating%20system/roundrobin-scheduling-program/)

<iframe width="560" height="315" src="https://www.youtube.com/embed/HYe7zOkQF2U" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
 
## What is FCFS Scheduling Algorithm

* FCFS stands for First Come First Serve.
* The process that arrives in ready queue have least first gets the CPU first.
* The processes gets serviced by the CPU in order of their arrival time in ascending order.
* FCFS is a nonpreemptive algorithm

If you want to understand more about FCFS algorithm with example, watch the below video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/CHeAo9U2s_E" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Implementing FCFS Algorithm in C++

**Input**

1. Number of processes
2. Arrival time of each process. If all process arrives at the same time, this can be set to 0 for all processes.
3. Burst time of each process

**Output**

1. Start Time(ST), Completion Time(CT), Turnaround Time(TAT), Waiting Time(WT) and Response Time(RT) for each process.
2. Average turnaround Time, average waiting time and average response time.
3. Throughput and cpu utilization.

If you want to understand implementation of algorithm with an example, do watch the video below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/HYe7zOkQF2U" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% include article_ad.html %}

**Algorithm**

1. Stable sort the processes in order of arrival time in ascending order. 
2. Calculate start time and completion time for each process
3. Start time = (i == 0)?p[i].arrival_time:max(p[i].arrival_time,p[i-1].completion_time)
4. completion_time = start_time + p[i].burst_time
5. Calculate TAT, WT and RT using formulas

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

bool compareArrival(process p1, process p2) 
{ 
    return p1.arrival_time < p2.arrival_time;
}

bool compareID(process p1, process p2) 
{  
    return p1.pid < p2.pid;
}

int main() {

    int n;
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

    cout << setprecision(2) << fixed;

    cout<<"Enter the number of processes: ";
    cin>>n;

    for(int i = 0; i < n; i++) {
        cout<<"Enter arrival time of process "<<i+1<<": ";
        cin>>p[i].arrival_time;
        cout<<"Enter burst time of process "<<i+1<<": ";
        cin>>p[i].burst_time;
        p[i].pid = i+1;
        cout<<endl;
    }

    sort(p,p+n,compareArrival);

    for(int i = 0; i < n; i++) {
        p[i].start_time = (i == 0)?p[i].arrival_time:max(p[i-1].completion_time,p[i].arrival_time);
        p[i].completion_time = p[i].start_time + p[i].burst_time;
        p[i].turnaround_time = p[i].completion_time - p[i].arrival_time;
        p[i].waiting_time = p[i].turnaround_time - p[i].burst_time;
        p[i].response_time = p[i].start_time - p[i].arrival_time;

        total_turnaround_time += p[i].turnaround_time;
        total_waiting_time += p[i].waiting_time;
        total_response_time += p[i].response_time;
        total_idle_time += (i == 0)?(p[i].arrival_time):(p[i].start_time - p[i-1].completion_time);
    }

    avg_turnaround_time = (float) total_turnaround_time / n;
    avg_waiting_time = (float) total_waiting_time / n;
    avg_response_time = (float) total_response_time / n;
    cpu_utilisation = ((p[n-1].completion_time - total_idle_time) / (float) p[n-1].completion_time)*100;
    throughput = float(n) / (p[n-1].completion_time - p[0].arrival_time);

    sort(p,p+n,compareID);

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