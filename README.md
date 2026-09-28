# Active Directory & Windows Security Lab
Hands on Active Directory and Windows security lab built with Windows Server 2025, Group Policy, DNS, and a domain joined Windows 11 client.
## Overview

This project demonstrates the deployment and security configuration of a small Windows domain environment using VirtualBox. The lab includes a Windows Server 2025 domain controller, Active Directory Domain Services, DNS, Group Policy, and a domain joined Windows 11 client.

The goal of the lab was to build a functional Windows domain while implementing basic security controls for identity management, password policy, account lockout, auditing, and authentication monitoring.

## Objectives

- Deploy Windows Server 2025 as an Active Directory domain controller.
- Configure Active Directory Domain Services (AD DS) and DNS.
- Create organizational units, users, and security groups.
- Configure and apply a Group Policy security baseline.
- Join a Windows 11 client to the domain.
- Implement password and account lockout policies.
- Configure Windows security auditing.
- Validate security controls using Resultant Set of Policy (RSoP).
- Generate and investigate a failed authentication event (Event ID 4625).

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Domain Controller | Windows Server 2025 Standard Evaluation |
| Client | Windows 11 |
| Domain | corp.lab |
| Domain Controller | AD-DC01 |
| Client Computer | CLIENT01 |
| Directory Services | Active Directory Domain Services (AD DS) |
| DNS | Windows Server DNS |
| Network | VirtualBox NAT + Host-Only |
| Host-Only Subnet | 192.168.56.0/24 |

## Architecture

The lab was built in VirtualBox using two Windows virtual machines connected through a Host Only network.

```text
Windows 11 Host
       │
       └── VirtualBox
             │
             ├── AD-DC01
             │    ├── Windows Server 2025
             │    ├── Active Directory Domain Services
             │    ├── DNS
             │    └── corp.lab
             │
             └── CLIENT01
                  ├── Windows 11
                  └── Domain-Joined
```

## Active Directory Structure

The domain environment was organized using dedicated organizational units (OUs) for users, computers, and security groups.

### Organizational Units

- `Lab-Users`
- `Lab-Computers`
- `Lab-Groups`

### Users

| User | Account |
|---|---|
| John Smith | `john.smith` |
| Sarah Johnson | `sarah.johnson` |
| Mike Davis | `mike.davis` |
| Emily Brown | `emily.brown` |

### Security Groups

| Group | Members |
|---|---|
| IT-Admins | John Smith |
| Helpdesk | Sarah Johnson |
| VPN-Users | Mike Davis, Emily Brown |

## Group Policy Security Baseline

A domain level Group Policy Object (GPO) named `GPO - Security Baseline` was created to establish baseline security controls across the Windows environment.

### Password Policy

- Password history: 5 passwords remembered
- Maximum password age: 90 days
- Minimum password age: 1 day
- Minimum password length: 12 characters
- Password complexity: Enabled
- Reversible password encryption: Disabled

### Account Lockout Policy

- Account lockout threshold: 5 invalid attempts
- Account lockout duration: 15 minutes
- Reset account lockout counter after: 15 minutes

### Advanced Audit Policy

Auditing was configured for:

- Account Logon
- Account Management
- Logon/Logoff
- Policy Change
- Authentication activity
- Security group management
- User account management

## Validation & Testing

Security controls were validated from the domain joined Windows 11 client.

### Group Policy Validation

Resultant Set of Policy (RSoP) was used to confirm that the domain security baseline was being applied to the client.

The following controls were verified:

- Password policy settings
- Account lockout settings
- Domain security policy configuration

### Authentication Testing

A controlled failed login attempt was performed using a domain account to validate Windows security auditing.

Windows Event Viewer recorded:

- **Event ID:** 4625
- **Event Type:** Audit Failure
- **Account:** `john.smith`
- **Domain:** `CORP`
- **Failure Reason:** Unknown user name or bad password
- **Computer:** `CLIENT01.corp.lab`

This demonstrated that failed authentication activity was being captured in the Windows Security event log.

## Screenshots

The following screenshots document the implementation and validation of the lab.

### Active Directory & Domain Configuration

- Server initial configuration
- Active Directory Domain Services installation
- Domain controller promotion
- Domain configuration and verification
- DNS resolution

### Active Directory Structure

- Organizational units
- Test users
- Security groups
- Group memberships

### Security Configuration

- Password policy
- Security baseline GPO
- GPO application verification
- Domain security policy verification

### Client & Security Validation

- RSoP password policy verification
- Failed authentication Event ID 4625

## Challenges & Troubleshooting

Several issues were encountered during the lab and resolved during implementation.

- Configured VirtualBox networking to provide both Internet connectivity and private communication between the domain controller and client.
- Troubleshot DNS resolution to ensure the Windows 11 client could locate the `corp.lab` domain controller.
- Resolved a Windows 11 domain join issue related to domain connectivity and name resolution.
- Troubleshot VirtualBox storage and disk space issues that temporarily affected the client virtual machine.
- Used RSoP to distinguish effective domain Group Policy settings from local security policy configuration.

## Lessons Learned

This project provided hands on experience with Windows domain infrastructure and foundational security controls.

Key takeaways included:

- Understanding the role of Active Directory Domain Services in a Windows domain environment.
- Managing users, organizational units, and security groups.
- Using Group Policy to establish centralized security controls.
- Understanding the relationship between DNS and Active Directory.
- Using RSoP to verify effective Group Policy configuration.
- Using Windows Security logs to investigate authentication failures.
- Applying troubleshooting techniques to resolve networking, DNS, and domain connectivity issues.

## Technologies Used

- Windows Server 2025
- Windows 11
- Active Directory Domain Services (AD DS)
- Active Directory Users and Computers (ADUC)
- DNS
- Group Policy
- Resultant Set of Policy (RSoP)
- Windows Event Viewer
- VirtualBox
