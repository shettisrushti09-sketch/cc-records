# Performance Analysis of Type-1 and Type-2 Hypervisors

| Course | Hypervisors | Benchmark | Status |
| :--- | :--- | :--- | :--- |
| Cloud Computing Lab | Proxmox VE (Type-1) vs VMware Workstation (Type-2) | sysbench CPU (20,000 primes) | Completed |

---

## Executive Summary

This repository contains the complete experimental setup, empirical benchmark data, performance visualization, and technical report comparing the CPU performance of a Type-1 Bare-Metal Hypervisor (Proxmox VE) and a Type-2 Hosted Hypervisor (VMware Workstation).

Both hypervisors were deployed with identically configured Ubuntu Virtual Machines (2 vCPU, 2 GB RAM, 20 GB Disk). The standard sysbench CPU prime-number calculation benchmark (`--cpu-max-prime=20000`) was executed on both virtual machines under identical workload conditions.

> **Key Finding:** Proxmox VE (Type-1 Hypervisor) achieved **1,716.69 Events/sec** compared to VMware Workstation's **1,364.78 Events/sec** — demonstrating a **+25.79% throughput advantage** and a **20.55% reduction in average latency**.

---

## Table of Contents

- [1. Project Objectives](#1-project-objectives)
- [2. Hypervisor Architectural Comparison](#2-hypervisor-architectural-comparison)
- [3. Virtual Machine Specifications](#3-virtual-machine-specifications)
- [4. Experimental Procedure](#4-experimental-procedure)
- [5. Empirical Results & Screenshots](#5-empirical-results--screenshots)
- [6. Performance Comparison Table](#6-performance-comparison-table)
- [7. Metric Explanations & Visualizations](#7-metric-explanations--visualizations)
- [8. Technical Analysis & Discussion](#8-technical-analysis--discussion)
- [9. Conclusion & Engineering Takeaways](#9-conclusion--engineering-takeaways)
- [10. Repository Structure & Reproduction](#10-repository-structure--reproduction)

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

```text
+-------------------------------------------------------------------+
|               Ubuntu Virtual Machine (Type-1 Guest)               |
+-------------------------------------------------------------------+
|               Proxmox VE Hypervisor (Linux Kernel / KVM)          |
+-------------------------------------------------------------------+
|                 Physical Server Hardware (Bare Metal)             |
+-------------------------------------------------------------------+
+-------------------------------------------------------------------+
|               Ubuntu Virtual Machine (Type-2 Guest)               |
+-------------------------------------------------------------------+
|               VMware Workstation (Virtual Machine Monitor)        |
+-------------------------------------------------------------------+
|               Host Operating System (Windows 11 / 10)             |
+-------------------------------------------------------------------+
|                        Physical PC Hardware                       |
+-------------------------------------------------------------------+
# 1. Verify Hostname & System Architecture
hostnamectl

# 2. Verify CPU Topology & Core Allocation
lscpu

# 3. Verify Memory Allocation
free -h

# 4. Verify Disk Partition Allocation
df -h

# 5. Monitor Real-time Process & System Load
top
# Package Index Update & Sysbench Installation
sudo apt update && sudo apt install sysbench -y

# Verify Version
sysbench --version

# Execute CPU Benchmark (Prime Calculation up to 20,000)
sysbench cpu --cpu-max-prime=20000 run
Cloud_computing/
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
