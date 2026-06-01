# Microsoft Hyper-V Lab

Virtualized lab environment with Nested Virtualization, Live Migration, and Active Directory.

---

## Environment

- Host: Windows 11 Pro with Hyper-V enabled
- Nested virtualization running HOST1 and HOST2 as virtual Hyper-V hosts
- Domain: lab.local (managed by DC1)

---

## Infrastructure

| Server | RAM | Role |
|---|---|---|
| DC1 | 4 GB | Active Directory + DNS |
| HOST1 | 10 GB | Primary Hyper-V host |
| HOST2 | 8 GB | Secondary Hyper-V host |
| Ubuntu VM | 2 GB | Live Migration test workload |
| Windows Server VM | 4 GB | Live Migration test workload |

---

## Network Design

| Network | Type | Purpose |
|---|---|---|
| Management | External | Administration and Windows Admin Center |
| AD-Network | Internal | Active Directory and domain traffic |
| Migration | Internal | Live Migration traffic between hosts |

### IP Planning

| Server | Network | IP Address | Role |
|---|---|---|---|
| DC1 | AD-Network | 10.0.0.10 | Active Directory + DNS |
| HOST1 | AD-Network | 10.0.0.11 | Hyper-V Host |
| HOST2 | AD-Network | 10.0.0.12 | Hyper-V Host |
| HOST1 | Migration | 172.16.0.11 | Migration |
| HOST2 | Migration | 172.16.0.12 | Migration |

---

## Key Steps

### 1. Nested Virtualization
```powershell
Set-VMProcessor -VMName "HOST1" -ExposeVirtualizationExtensions $true
Set-VMProcessor -VMName "HOST2" -ExposeVirtualizationExtensions $true
```

### 2. Hyper-V Role Installation
```powershell
Install-WindowsFeature Hyper-V -IncludeManagementTools -Restart
```

### 3. PowerShell Remoting
```powershell
Enable-PSRemoting -Force
Set-Item WSMan:\Localhost\Client\TrustedHosts -Value "DC1,HOST1,HOST2" -Force
```

### 4. Active Directory
- AD DS installed on DC1 via Server Manager
- New forest created: lab.local
- HOST1 and HOST2 joined to domain

### 5. Kerberos Constrained Delegation
- Service: Microsoft Virtual System Migration Service
- Service: cifs
- Both servers configured bidirectionally

### 6. Live Migration
```powershell
Move-VM -Name "ServerVM" -DestinationHost "HOST2" -IncludeStorage -DestinationStoragePath "E:\VMS"
```

### 7. Checkpoints
- Checkpoint BeforeTest_User created on DC1
- Test user created in Active Directory
- Checkpoint After-TestUser created
- Restored to BeforeTest_User — user no longer existed, confirming rollback worked

---

## Storage Structure



E:\VMS
├── Config
├── ISO
├── Snapshots
└── VHDX\



---

## Summary

This lab demonstrated how to build a full nested Hyper-V environment with centralized authentication via Active Directory, 
remote management via Windows Admin Center and PowerShell, 
Live Migration between two Hyper-V hosts using Kerberos, and checkpoint creation and rollback.
