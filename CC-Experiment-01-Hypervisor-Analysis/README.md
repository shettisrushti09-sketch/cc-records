# Cloud Computing Lab – Experiment 1

## Comparative Performance Analysis of Type-1 and Type-2 Hypervisors

**Type-1 Hypervisor:** Proxmox VE  
**Type-2 Hypervisor:** VMware Workstation  
**Benchmark:** Sysbench CPU  
**Guest OS:** Ubuntu 22.04.5 LTS x86_64  
**Status:** Completed

---

## 1. Overview

This experiment evaluates the performance of two virtualization approaches:

- **Proxmox VE** – Type-1 hypervisor using KVM-based virtualization
- **VMware Workstation** – Type-2 hosted hypervisor

The same Ubuntu virtual-machine configuration and the same CPU benchmark were used in both environments so that the measured results could be compared under similar conditions.

---

## 2. Objectives

The objectives of this experiment are:

1. To understand the difference between Type-1 and Type-2 hypervisors.
2. To configure an Ubuntu virtual machine on Proxmox VE and VMware Workstation.
3. To verify CPU, memory, storage, and system configuration.
4. To run the same Sysbench CPU benchmark in both environments.
5. To record and compare CPU performance metrics.
6. To analyze how virtualization architecture and configuration can affect virtual-machine performance.

---

## 3. Common Virtual Machine Configuration

| Resource | Proxmox VE | VMware Workstation |
|---|---|---|
| Hypervisor Type | Type-1 | Type-2 |
| Guest OS | Ubuntu 22.04.5 LTS | Ubuntu 22.04.5 LTS |
| vCPU | 2 | 2 |
| RAM | 2 GB | 2 GB |
| Virtual Disk | 20 GB | 20 GB |
| Benchmark | Sysbench CPU | Sysbench CPU |
| CPU Prime Limit | 20,000 | 20,000 |

---

## 4. Hypervisor Architecture

### 4.1 Type-1 Hypervisor – Proxmox VE

Proxmox VE provides virtualization using KVM-based virtualization.

The general architecture used in this experiment is:

```text
+---------------------------------------------+
|           Ubuntu Virtual Machine            |
|                                             |
|           Sysbench CPU Benchmark             |
+---------------------------------------------+
|              Proxmox VE / KVM               |
|                Type-1 Layer                  |
+---------------------------------------------+
|             Physical Host Hardware          |
|              CPU / RAM / Storage             |
+---------------------------------------------+
4.2 Type-2 Hypervisor – VMware Workstation

VMware Workstation runs as software on a host operating system and provides virtualization for the guest virtual machine.

The general architecture is:

+---------------------------------------------+
|           Ubuntu Virtual Machine            |
|                                             |
|           Sysbench CPU Benchmark             |
+---------------------------------------------+
|              VMware Workstation             |
|                Type-2 Layer                  |
+---------------------------------------------+
|              Host Operating System           |
+---------------------------------------------+
|             Physical Host Hardware           |
|              CPU / RAM / Storage             |
+---------------------------------------------+
5. Experimental Procedure
Step 1 – Configure the Virtual Machines

Both virtual machines were configured with:

2 virtual CPUs
2 GB RAM
20 GB virtual disk
Ubuntu guest operating system

The purpose was to keep the virtual-machine resources consistent between the two environments.

Step 2 – Verify the Guest System

The following commands were used inside the Ubuntu virtual machines:

hostnamectl
lscpu
free -h
df -h
top
Purpose of the commands
Command	Purpose
hostnamectl	Displays operating system and hostname information
lscpu	Displays CPU architecture and processor information
free -h	Displays memory and swap usage
df -h	Displays disk-space usage
top	Displays running processes and resource usage
Step 3 – Install Sysbench

The benchmark tool was installed using:

sudo apt update
sudo apt install sysbench -y
Step 4 – Verify Sysbench

The installed version was checked using:

sysbench --version
Step 5 – Run the CPU Benchmark

The same benchmark command was used in both environments:

sysbench cpu --cpu-max-prime=20000 run

Using the same benchmark workload allows the measured CPU performance of the two virtualization environments to be compared under the selected test conditions.

6. Experimental Evidence
6.1 Proxmox VE – Type-1 Hypervisor
Proxmox Dashboard

Proxmox VM Configuration

Proxmox VM Running

Proxmox Ubuntu Console

Proxmox System Configuration

Proxmox Sysbench Result

Proxmox Resource Monitoring

6.2 VMware Workstation – Type-2 Hypervisor
VMware VM Configuration

VMware VM Running

VMware System Configuration 1

VMware System Configuration 2

VMware Sysbench Result

7. Performance Results

The following measurements were obtained from the Sysbench CPU benchmark.

Metric	Proxmox VE (Type-1)	VMware Workstation (Type-2)
Total Execution Time	10.0006 s	10.0010 s
Total Events	16,903	9,842
Events per Second	1,689.43	983.84
Average Latency	0.59 ms	1.02 ms
Minimum Latency	0.57 ms	0.81 ms
Maximum Latency	1.09 ms	4.38 ms
8. Performance Comparison

The measured results show differences in CPU throughput and latency between the two tested virtualization environments.

Total Events
Proxmox VE: 16,903
VMware Workstation: 9,842
Events per Second
Proxmox VE: 1,689.43 events/sec
VMware Workstation: 983.84 events/sec
Average Latency
Proxmox VE: 0.59 ms
VMware Workstation: 1.02 ms
Maximum Latency
Proxmox VE: 1.09 ms
VMware Workstation: 4.38 ms
Execution Time
Proxmox VE: 10.0006 seconds
VMware Workstation: 10.0010 seconds

The execution times were nearly identical because the Sysbench test was run for approximately the same duration in both environments.

9. Understanding the Metrics
Total Execution Time

The total time required by Sysbench to complete the benchmark workload.

Total Events

The total number of benchmark operations completed during the test.

Events per Second

The number of benchmark operations completed per second.

A higher value indicates greater measured throughput for this particular workload.

Average Latency

The average time required to complete an individual benchmark operation.

Minimum Latency

The lowest recorded time required to complete an individual operation.

Maximum Latency

The highest recorded time required to complete an individual operation.

10. Performance Visualizations

The repository contains the following performance visualizations.

Events per Second Comparison

Latency Comparison

Total Events Comparison

Overall Performance Dashboard

Combined Hypervisor Comparison

11. Technical Analysis

The measured results were:

Metric	Proxmox VE	VMware Workstation
Total Events	16,903	9,842
Events/sec	1,689.43	983.84
Average Latency	0.59 ms	1.02 ms
Maximum Latency	1.09 ms	4.38 ms

In this particular experiment, Proxmox VE recorded a higher number of completed events and higher events-per-second throughput, while the measured latency values were lower.

The total execution times were almost identical, with both tests completing in approximately 10 seconds.

These observations are specific to the tested hardware, VM configuration, software environment, and benchmark conditions. Differences in measured performance can be affected by factors such as virtualization architecture, host workload, VM configuration, resource allocation, and software configuration.

Therefore, the results should be interpreted as measurements from this experimental setup rather than as universal performance values for every Proxmox VE or VMware Workstation installation.

12. Key Observations
Both environments successfully ran Ubuntu virtual machines.
Both virtual machines were configured with 2 vCPUs, 2 GB RAM, and a 20 GB virtual disk.
The same Sysbench CPU workload was used for both tests.
The measured total execution times were approximately 10 seconds in both environments.
The measured number of events differed between the two environments.
The measured events-per-second values also differed.
The measured average and maximum latency values differed.
The results demonstrate that virtualization architecture and configuration can influence the observed performance of a CPU-bound workload.
13. Conclusion

This experiment provided a practical comparison between a Type-1 virtualization environment using Proxmox VE and a Type-2 virtualization environment using VMware Workstation.

Both environments used comparable Ubuntu virtual-machine resources and the same Sysbench CPU benchmark.

Under the specific experimental conditions, differences were observed in total events, events per second, and latency, while total execution time remained nearly identical.

The experiment demonstrates the importance of virtualization architecture, VM configuration, host conditions, and benchmark methodology when evaluating virtual-machine performance.

14. Repository Structure
cc-records/
│
├── README.md
│
└── CC-Experiment-01-Hypervisor-Analysis/
    │
    ├── README.md
    │
    ├── images/
    │   ├── 04-vmware-sysbench-result.png
    │   ├── 06-promox-ubantu-sysbench-result.png
    │   ├── events_per_second_comparison.png
    │   ├── latency_comparison.png
    │   ├── overall_performance_dashboard.png
    │   └── total_events_comparison.png
    │
    ├── results/
    │   └── performance-analysis.md
    │
    └── screenshots/
        │
        ├── comparison/
        │   └── 01-hypervisor-performance-comparison.png
        │
        ├── type1-promox/
        │   ├── 01-proxmox-dashboard.png
        │   ├── 02-proxmox-vm-configuration.png
        │   ├── 03-proxmox-vm-running.png
        │   ├── 04-proxmox-ubuntu-console.png
        │   ├── 05-proxmox-system-configuration.png
        │   ├── 06-promox-sysbench-result.png
        │   └── 07-promox-resource-monitoring.png
        │
        └── type2-vmware/
            ├── 01-vmware-vm-configuration.png
            ├── 02-vmware-vm-running.png
            ├── 03-vmware-system-configuration1.png
            ├── 03-vmware-system-configuration 2.png
            └── 04-vmware-sysbench-result.png
15. Key Command Reference
System Information
hostnamectl
lscpu
free -h
df -h
top
Sysbench Installation
sudo apt update
sudo apt install sysbench -y
Sysbench Version
sysbench --version
CPU Benchmark
sysbench cpu --cpu-max-prime=20000 run
16. Experiment Summary
Item	Details
Experiment	Performance Analysis of Type-1 and Type-2 Hypervisors
Type-1 Hypervisor	Proxmox VE
Type-2 Hypervisor	VMware Workstation
Guest OS	Ubuntu 22.04.5 LTS
CPU Allocation	2 vCPU
Memory Allocation	2 GB
Disk Allocation	20 GB
Benchmark	Sysbench CPU
Prime Limit	20,000
Main Metrics	Execution Time, Total Events, Events/sec, Latency
Result	Measured performance characteristics differed between the two environments
