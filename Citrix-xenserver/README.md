# Citrix XenServer Lab

Virtualized lab environment with XenServer, virtual machines, and centralized management via XenCenter.

---

## Environment

- VMware Workstation → XenServer VM (nested virtualization)
- Two XenServer hosts: HPV01 and HPV02
- Managed via XenCenter

---

## Infrastructure

| Component | Details |
|---|---|
| CPU | 8 vCPU |
| RAM | 10 GB |
| System disk | 50 GB |
| VM storage disk | 120 GB |
| Network adapters | 3 (Management, VM traffic, reserved) |

---

## Network Design

| Network | NIC | Purpose |
|---|---|---|
| Management Network | NIC0 | Hypervisor administration via XenCenter |
| VMS-NETWORK | NIC1 | Virtual machine traffic |
| VMS-TRAFIK | NIC1 | Additional VM traffic |
| Network 2 | NIC2 | Reserved for future use |

---

## Storage

### ISO Storage Repository
- ISO files stored on host PC and shared via SMB
- ISO SR created in XenCenter using SMB/CIFS connection

### VM Storage Repository
Created via CLI:
```bash
xe sr-create name-label="VM-Storage" type=lvm content-type=user device-config:device=/dev/nvme0n1
```
- 120 GB disk (nvme0n1) used for VM storage
- Separated from system disk for better structure and performance
- Thin provisioning selected during installation

---

## Virtual Machines Installed

### Ubuntu 24.04 LTS
| Resource | Value |
|---|---|
| vCPU | 2 |
| RAM | 4 GB |
| Disk | 20 GB |
| Network | VMS-NETWORK (External) |
| Boot | UEFI |

### Windows Server 2025
| Resource | Value |
|---|---|
| vCPU | 2 |
| RAM | 4 GB |
| Disk | 64 GB |
| Network | VMS-NETWORK (External) |
| Boot | UEFI Secure Boot |

XenServer VM Tools installed on Windows Server for better performance.

---

## Snapshots

Demonstrated on both Ubuntu and Windows Server VMs:
1. Open VM in XenCenter → Snapshots tab
2. Click **Take Snapshot** and give it a name
3. Snapshot saves disk state and can be used to restore the VM

---

## XenMotion – Live Migration

Migrated Ubuntu VM from HPV02 to HPV01 while the VM was running.

### Steps
1. Right-click VM → Migrate
2. Select destination host (HPV01)
3. Select local storage on destination
4. Map virtual network adapter to matching network on destination
5. Select management network for migration traffic
6. Start migration

### Result
Migration completed successfully with Ubuntu running throughout. VM appeared on HPV01 after migration confirmed.

---

## Summary

- Citrix Hypervisor installed in nested virtualization environment
- Two XenServer hosts configured and managed via XenCenter
- Separate storage repositories for ISO files (SMB) and VMs (LVM)
- Ubuntu 24.04 and Windows Server 2025 installed and configured
- Snapshots demonstrated for both VMs
- XenMotion live migration completed successfully between HPV01 and HPV02
