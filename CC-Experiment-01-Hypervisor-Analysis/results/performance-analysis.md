# Performance Analysis: Type-1 (Proxmox VE) vs Type-2 (VMware Workstation)

## 1. Experimental Overview

This benchmark evaluates the compute performance and virtualization overhead between a bare-metal Type-1 hypervisor (Proxmox VE / KVM) and a hosted Type-2 hypervisor (VMware Workstation).

Both environments were allocated identical virtual hardware resources:

* **vCPUs:** 2
* **Memory:** 2048 MB (2 GB)
* **Disk:** 20 GB
* **Guest Operating System:** Ubuntu 22.04.5 LTS (x86_64)

Benchmarking was performed using the standard Sysbench CPU compute test:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

## 2. Comparative Benchmark Observations

| Metric / Parameter         | Type-1 Hypervisor (Proxmox VE) | Type-2 Hypervisor (VMware Workstation) | Impact / Observations                                                  |
| -------------------------- | -----------------------------: | -------------------------------------: | ---------------------------------------------------------------------- |
| **Total Execution Time**   |                      10.0006 s |                              10.0010 s | Both tests completed within the approximately 10-second test duration. |
| **Total Number of Events** |                         16,903 |                                  9,842 | Type-1 processed approximately 71.7% more events in this benchmark.    |
| **Events per Second**      |                       1,689.43 |                                 983.84 | Type-1 achieved higher compute throughput.                             |
| **Average Latency**        |                        0.59 ms |                                1.02 ms | Type-1 showed lower average latency per event.                         |
| **Minimum Latency**        |                        0.57 ms |                                0.81 ms | Type-1 showed a lower minimum latency.                                 |
| **Maximum Latency**        |                        1.09 ms |                                4.38 ms | Type-2 showed a larger latency variation during the test.              |
| **Virtualization Mode**    |          Hardware-assisted KVM |                  VMware VMM on Host OS | The two environments use different virtualization architectures.       |

## 3. Analysis and Key Findings

### 3.1 Architectural Overhead

**Type-1 (Proxmox VE):**
Proxmox VE uses the KVM virtualization framework and operates directly on the physical hardware. This reduces the additional abstraction associated with a general-purpose host operating system and allows virtual CPUs to be scheduled efficiently on the physical processor.

**Type-2 (VMware Workstation):**
VMware Workstation runs as an application on top of a host operating system. The guest virtual machine therefore shares the host system's CPU resources with the host OS and other applications. This can introduce additional scheduling overhead.

### 3.2 Throughput and Latency

Proxmox VE achieved **1,689.43 events per second**, while VMware Workstation achieved **983.84 events per second** in this experiment.

The average latency was **0.59 ms** for Proxmox VE compared with **1.02 ms** for VMware Workstation.

The observed latency range was also narrower on Proxmox VE (**0.57–1.09 ms**) compared with VMware Workstation (**0.81–4.38 ms**). The wider variation in the Type-2 environment may be associated with host operating system scheduling and background activity.

## 4. Conclusion

Based on this benchmark:

1. **Type-1 Hypervisor:** Proxmox VE demonstrated higher CPU throughput and lower latency in the tested configuration.
2. **Type-2 Hypervisor:** VMware Workstation provided a convenient environment for development, testing, and educational use, but showed lower CPU throughput and higher latency in this particular experiment.
3. **Overall Observation:** The results demonstrate that virtualization architecture and host-system overhead can affect VM compute performance. However, the results are specific to the tested hardware, software versions, VM configuration, and system workload.
