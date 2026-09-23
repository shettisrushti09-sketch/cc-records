# Cloud Computing Lab – Experiment 1

## Performance Analysis of Type-1 and Type-2 Hypervisors

### Proxmox VE (Type-1) vs VMware Workstation (Type-2)

[![Course](https://img.shields.io/badge/Course-Cloud%20Computing-blue.svg)](#)
[![Hypervisors](https://img.shields.io/badge/Hypervisors-Proxmox%20VE%20%7C%20VMware%20Workstation-orange.svg)](#)
[![Benchmark](https://img.shields.io/badge/Benchmark-Sysbench%20CPU-green.svg)](#)
[![Status](https://img.shields.io/badge/Status-Completed-brightgreen.svg)](#)

---

## Executive Summary

This repository contains the experimental setup, configuration details, screenshots, benchmark results, and performance comparison of a **Type-1 hypervisor (Proxmox VE)** and a **Type-2 hypervisor (VMware Workstation)**.

Both environments were tested using an Ubuntu virtual machine with the same allocated resources:

- 2 vCPU
- 2 GB RAM
- 20 GB virtual disk

The CPU performance of both virtual machines was evaluated using the **Sysbench CPU benchmark** with the following command:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

---

## Hypervisor Architecture

### Type-1 Hypervisor – Proxmox VE

```text
+-------------------------------------------------------------+
|                 Ubuntu Virtual Machine                      |
|                                                             |
|                 Sysbench CPU Benchmark                      |
+-------------------------------------------------------------+
|                  Proxmox VE / KVM                           |
|                  Type-1 Hypervisor                          |
+-------------------------------------------------------------+
|                  Physical Host Hardware                     |
|                    CPU / RAM / Storage                      |
+-------------------------------------------------------------+
```

### System Verification and Benchmark Commands

```bash
hostnamectl
lscpu
free -h
df -h
top
sudo apt update
sudo apt install sysbench -y
sysbench --version
sysbench cpu --cpu-max-prime=20000 run
```

---

## Repository Structure

```text
cc-records/
│
├── README.md
│
└── CC-Experiment-01-Hypervisor-Analysis/
    │
    ├── images/
    ├── results/
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
            ├── 03-vmware-system-configuration.png
            └── 04-vmware-sysbench-result.png
