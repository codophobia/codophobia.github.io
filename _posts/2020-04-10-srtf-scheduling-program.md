---
Layout: post
classes: wide
section-type: post
title: "SRTF scheduling program in C++ with explanation"
category: operating system
description: "In this post, we implement srtf scheduling in operating system using c programming language"
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

<iframe width="560" height="315" src="https://www.youtube.com/embed/WjHOShcT9Fk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
 
## What is SRTF Scheduling Algorithm

* SRTF stands for Shortest Remaining Time First.
* The process that has least burst time gets the CPU first.
* The processes gets serviced by the CPU in order of their burst time in ascending order.
* SRTF is preemptive. A process that is running on the CPU can be removed if a new process arrives in ready queue with lower burst time than current process.
* SRTF is basically preemtive version of SJF.

If you want to understand more about SRTF algorithm with example, watch the below video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/6iG_c8I97rI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Implementing SRTF Algorithm in C++

**Input**

1. Number of processes
2. Arrival time of each process. If all process arrives at the same time, this can be set to 0 for all processes.
3. Burst time of each process

**Output**

1. Start Time(ST), Completion Time(CT), Turnaround Time(TAT), Waiting Time(WT) and Response Time(RT) for each process.
2. Average turnaround Time, average waiting time and average response time.
3. Throughput and cpu utilization.

{% include article_ad.html %}

**Algorithm**

```c
completed = 0
current_time = 0
while(completed != n) {
    find process with minimum burst time among process that are in ready queue at current_time
    if(process found) {
        if(process is getting CPU for the first time) {
            start_time = current_time
        }
        burst_time = burst_time - 1
        current_time = current_time + 1
        if(burst_time == 0) {
            completion_time  = current_time
            turnaround_time = completion_time - arrival_time
            waiting_time = turnaround_time - burst_time
            response_time = start_time - arrival_time

            mark process as completed
            completed++
        }
    }
    else {
        current_time++
    }
}
```

If you want to understand how these formulas are derived, watch the below video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/maosvQi-uWQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% include article_ad.html %}

```c++
#include <iostream>
#include <algorithm> 
#include <iomanip>
#include <string.h> 
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
    int burst_remaining[100];
    int is_completed[100];
    memset(is_completed,0,sizeof(is_completed));

    cout << setprecision(2) << fixed;

    cout<<"Enter the number of processes: ";
    cin>>n;

    for(int i = 0; i < n; i++) {
        cout<<"Enter arrival time of process "<<i+1<<": ";
        cin>>p[i].arrival_time;
        cout<<"Enter burst time of process "<<i+1<<": ";
        cin>>p[i].burst_time;
        p[i].pid = i+1;
        burst_remaining[i] = p[i].burst_time;
        cout<<endl;
    }

    int current_time = 0;
    int completed = 0;
    int prev = 0;

    while(completed != n) {
        int idx = -1;
        int mn = 10000000;
        for(int i = 0; i < n; i++) {
            if(p[i].arrival_time <= current_time && is_completed[i] == 0) {
                if(burst_remaining[i] < mn) {
                    mn = burst_remaining[i];
                    idx = i;
                }
                if(burst_remaining[i] == mn) {
                    if(p[i].arrival_time < p[idx].arrival_time) {
                        mn = burst_remaining[i];
                        idx = i;
                    }
                }
            }
        }

        if(idx != -1) {
            if(burst_remaining[idx] == p[idx].burst_time) {
                p[idx].start_time = current_time;
                total_idle_time += p[idx].start_time - prev;
            }
            burst_remaining[idx] -= 1;
            current_time++;
            prev = current_time;
            
            if(burst_remaining[idx] == 0) {
                p[idx].completion_time = current_time;
                p[idx].turnaround_time = p[idx].completion_time - p[idx].arrival_time;
                p[idx].waiting_time = p[idx].turnaround_time - p[idx].burst_time;
                p[idx].response_time = p[idx].start_time - p[idx].arrival_time;

                total_turnaround_time += p[idx].turnaround_time;
                total_waiting_time += p[idx].waiting_time;
                total_response_time += p[idx].response_time;

                is_completed[idx] = 1;
                completed++;
            }
        }
        else {
             current_time++;
        }  
    }

    int min_arrival_time = 10000000;
    int max_completion_time = -1;
    for(int i = 0; i < n; i++) {
        min_arrival_time = min(min_arrival_time,p[i].arrival_time);
        max_completion_time = max(max_completion_time,p[i].completion_time);
    }

    avg_turnaround_time = (float) total_turnaround_time / n;
    avg_waiting_time = (float) total_waiting_time / n;
    avg_response_time = (float) total_response_time / n;
    cpu_utilisation = ((max_completion_time - total_idle_time) / (float) max_completion_time )*100;
    throughput = float(n) / (max_completion_time - min_arrival_time);

    cout<<endl<<endl;

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