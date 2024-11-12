# Prerequisites

In this lab you will review the machine requirements necessary to follow this tutorial.

## Virtual or Physical Machines

This tutorial requires four (4) virtual or physical AMD64 machines running Debian 12 (bookworm). The follow table list the four machines and their CPU, memory, and storage requirements.

| Name    | Description            | CPU | RAM   | Storage |
|---------|------------------------|-----|-------|---------|
| jumpbox | Administration host    | 1   | 512MB | 10GB    |
| server  | Kubernetes server      | 1   | 2GB   | 20GB    |
| node-0  | Kubernetes worker node | 1   | 2GB   | 20GB    |
| node-1  | Kubernetes worker node | 1   | 2GB   | 20GB    |

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
