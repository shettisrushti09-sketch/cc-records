Markdown# Performance Analysis of Type-1 and Type-2 Hypervisors

[![Course](https://img.shields.io/badge/Course-Cloud%20Computing%20%2F%20Computer%20Networks-blue)](#)
[![Hypervisors](https://img.shields.io/badge/Hypervisors-Proxmox%20VE%20%7C%20VMware%20Workstation-orange)](#)
[![Benchmark](https://img.shields.io/badge/Benchmark-Sysbench%20CPU%2020k%20Primes-brightgreen)](#)
[![Status](https://img.shields.io/badge/Status-Completed-green)](#)

---

## Executive Summary

This repository contains the complete experimental setup, empirical benchmark data, performance visualization, and technical report comparing the CPU performance of a **Type-1 Bare-Metal Hypervisor (Proxmox VE)** and a **Type-2 Hosted Hypervisor (VMware Workstation)**.

Both hypervisors were deployed with identically configured **Ubuntu Virtual Machines** (2 vCPU, 2 GB RAM, 20 GB Disk). The standard `sysbench` CPU prime-number calculation benchmark (`--cpu-max-prime=20000`) was executed on both virtual machines under identical workload conditions.

### Key Finding

> Proxmox VE (Type-1 Hypervisor) achieved **1,716.69 Events/sec** compared to VMware Workstation's **1,364.78 Events/sec** — demonstrating a **+25.79% throughput advantage** and a **20.55% reduction in average latency**.

---

## Table of Contents

1. [Project Objectives](#1-project-objectives)
2. [Hypervisor Architectural Comparison](#2-hypervisor-architectural-comparison)
3. [Virtual Machine Specifications](#3-virtual-machine-specifications)
4. [Experimental Procedure](#4-experimental-procedure)
5. [Empirical Results & Screenshots](#5-empirical-results--screenshots)
6. [Performance Comparison Table](#6-performance-comparison-table)
7. [Metric Explanations & Visualizations](#7-metric-explanations--visualizations)
8. [Technical Analysis & Discussion](#8-technical-analysis--discussion)
9. [Conclusion & Engineering Takeaways](#9-conclusion--engineering-takeaways)
10. [Repository Structure & Reproduction](#10-repository-structure--reproduction)

---

## 1. Project Objectives

The primary objectives of this Cloud Computing laboratory experiment are:

1. **Deployment:** Provision two identical Ubuntu Virtual Machines across different hypervisor architectures:
   - **Type-1 (Bare-Metal):** Proxmox VE (Kernel-based Virtual Machine / KVM)
   - **Type-2 (Hosted):** VMware Workstation Pro on a Windows Host OS
2. **Standardization:** Enforce uniform hardware resource allocations (2 vCPU, 2048 MB RAM, 20 GB Virtual Storage) to ensure direct comparability.
3. **Benchmarking:** Execute the `sysbench` CPU computational benchmark using 20,000 prime numbers to stress test CPU virtualization efficiency.
4. **Metric Collection:** Capture execution time, total events processed, throughput (events/sec), and latency statistics (min, avg, max, 95th percentile).
5. **Architectural Evaluation:** Quantify the performance overhead introduced by host operating system abstraction layers in Type-2 hypervisors versus bare-metal hypervisor execution.

---

## 2. Hypervisor Architectural Comparison

### Type-1 Hypervisor — Proxmox VE (Bare-Metal Architecture)

Proxmox VE runs directly on the bare-metal physical host hardware. The Linux kernel integrated with KVM (Kernel-based Virtual Machine) acts as the hypervisor. Guest operating system instructions execute directly on hardware CPU VT-x/AMD-V extensions without passing through an intermediate desktop operating system.

```mermaid
graph TD
    A["Physical Hardware (CPU, Memory, Storage, NIC)"] --> B["Proxmox VE Hypervisor<br/>(Bare-Metal OS & KVM Kernel)"]
    B --> C["Ubuntu 24.04 Virtual Machine (CC-Experiment1-type1)"]
    subgraph Guest VM
        C --> D["Sysbench CPU Benchmark"]
    end
Plaintext+-------------------------------------------------------------------+
|               Ubuntu Virtual Machine (Type-1 Guest)               |
+-------------------------------------------------------------------+
|               Proxmox VE Hypervisor (Linux Kernel / KVM)          |
+-------------------------------------------------------------------+
|                 Physical Server Hardware (Bare Metal)             |
+-------------------------------------------------------------------+
Type-2 Hypervisor — VMware Workstation (Hosted Architecture)VMware Workstation runs as an application process on top of a host operating system (Windows 11/10). CPU requests from the guest VM must navigate through the VMware VMM engine, translate through host OS system calls, and be scheduled by the Windows NT kernel scheduler before reaching physical hardware.Code snippetgraph TD
    A["Physical Hardware (CPU, Memory, Storage, NIC)"] --> B["Host Operating System<br/>(Windows 11 / Windows NT Kernel)"]
    B --> C["VMware Workstation (Virtual Machine Monitor - VMM)"]
    C --> D["Ubuntu Virtual Machine (CC-Experiment1-Type2)"]
    subgraph Guest VM
        D --> E["Sysbench CPU Benchmark"]
    end
Plaintext+-------------------------------------------------------------------+
|               Ubuntu Virtual Machine (Type-2 Guest)               |
+-------------------------------------------------------------------+
|               VMware Workstation (Virtual Machine Monitor)        |
+-------------------------------------------------------------------+
|               Host Operating System (Windows 11 / 10)             |
+-------------------------------------------------------------------+
|                        Physical PC Hardware                       |
+-------------------------------------------------------------------+
3. Virtual Machine SpecificationsTo guarantee scientific accuracy and eliminate resource skewing, identical configurations were assigned to both VMs:Resource ParameterProxmox VE (Type-1)VMware Workstation (Type-2)StatusVirtual Machine NameCC-Experiment1-type1CC-Experiment1-Type2StandardizedVM IdentifierVMID 123janzz-virtual-machineStandardizedGuest Operating SystemUbuntu 24.04.3 LTS AMD64Ubuntu Linux 64-bitStandardizedCPU Allocation2 vCPU (1 Socket, 2 Cores)2 vCPU (1 Processor, 2 Cores)IdenticalCPU Type / Modelx86-64-v2-AESHost Passthrough / DefaultHardware MatchedRAM Allocation2048 MiB (2.0 GB)2048 MB (2.0 GB)IdenticalVirtual Disk Capacity20.0 GB20.0 GBIdenticalVirtual Network AdapterVirtIO (vmbr0)NAT (VMnet8)StandardizedBenchmark Toolsysbench 1.0.20sysbench 1.0.20Identical4. Experimental ProcedureStep 1: Virtual Machine Creation & SetupProxmox VE (Type-1):Navigated to https://10.11.0.252:8006 via browser.Initialized Create VM wizard (VM ID: 123, Name: CC-Experiment1-type1).Attached Ubuntu 24.04 ISO, assigned 2 Cores, 2048 MiB RAM, 20 GB VirtIO disk, and vmbr0 network bridge.Completed standard Ubuntu server/desktop installation.VMware Workstation (Type-2):Launched VMware Workstation application on Windows host.Selected Typical Configuration wizard.Mounted Ubuntu ISO, set name to CC-Experiment1-Type2.Specified 20 GB virtual disk, configured 1 Processor with 2 Cores (2 vCPU total), 2 GB RAM, and NAT adapter.Completed standard Ubuntu installation.Step 2: System Configuration VerificationOn both guest OS terminals, system specs were verified prior to testing:Bash# 1. Verify Hostname & System Architecture
hostnamectl

# 2. Verify CPU Topology & Core Allocation
lscpu

# 3. Verify Memory Allocation
free -h

# 4. Verify Disk Partition Allocation
df -h

# 5. Monitor Real-time Process & System Load
top
Step 3: Sysbench Benchmark Installation & ExecutionBash# Package Index Update & Sysbench Installation
sudo apt update && sudo apt install sysbench -y

# Verify Version
sysbench --version

# Execute CPU Benchmark (Prime Calculation up to 20,000)
sysbench cpu --cpu-max-prime=20000 run
5. Empirical Results & ScreenshotsType-1 Hypervisor Screenshot (Proxmox VE)Below is the verified screenshot images/1.png captured directly from the Proxmox VE noVNC web console:Figure 1: Proxmox VE (Type-1 Hypervisor) Sysbench Benchmark Console Output.Type-2 Hypervisor Screenshot (VMware Workstation)Below is the verified screenshot images/2.png captured directly from VMware Workstation:Figure 2: VMware Workstation (Type-2 Hypervisor) Sysbench Benchmark Terminal Output.6. Performance Comparison TableThe following table summarizes the exact values recorded from the experimental benchmark runs:Performance MetricProxmox VE (Type-1)VMware Workstation (Type-2)Performance DeltaWinner / AdvantageHypervisor TypeBare-MetalHostedArchitecturalType-1 Direct ControlGuest OSUbuntuUbuntuMatchedIdentical BaselinevCPU Allocation2 vCPU2 vCPUMatchedIdentical ComputeRAM Allocation2 GB2 GBMatchedIdentical MemoryDisk Capacity20 GB20 GBMatchedIdentical StorageBenchmark Limit20,000 Primes20,000 PrimesMatchedIdentical Stress TestTotal Execution Time10.0004 s10.0007 s~0.003% differenceFixed 10s WindowTotal Events Processed17,16913,650+3,519 events (+25.78%)Proxmox VE (Type-1)Events per Second (EPS)1,716.691,364.78+351.91 eps (+25.78%)Proxmox VE (Type-1)Minimum Latency0.57 ms0.67 ms-0.10 ms (-14.93%)Proxmox VE (Faster)Average Latency0.58 ms0.73 ms-0.15 ms (-20.55%)Proxmox VE (Lower)95th Percentile Latency0.65 ms0.89 ms-0.24 ms (-26.97%)Proxmox VE (More Consistent)Maximum Latency2.78 ms4.06 ms-1.28 ms (-31.53%)Proxmox VE (Fewer Spikes)7. Metric Explanations & VisualizationsPerformance Metric DefinitionsTotal Execution Time (seconds): The wall-clock duration taken to execute the Sysbench workload. Standardized to ~10 seconds.Events per Second (Throughput / EPS): The number of prime number calculation iterations completed per second. Higher is better.Total Events: Total number of prime verification cycles executed during the test duration. Higher is better.Latency (milliseconds): Time elapsed per event execution:Minimum Latency: The fastest event execution time.Average Latency: Arithmetic mean of all event processing times.95th Percentile Latency: The latency threshold below which 95% of all events fell. Critical for evaluating response consistency.Maximum Latency: The worst-case event delay, highlighting thread scheduling latency spikes.Chart 1: CPU Throughput Comparison (Events / Sec)Figure 3: CPU Throughput comparison showing Proxmox VE (+25.79% faster).Chart 2: CPU Latency Metrics ComparisonFigure 4: Latency comparison (Min, Avg, 95th Percentile, Max) across both hypervisors.Chart 3: Total Events ProcessedFigure 5: Total Events completed in 10 seconds (17,169 vs 13,650).Chart 4: Comprehensive Performance DashboardFigure 6: Multi-panel performance evaluation dashboard.8. Technical Analysis & DiscussionThe empirical data demonstrates a clear performance superiority of Proxmox VE (Type-1) over VMware Workstation (Type-2) in CPU-bound computational workloads.1. Architectural Overhead & Trap-and-Emulate DelaysProxmox VE (Type-1) utilizes Linux KVM, which interfaces directly with hardware Intel VT-x / AMD-V virtualization extensions. CPU instructions generated inside the VM execute directly in VMX root mode with minimal hypervisor interception.VMware Workstation (Type-2) operates on top of Windows NT OS. Privileged guest CPU operations undergo double translation: first through VMware's VMM virtualization engine, and second through Windows kernel user-to-kernel mode context transitions (NtSystemService).2. CPU Scheduling & Context SwitchingIn Proxmox VE, guest vCPUs map directly to host Linux kernel POSIX threads scheduled by the Completely Fair Scheduler (CFS) operating at Ring 0.In VMware Workstation, guest CPU execution competes with Windows host background services (e.g., Windows Defender, System Updates, Desktop Window Manager). The host OS scheduler introduces thread preemptions, leading to higher latency spikes (Max Latency: 4.06 ms on VMware vs 2.78 ms on Proxmox).3. Memory & Virtual Cache AccessProxmox VE benefits from direct Extended Page Tables (EPT / NPT) hardware translation.Type-2 hypervisors incur memory address translation penalties when mapping Guest Physical Address (GPA) → Host Virtual Address (HVA) → Host Physical Address (HPA).9. Conclusion & Engineering TakeawaysBare-metal dominance: Proxmox VE (Type-1) delivers +25.79% higher CPU throughput and 20.55% lower average latency compared to VMware Workstation (Type-2).Predictable Latency: Proxmox VE exhibits lower 95th percentile latency (0.65 ms vs 0.89 ms), making Type-1 hypervisors essential for latency-critical production enterprise workloads.Use-Case Recommendation:Type-1 (Proxmox VE / KVM / ESXi): Recommended for Cloud Data Centers, Production Enterprise Infrastructure, Database Servers, and High-Performance Computing (HPC).Type-2 (VMware Workstation / VirtualBox): Recommended for Local Software Development, Testing, Desktop Sandbox Environments, and Educational Labs.10. Repository Structure & ReproductionFolder LayoutPlaintextCloud_computing/
│
├── README.md                                           # Main Project & Benchmark Report
├── LAB_REPORT.md                                       # Formal Academic Lab Report Submission
├── Lab-Manual-Hypervisor-Performance-Analysis (1).docx # Reference Lab Manual Document
│
├── images/                                             # Screenshots & Generated Charts
│   ├── 1.png                                           # Proxmox VE Sysbench Result Screenshot
│   ├── 2.png                                           # VMware Workstation Sysbench Result Screenshot
│   ├── events_per_second_comparison.png                # Throughput Comparison Graph
│   ├── latency_comparison.png                          # Latency Metrics Graph
│   ├── total_events_comparison.png                     # Total Events Graph
│   └── overall_performance_dashboard.png               # Multi-panel Dashboard
│
└── scripts/                                            # Automation & Plotting Scripts
    ├── benchmark.sh                                    # Sysbench Automation Script
    ├── generate_plots.py                               # Matplotlib Visualization Generator
    └── parse_sysbench.py                               # Results Parser & Ratio Calculator
How to ReproduceRun Benchmark Script on VM:Bashchmod +x scripts/benchmark.sh
./scripts/benchmark.sh
Generate Plots:Bashpython scripts/generate_plots.py
Parse & Compare Results:Bashpython scripts/parse_sysbench.py
