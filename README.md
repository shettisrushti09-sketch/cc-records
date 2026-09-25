Cloud Computing Lab – Experiment 1

Performance Analysis of Type-1 and Type-2 Hypervisors






Executive Summary

This repository contains the experimental setup, screenshots, benchmark results, performance visualizations, and analysis for comparing a Type-1 hypervisor (Proxmox VE) with a Type-2 hypervisor (VMware Workstation).

Both environments use Ubuntu virtual machines with the same allocated resources:

2 vCPU

2 GB RAM

20 GB virtual disk

The CPU performance of both virtual machines was evaluated using the Sysbench CPU benchmark with the following command:

sysbench cpu --cpu-max-prime=20000 run

The experiment records CPU throughput, total benchmark events, and latency measurements under the tested configurations.

Table of Contents

Project Objectives

Hypervisor Architectural Comparison

Virtual Machine Specifications

Experimental Procedure

Screenshots and Experimental Evidence

Performance Results

Metric Explanations and Visualizations

Technical Analysis

Conclusion

Repository Structure

1. Project Objectives

The objectives of this Cloud Computing laboratory experiment are:

To understand the difference between Type-1 and Type-2 hypervisors.

To configure an Ubuntu virtual machine using Proxmox VE.

To configure an Ubuntu virtual machine using VMware Workstation.

To verify CPU, memory, storage, and system configuration.

To run the same Sysbench CPU benchmark in both environments.

To record benchmark performance metrics.

To compare CPU throughput and latency between the two virtualization environments.

To understand how hypervisor architecture can affect virtual-machine performance.

2. Hypervisor Architectural Comparison

2.1 Type-1 Hypervisor – Proxmox VE

Proxmox VE is used as the Type-1 hypervisor in this experiment. It uses KVM-based virtualization and operates directly on the physical host hardware.

+------------------------------------------------------+
|              Ubuntu Virtual Machine                  |
|                                                      |
|              Sysbench CPU Benchmark                 |
+------------------------------------------------------+
|                 Proxmox VE / KVM                    |
|                  Type-1 Hypervisor                  |
+------------------------------------------------------+
|              Physical Host Hardware                  |
|                 CPU / RAM / Storage                  |
+------------------------------------------------------+

2.2 Type-2 Hypervisor – VMware Workstation

VMware Workstation is used as the Type-2 hypervisor. It runs on top of the host operating system and provides virtualization for the Ubuntu guest operating system.

+------------------------------------------------------+
|              Ubuntu Virtual Machine                  |
|                                                      |
|              Sysbench CPU Benchmark                 |
+------------------------------------------------------+
|                VMware Workstation                   |
|                  Type-2 Hypervisor                  |
+------------------------------------------------------+
|                Host Operating System                 |
+------------------------------------------------------+
|              Physical Host Hardware                  |
|                 CPU / RAM / Storage                  |
+------------------------------------------------------+

3. Virtual Machine Specifications

The following resources were allocated to the virtual machines used in the experiment:

Resource Parameter

Proxmox VE

VMware Workstation

Hypervisor Type

Type-1

Type-2

Guest OS

Ubuntu

Ubuntu

CPU

2 vCPU

2 vCPU

Memory

2 GB

2 GB

Virtual Disk

20 GB

20 GB

Network

vmbr0

NAT

Benchmark

Sysbench CPU

Sysbench CPU

Prime Limit

20,000

20,000

The common configuration helps provide a comparable basis for the benchmark measurements.

4. Experimental Procedure

Step 1 – Configure the Virtual Machines

Proxmox VE

The Ubuntu virtual machine was configured on Proxmox VE with:

2 vCPU

2 GB RAM

20 GB virtual disk

vmbr0 network

VMware Workstation

The Ubuntu virtual machine was configured on VMware Workstation with:

2 vCPU

2 GB RAM

20 GB virtual disk

NAT networking

Step 2 – Verify System Configuration

The following commands were used inside the Ubuntu virtual machines:

hostnamectl
lscpu
free -h
df -h
top

These commands were used to verify:

Operating-system information

CPU configuration

Memory allocation

Disk-space allocation

Current system/resource usage

Step 3 – Install Sysbench

Sysbench was installed using:

sudo apt update
sudo apt install sysbench -y

The installed version was checked using:

sysbench --version

Step 4 – Run the CPU Benchmark

The same benchmark workload was executed in both environments:

sysbench cpu --cpu-max-prime=20000 run

The benchmark calculates prime numbers up to 20,000 and provides CPU performance measurements such as total events, events per second, and latency.

5. Screenshots and Experimental Evidence

5.1 Proxmox VE – Type-1 Hypervisor

Proxmox Dashboard



Proxmox VM Configuration



Proxmox VM Running



Proxmox Ubuntu Console



Proxmox System Configuration



Proxmox Sysbench Result



Proxmox Resource Monitoring



5.2 VMware Workstation – Type-2 Hypervisor

VMware VM Configuration



VMware VM Running



VMware System Configuration 1



VMware System Configuration 2



VMware Sysbench Result



5.3 Comparison Screenshot



6. Performance Results

The Sysbench CPU benchmark produced the following results:

Performance Metric

Proxmox VE (Type-1)

VMware Workstation (Type-2)

Total Execution Time

10.0006 s

10.0010 s

Total Events

16,903

9,842

Events per Second

1,689.43

983.84

Average Latency

0.59 ms

1.02 ms

Minimum Latency

0.57 ms

0.81 ms

Maximum Latency

1.09 ms

4.38 ms

Result Summary

Proxmox VE

Total Events: 16,903

Events per Second: 1,689.43

Average Latency: 0.59 ms

Minimum Latency: 0.57 ms

Maximum Latency: 1.09 ms

VMware Workstation

Total Events: 9,842

Events per Second: 983.84

Average Latency: 1.02 ms

Minimum Latency: 0.81 ms

Maximum Latency: 4.38 ms

The measured execution times were nearly identical at approximately 10 seconds, while the number of completed events, throughput, and latency measurements differed.

7. Metric Explanations and Visualizations

7.1 Metric Definitions

Total Execution Time

The total time taken by Sysbench to complete the benchmark workload.

Total Events

The total number of benchmark operations completed during the test.

Events per Second

The number of benchmark operations completed per second.

Average Latency

The average time required to complete an individual benchmark operation.

Minimum Latency

The lowest recorded time required to complete an individual operation.

Maximum Latency

The highest recorded time required to complete an individual operation.

7.2 Events per Second Comparison



This visualization compares the CPU throughput measured in events per second.

7.3 Latency Comparison



This visualization compares the latency measurements recorded during the benchmark.

7.4 Total Events Comparison



This visualization compares the total number of benchmark events completed by each virtual machine.

7.5 Overall Performance Dashboard



The dashboard provides a combined visual representation of the benchmark performance measurements.

8. Technical Analysis

The benchmark results show different CPU throughput and latency characteristics between the two virtualization environments.

Proxmox VE

Proxmox VE recorded:

16,903 total events

1,689.43 events/sec

0.59 ms average latency

1.09 ms maximum latency

VMware Workstation

VMware Workstation recorded:

9,842 total events

983.84 events/sec

1.02 ms average latency

4.38 ms maximum latency

The results indicate that the tested environments produced different performance measurements even though the guest VMs were configured with the same basic CPU, memory, and disk resources.

The experiment demonstrates that the virtualization architecture and virtualization stack can influence the performance observed by a CPU-bound workload.

These measurements are specific to the tested hardware, VM configuration, software versions, system conditions, and Sysbench workload. They should not be treated as universal performance values for every Proxmox VE or VMware Workstation installation.

9. Conclusion

This experiment provided a practical comparison between:

Proxmox VE – Type-1 hypervisor

VMware Workstation – Type-2 hypervisor

Both environments used Ubuntu virtual machines with:

2 vCPU

2 GB RAM

20 GB virtual disk

The same Sysbench CPU benchmark was executed in both environments using:

sysbench cpu --cpu-max-prime=20000 run

The collected measurements showed differences in total events, events per second, and latency, while the total execution times were nearly identical.

The experiment therefore demonstrates the importance of hypervisor architecture, VM configuration, and test conditions when evaluating virtual-machine performance.

10. Repository Structure

The repository is organized as follows:

cc-records/
│
├── README.md
│
└── CC-Experiment-01-Hypervisor-Analysis/
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

Key Commands

System Information

hostnamectl
lscpu
free -h
df -h
top

Install Sysbench

sudo apt update
sudo apt install sysbench -y

Check Sysbench Version

sysbench --version

Run CPU Benchmark

sysbench cpu --cpu-max-prime=20000 run

Cloud Computing Lab – Experiment 1
