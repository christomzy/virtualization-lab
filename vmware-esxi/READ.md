# VMware ESXi Lab

Virtualized lab environment with VMware ESXi 8.0.3, virtual machines, and network/storage configuration.

---

## Environment

- VMware Workstation Pro → ESXi 8.0.3 VM (nested virtualization)
- Managed via VMware Host Client (web browser)

---

## Resource Planning

| Resource | Allocation |
|---|---|
| CPU | 4 vCPU |
| RAM | 12 GB |
| Network adapters | 2 |
| System disk | 80 GB |
| VM storage disk | 100 GB |

---

## Network Design

| Component | Details |
|---|---|
| vSwitch0 | Management Network → vmnic0 |
| vSwitch VM | VM Network → vmnic1 |
| vmk0 | Management adapter, IP: 192.168.0.60 |

### Network Topology

Virtual Machine
↓
Port Group
↓
vSwitch
↓
Physical NIC


### Management Network Configuration
- IP Address: 192.168.0.60
- Subnet mask: 255.255.255.0
- Default gateway: 192.168.0.1

---

## Storage

### VMFS Datastore
- Created on separate 100 GB virtual disk
- Format: VMFS 6
- Name: vms-datastore
- Used full disk option

### Folder Structure



vms-datastore/
├── linux/       ← Linux ISO files
├── windows/     ← Windows ISO files
├── others/      ← Other files
└── vms/         ← Virtual machine files


---

## Virtual Machines Installed

### Windows Server 2025
| Resource | Value |
|---|---|
| Name | Test1-WindowsServer |
| vCPU | 2 |
| RAM | 4 GB |
| Disk | 60 GB |
| Network | VM Network |
| OS | Windows Server 2025 Standard Evaluation (Desktop Experience) |

- Virtualization Based Security (VBS) enabled
- VMware Tools installed (Complete)

### Ubuntu Server
| Resource | Value |
|---|---|
| Name | Ubuntu-Core |
| vCPU | 2 |
| RAM | 4 GB |
| Disk | 25 GB |
| Network | VM Network |
| OS | Ubuntu Linux 64-bit |

- LVM used for storage management
- VMXNET3 network adapter
- VMware Paravirtual SCSI controller

---

## Snapshots

Tested on Windows Server VM:

1. Created snapshot **Before_Folder** before any changes
2. Created a new folder on the Windows Server desktop
3. Created snapshot **After_Folder** after the change
4. Restored **Before_Folder** — folder no longer existed, confirming rollback
5. Deleted all snapshots to consolidate disk and free storage

---

## Key Concepts Explored

### VMkernel NICs
- vmk0 used for Management Network
- Handles ESXi host system traffic (management, vMotion, storage)

### Physical NICs
- vmnic0 → vSwitch0 (Management)
- vmnic1 → vSwitch VM (VM traffic)
- Both running at 10000 Mbps full duplex

### Virtual Switches
- vSwitch0: Management Network
- vSwitch VM: VM Network with Port Group

### Port Groups
- Management Network → vSwitch0
- VM Network → vSwitch VM

---

## Summary

- VMware ESXi 8.0.3 installed in nested virtualization environment
- VMFS 6 datastore created on dedicated 100 GB disk
- Separate vSwitches for management and VM traffic
- Windows Server 2025 and Ubuntu Server installed and configured
- Snapshot workflow fully tested: create, change, restore, delete
- VMware Tools installed on Windows Server for better performance

