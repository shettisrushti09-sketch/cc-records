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
