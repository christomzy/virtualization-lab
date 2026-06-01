# Comparative Analysis – Hyper-V, XenServer & VMware ESXi

---

## Overview

Three virtualization platforms were installed, configured, and compared:

- Microsoft Hyper-V
- Citrix XenServer
- VMware ESXi

The comparison covers architecture, networking, storage, migration, and administration.

---

## Platform Comparison Table

| Feature | Hyper-V | XenServer | VMware ESXi |
|---|---|---|---|
| **Hypervisor type** | Type-1 (integrated with Windows) | Type-1 (bare-metal) | Type-1 (bare-metal) |
| **Management tool** | Windows Admin Center / PowerShell | XenCenter | VMware Host Client |
| **Live migration** | Live Migration (Kerberos + Constrained Delegation) | XenMotion (GUI-based) | vMotion (not tested) |
| **Networking** | External/Internal virtual switches | Virtual networks via XenCenter | vSwitches, Port Groups, VMkernel NICs |
| **Storage format** | VHDX files | Storage Repositories (LVM/SMB) | VMFS Datastore |
| **Snapshots** | Checkpoints | Snapshots via XenCenter | Snapshots via Host Client |
| **Windows integration** | Excellent (native AD, PowerShell) | Good | Good |
| **Complexity** | Medium | Low–Medium | Medium–High |

---

## Architecture

### Hyper-V
- Tightly integrated with Windows Server and Active Directory
- Managed via Windows Admin Center and PowerShell
- Best choice for Windows-heavy environments

### XenServer
- Bare-metal hypervisor based on Xen Project
- Simple and clean management via XenCenter GUI
- Easy to get started with

### VMware ESXi
- Bare-metal hypervisor widely used in enterprise environments
- Most advanced network and storage structure of the three
- Full control over vSwitches, Port Groups, VMkernel, and datastores

---

## Networking

### Hyper-V
- External and Internal virtual switches
- Separate networks for Management, AD traffic, and Live Migration
- Network profiles managed via PowerShell

### XenServer
- Virtual networks tied to physical NICs
- Simple configuration via XenCenter
- Clear overview of VM connections

### VMware ESXi
- Most advanced networking of the three platforms
- Full topology: VM → Port Group → vSwitch → Physical NIC
- VMkernel NICs separate host traffic from VM traffic

---

## Storage

### Hyper-V
- VHDX virtual hard disk files
- Structured local folders for ISO, snapshots, config, and VHDXs
- Storage managed via Windows and PowerShell

### XenServer
- Storage Repositories (SR) for VMs and ISO files
- ISO SR connected via SMB share
- VM SR created via CLI using LVM on dedicated disk

### VMware ESXi
- VMFS 6 datastore on dedicated disk
- Datastore Browser for file and folder management
- Clean separation between ISO files and VM files

---

## Live Migration

| Platform | Method | Authentication | Result |
|---|---|---|---|
| Hyper-V | PowerShell `Move-VM` | Kerberos + Constrained Delegation | ✅ Successful |
| XenServer | XenCenter GUI | Built-in | ✅ Successful |
| VMware ESXi | vMotion | Not configured | ❌ Not tested |

---

## Snapshots / Checkpoints

All three platforms support snapshots with similar workflows:
1. Create snapshot before changes
2. Make changes to the VM
3. Restore to previous snapshot
4. Verify changes are gone

| Platform | Name | Tool Used |
|---|---|---|
| Hyper-V | Checkpoints | Hyper-V Manager |
| XenServer | Snapshots | XenCenter |
| VMware ESXi | Snapshots | VMware Host Client |

---

## Strengths and Weaknesses

| Platform | Strengths | Weaknesses |
|---|---|---|
| Hyper-V | Best Windows/AD integration, PowerShell automation | More dependent on Windows ecosystem |
| XenServer | Easiest migration via GUI, simple setup | Less common in enterprise than VMware |
| VMware ESXi | Most advanced networking and storage, enterprise standard | Some features require paid licensing |

---

## Conclusion

All three platforms can create and manage virtual environments, but each has different strengths:

- **Hyper-V** is the best choice for Windows-based environments with Active Directory
- **XenServer** offers the simplest live migration experience through XenCenter
- **VMware ESXi** provides the most advanced and structured approach to networking and storage, making it the strongest enterprise platform of the three
