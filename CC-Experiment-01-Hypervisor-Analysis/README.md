# Performance Analysis of Type-1 and Type-2 Hypervisors

## Executive Summary
This repository contains the complete experimental setup, empirical benchmark data, performance visualization, and technical report comparing the CPU performance of a **Type-1 Bare-Metal Hypervisor (Proxmox VE)** and a **Type-2 Hosted Hypervisor (VMware Workstation)**.

Both hypervisors were deployed with identically configured Ubuntu Virtual Machines (2 vCPU, 2 GB RAM, 20 GB Disk). The standard `sysbench` CPU prime-number calculation benchmark (`--cpu-max-prime=20000`) was executed on both virtual machines under identical workload conditions.

### Key Finding
Proxmox VE (Type-1 Hypervisor) achieved **1,689.43 Events/sec** compared to VMware Workstation's **983.84 Events/sec** — demonstrating a **+71.72% throughput advantage** and a **42.16% reduction** in average latency.

---

## 1. Project Objectives
1. **Deployment:** Provision two identical Ubuntu Virtual Machines across different hypervisor architectures:
   * **Type-1 (Bare-Metal):** Proxmox VE (Kernel-based Virtual Machine / KVM)
   * **Type-2 (Hosted):** VMware Workstation on a Windows Host OS
2. **Standardization:** Enforce uniform hardware resource allocations (2 vCPU, 2048 MB RAM, 20 GB Virtual Storage) to ensure direct comparability.
3. **Benchmarking:** Execute the `sysbench` CPU computational benchmark using 20,000 prime numbers to stress test CPU virtualization efficiency.
4. **Metric Collection:** Capture execution time, total events processed, throughput (events/sec), and latency statistics (min, avg, max).
5. **Architectural Evaluation:** Quantify the performance overhead introduced by host operating system abstraction layers in Type-2 hypervisors versus bare-metal hypervisor execution.

---

## 2. Hypervisor Architectural Comparison

### Type-1 Hypervisor — Proxmox VE (Bare-Metal Architecture)
Proxmox VE runs directly on the bare-metal physical host hardware. The Linux kernel integrated with KVM acts as the hypervisor. Guest operating system instructions execute directly on hardware CPU VT-x/AMD-V extensions without passing through an intermediate desktop operating system.

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
|               Host Operating System (Windows Host OS)             |
+-------------------------------------------------------------------+
|                        Physical PC Hardware                       |
+-------------------------------------------------------------------+
hostnamectl
lscpu
free -h
df -h
sudo apt update && sudo apt install sysbench -y
sysbench cpu --cpu-max-prime=20000 run
