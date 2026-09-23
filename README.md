# Cloud Computing Lab – Experiment 1

## Comparative Performance Analysis of Type-1 and Type-2 Hypervisors

### Proxmox VE (Type-1) vs VMware Workstation (Type-2)

---

## 1. Objective

The objective of this experiment is to perform a comparative performance analysis of:

- **Type-1 Hypervisor:** Proxmox VE
- **Type-2 Hypervisor:** VMware Workstation

Both hypervisors are evaluated using an identically configured Ubuntu virtual machine.

CPU performance is measured using the **Sysbench CPU benchmark**.

The experiment compares:

- Total execution time
- Total number of events
- Events per second
- Minimum latency
- Average latency
- Maximum latency

---

## 2. Experimental Environment

| Parameter | Configuration |
|---|---|
| Guest Operating System | Ubuntu |
| CPU Allocation | 2 vCPU |
| Memory Allocation | 2 GB |
| Virtual Disk | 20 GB |
| Benchmark Tool | Sysbench |
| CPU Benchmark | `sysbench cpu --cpu-max-prime=20000 run` |
| Type-1 Hypervisor | Proxmox VE |
| Type-2 Hypervisor | VMware Workstation |

---

# 3. Part A – Type-1 Hypervisor: Proxmox VE

Proxmox VE is used as the Type-1 hypervisor for the first part of the experiment.

The Ubuntu virtual machine was configured with:

- 2 vCPU
- 2 GB RAM
- 20 GB virtual disk
- Ubuntu guest operating system

The VM configuration and CPU performance were verified using system commands and Sysbench.

## 3.1 Proxmox VE Dashboard

![Proxmox Dashboard](CC-Experiment-01-Hypervisor-Analysis/screenshots/type1-promox/01-proxmox-dashboard.png)

---

## 3.2 Proxmox VM Configuration

![Proxmox VM Configuration](CC-Experiment-01-Hypervisor-Analysis/screenshots/type1-promox/02-proxmox-vm-configuration.png)

---

## 3.3 Proxmox VM Running

![Proxmox VM Running](CC-Experiment-01-Hypervisor-Analysis/screenshots/type1-promox/03-proxmox-vm-running.png)

---

## 3.4 Ubuntu Console

![Proxmox Ubuntu Console](CC-Experiment-01-Hypervisor-Analysis/screenshots/type1-promox/04-proxmox-ubuntu-console.png)

---

## 3.5 System Configuration

![Proxmox System Configuration](CC-Experiment-01-Hypervisor-Analysis/screenshots/type1-promox/05-proxmox-system-configuration.png)

The system configuration was verified using:

```bash
hostnamectl
lscpu
free -h
df -h

## 3.6 Sysbench CPU Benchmark

The following command was used:

```bash
sysbench cpu --cpu-max-prime=20000 run
