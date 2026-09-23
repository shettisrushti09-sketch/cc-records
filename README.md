# Cloud Computing Lab – Experiment 1

## Comparative Performance Analysis of Type-1 and Type-2 Hypervisors

**Type-1:** Proxmox VE (KVM Bare-Metal)
**Type-2:** VMware Workstation (Hosted)

---

## 1. Objective

To deploy and configure Ubuntu Linux 22.04 LTS virtual machines on both a Type-1 bare-metal hypervisor (Proxmox VE) and a Type-2 hosted hypervisor (VMware Workstation), configure matching virtual hardware resources, and evaluate their CPU compute performance using the Sysbench benchmarking utility.

---

## 2. Environment and Resource Allocation

| Hardware / Resource        | Type-1: Proxmox VE                 | Type-2: VMware Workstation  |
| -------------------------- | ---------------------------------- | --------------------------- |
| **Hypervisor Platform**    | Proxmox VE (KVM Bare-Metal)        | VMware Workstation (Hosted) |
| **Host Operating System**  | None – runs directly on bare metal | Windows Host OS             |
| **Guest Operating System** | Ubuntu 22.04.5 LTS (x86_64)        | Ubuntu 22.04.5 LTS (x86_64) |
| **Virtual CPUs**           | 2 vCPUs                            | 2 vCPUs                     |
| **Allocated Memory**       | 2048 MB (2 GB)                     | 2048 MB (2 GB)              |
| **Virtual Disk**           | 20 GB                              | 20 GB                       |
| **Benchmark Tool**         | Sysbench                           | Sysbench                    |

---

# 3. Part A – Type-1 Hypervisor: Proxmox VE

## 3.1 Proxmox VE Dashboard and VM Configuration

The Proxmox VE web management interface was used to create and configure the Ubuntu virtual machine.

The VM was configured with:

* **VM Name:** `CC-Exp1-Type1`
* **VM ID:** `124`
* **CPU:** 2 vCPUs
* **Memory:** 2 GB
* **Disk:** 20 GB
* **Guest OS:** Ubuntu 22.04.5 LTS

### Proxmox Dashboard

![Proxmox Dashboard](screenshots/type1-promox/01-proxmox-dashboard.png)

### Proxmox VM Hardware Configuration

![Proxmox VM Configuration](screenshots/type1-promox/02-proxmox-vm-configuration.png)

### Proxmox VM Running

![Proxmox VM Running](screenshots/type1-promox/03-proxmox-vm-running.png)

---

## 3.2 Guest System Verification and Benchmarking

The Ubuntu guest operating system was accessed through the Proxmox noVNC console.

The following commands were used to verify the guest system:

```bash
hostnamectl
lscpu
free -h
df -h
```

CPU performance was evaluated using:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

### Ubuntu Console

![Proxmox Ubuntu Console](screenshots/type1-promox/04-proxmox-ubuntu-console.png)

### Proxmox Guest System Configuration

![Proxmox System Configuration](screenshots/type1-promox/05-proxmox-system-configuration.png)

### Proxmox Sysbench CPU Benchmark

![Proxmox Sysbench Result](screenshots/type1-promox/06-promox-sysbench-res)
