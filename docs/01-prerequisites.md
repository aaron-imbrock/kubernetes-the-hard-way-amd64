# Prerequisites

In this lab you will review the machine requirements necessary to follow this tutorial.

## Virtual or Physical Machines

This tutorial requires four (4) virtual or physical AMD64 machines running Debian 12 (bookworm). The follow table lists the four machines and their CPU, memory, and storage requirements. Any IPs can be used but for this lab the host IPs are 10.0.0.20-23/32.

| Name    | Description            | CPU | RAM   | Storage | IPV4_ADDRESS | POD_SUBNET     |
|---------|------------------------|-----|-------|---------|--------------|----------------|
| jumpbox | Administration host    | 1   | 512MB | 10GB    | 10.0.0.20/32 | -              |
| server  | Kubernetes server      | 1   | 2GB   | 20GB    | 10.0.0.21/32 | -              |
| node-0  | Kubernetes worker node | 1   | 2GB   | 20GB    | 10.0.0.22/32 | 192.168.0.0/24 |
| node-1  | Kubernetes worker node | 1   | 2GB   | 20GB    | 10.0.0.23/32 | 192.168.1.0/24 |

How you provision the machines is up to you, the only requirement is that each machine meet the above system requirements including the machine specs and OS version. Once you have all four machine provisioned, verify the system requirements by running the `uname` command on each machine:

```bash
uname -mov
```

After running the `uname` command you should see the following output:

```text
#1 SMP PREEMPT_DYNAMIC Debian 6.1.112-1 (2024-09-30) x86_64 GNU/Linux
```

You maybe surprised to see `x86_64` here, but that is the official name for the 64-bit architecture used by all INTEL and AMD processors made since 2005. This tutorial will use `AMD64` consistently throughout to avoid confusion.

Next: [setting-up-the-jumpbox](02-jumpbox.md)
