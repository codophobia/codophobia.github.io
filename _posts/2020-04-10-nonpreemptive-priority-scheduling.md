---
Layout: post
classes: wide
section-type: post
title: "Nonpreemptive priority scheduling program in C++ with explaination"
category: operating system
description: "In this post, we will be implementing nonpreemptive priority scheduling in operating system using c programming language"
tags: ['operating system','process scheduling']
---
{% include article_ad.html %}
 
## What is Nonpreemptive Priority Scheduling Algorithm

* It's similar to SJF scheduling. In SJF, burst time was the priority. Here, priority is explicitly provided.
* The process that has highest priority gets the CPU first.
* The processes gets serviced by the CPU in order of their priority in descending order.
* Higher number always doesn't represents higher priority. In some cases, 0 may be the highest priority and 100 the lowest. It depends on systems.

If you want to understand more about nonpreemptive priority scheduling  algorithm with example, watch the below video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/3MXQ4uP0XsA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Implementing Nonpreemptive Priority Scheduling Algorithm in C++

**Input**

1. Number of processes
2. Arrival time of each process. If all process arrives at the same time, this can be set to 0 for all processes.
3. Burst time of each process
4. Priority of each process

**Output**

1. Start Time(ST), Completion Time(CT), Turnaround Time(TAT), Waiting Time(WT) and Response Time(RT) for each process.
2. Average turnaround Time, average waiting time and average response time.
3. Throughput and cpu utilization.

{% include article_ad.html %}

**Algorithm**

1. Stable sort the processes in order of arrival time in ascending order. If two processs have equal priority, give preference to the process that arrived first.
2. Calculate ST, CT, TAT, WT and RT for the first process after sorting.
3. Keep track of the time using a variable - current_time
4. Loop through the processes and find out the processes that have arrived till current_time and pick the one which has highest priority time among these.
5. If there are no processes that arrived before or equal to current_time, pick the first process from the list that is not completed.
6. Start time for other processes = max(current_time,p[i].arrival_time)
7. Completion_time for other processes = start_time + p[i].burst_time
8. Calculate TAT, WT and RT using formulas

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
#include <string.h> 
using namespace std;

struct process {
    int pid;
    int arrival_time;
    int priority;
    int burst_time;
    int start_time;
    int completion_time;
    int turnaround_time;
    int waiting_time;
    int response_time;
};

bool compareArrival(process p1, process p2) 
{ 
    if(p1.arrival_time == p2.arrival_time) {
        return p1.priority < p2.priority;
    } 
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
        cout<<"Enter priority of the process "<<i+1<<": ";
        cin>>p[i].priority;
        p[i].pid = i+1;
        cout<<endl;
    }

    sort(p,p+n,compareArrival);

    p[0].start_time = p[0].arrival_time;
    p[0].completion_time = p[0].arrival_time + p[0].burst_time;
    p[0].turnaround_time = p[0].completion_time - p[0].arrival_time;
    p[0].waiting_time = p[0].turnaround_time - p[0].burst_time;
    p[0].response_time = p[0].start_time - p[0].arrival_time;
    total_turnaround_time += p[0].turnaround_time;
    total_waiting_time += p[0].waiting_time;
    total_response_time += p[0].response_time;
    total_idle_time += p[0].arrival_time;


    int completed = 1;
    int current_time = p[0].completion_time;
    is_completed[0] = 1;

    while(completed != n) {
        int idx = -1;
        int mx = -1;
        for(int i = 1; i < n; i++) {
            if(p[i].arrival_time <= current_time && is_completed[i] == 0) {
                if(p[i].priority > mx) {
                    mx = p[i].priority;
                    idx = i;
                }
            }
        }
        if(idx == -1) {
            for(int i = 1; i < n; i++) {
                if(is_completed[i] == 0) {
                    idx = i;
                    break;
                }
            }
        }

        p[idx].start_time = max(current_time,p[idx].arrival_time);
        p[idx].completion_time = p[idx].start_time + p[idx].burst_time;
        p[idx].turnaround_time = p[idx].completion_time - p[idx].arrival_time;
        p[idx].waiting_time = p[idx].turnaround_time - p[idx].burst_time;
        p[idx].response_time = p[idx].start_time - p[idx].arrival_time;
        
        total_turnaround_time += p[idx].turnaround_time;
        total_waiting_time += p[idx].waiting_time;
        total_response_time += p[idx].response_time;
        total_idle_time += (p[idx].start_time - current_time);

        is_completed[idx] = 1;
        completed++;
        current_time = p[idx].completion_time;
        
    }

    avg_turnaround_time = (float) total_turnaround_time / n;
    avg_waiting_time = (float) total_waiting_time / n;
    avg_response_time = (float) total_response_time / n;
    cpu_utilisation = ((p[n-1].completion_time - total_idle_time) / (float) p[n-1].completion_time)*100;
    throughput = float(n) / (p[n-1].completion_time - p[0].arrival_time);

    sort(p,p+n,compareID);

    cout<<endl<<endl;

    cout<<"#P\t"<<"AT\t"<<"BT\t"<<"PRI\t"<<"ST\t"<<"CT\t"<<"TAT\t"<<"WT\t"<<"RT\t"<<"\n"<<endl;

    for(int i = 0; i < n; i++) {
        cout<<p[i].pid<<"\t"<<p[i].arrival_time<<"\t"<<p[i].burst_time<<"\t"<<p[i].priority<<"\t"<<p[i].start_time<<"\t"<<p[i].completion_time<<"\t"<<p[i].turnaround_time<<"\t"<<p[i].waiting_time<<"\t"<<p[i].response_time<<"\t"<<"\n"<<endl;
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