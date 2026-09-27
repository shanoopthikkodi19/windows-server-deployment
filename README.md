# Enterprise Active Directory & Web Infrastructure Project

![Windows Server](https://img.shields.io/badge/WINDOWS_SERVER-2022-2ea44f?style=for-the-badge)
![Active Directory](https://img.shields.io/badge/ACTIVE_DIRECTORY-Configured-2ea44f?style=for-the-badge)
![DNS](https://img.shields.io/badge/DHCP-CONFIGURED-2ea44f?style=for-the-badge)
![DNS](https://img.shields.io/badge/DNS-CONFIGURED-2ea44f?style=for-the-badge)
![IIS](https://img.shields.io/badge/IIS-SSL_Enabled-00244F?style=for-the-badge)
![SSL/TLS](https://img.shields.io/badge/SSL%2FTLS-Installed-2ea44f?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-2ea44f?style=for-the-badge)

# Overview

A self-hosted Project simulating a small business's IT environment on Windows Server 2022 - multi-department Active Directory structure, SSL-secured internal web server, and department - based email communication.

Built to demonstrate practical Windows Server Administartion, AD design, and infrastructure security skills.

## What this covers

- Active Directory Domain Services with department-based OU structure
- User and computer account management across three departments
- Mobile device enrollment for BYOD-style client access
- SSL/TLS-secured internal web server (IIS)
- Department email communication setup
- Basic security hardening (firewall, certificate-based encryption)
---

## Table of Contents

- [Overview](#overview)
- [Network Architecture](#network-architecture)
- [Domain & OU Structure](#domain--ou-structure)
- [Department Breakdown](#department-breakdown)
- [Web Server & SSL](#web-server--ssl)
- [Email Communication](#email-communication)
- [Security Notes](#security-notes)
- [Screenshots](#screenshots)
- [Lessons Learned](#lessons-learned)

---

## Network Architecture
![alt text](/ScreenShot/image.png)

## Domain & OU Structure

| Property | Value |
|---|---|
| Domain name | `shanoop.in` |
| Main network | `172.16.0.0/16` |
| Domain controller | `DC01` (AD DS + DNS) |
| Web server | `WEB01` (IIS, SSL bound to the domain) |

| OU | Subnet | Purpose |
|---|---|---|
| Sales Team | `172.16.1.0/24` | Sales staff accounts and department-owned client machines |
| Finance Team | `172.16.2.0/24` | Finance staff accounts and department-owned mobile devices |
| Technical Team | `172.16.3.0/24` | IT/technical staff accounts and department-owned mobile devices |

Each OU has a linked security group used for NTFS share permissions and Group Policy scoping.

---

## Department Breakdown

| Department | OU | Subnet | Users | Clients | Email domain |
|---|---|---|---|---|---|
| Sales team | Sales Team | `172.16.1.0/24` | `admin`, `junior` | 2 domain-joined machines | `@sales.shanoop.in` |
| Finance team | Finance Team | `172.16.2.0/24` | `admin`, `emp1`, `emp2` | 2 mobile devices | `@finance.shanoop.in` |
| Technical team | Technical Team | `172.16.3.0/24` | `admin`, `junior1`, `junior2`, `junior3` | 2 mobile devices | `@tech.shanoop.in` |

> **Note:** Sales clients are domain-joined PCs authenticating directly against AD. Finance and Technical clients are mobile devices, connecting via mail/web protocols (ActiveSync, HTTPS) rather than a traditional domain join — the standard pattern for mobile access in a real environment. Each department's `admin` account has delegated control scoped to its own OU only, not domain-wide admin rights.

---

## Web Server & SSL

- IIS installed on `WEB01`, hosting the internal site at the main domain (`https://shanoop.in`)
- SSL/TLS certificate installed and bound to port 443
- HTTP → HTTPS redirect enforced
- Verified via browser padlock check from both domain-joined PCs and mobile clients

## Email Communication

- Every user has a mailbox under their department's subdomain (e.g. `admin@sales.shanoop.in`, `emp1@finance.shanoop.in`, `junior1@tech.shanoop.in`)
- Internal mail server configured for cross-department communication
- Mail traffic secured using the same SSL/TLS certificate as the web server

## Security Notes

- Each department subnet is firewalled separately, default-deny inbound
- Department admin accounts limited to delegated control over their own OU
- Mobile devices restricted to mail/web access only — no direct file share access

---

## Screenshots

*(Add screenshots here as you build — ADUC OU view, GPO settings, IIS binding, SSL certificate details, mobile device enrollment)*

```markdown
![Network Architecture](../images/network-architecture.png)
![OU Structure](../images/ou-structure.png)
![SSL Certificate](../images/ssl-certificate.png)
```

## Lessons Learned

*(Fill in once complete — what you'd change for a production deployment, challenges hit, etc. This section is what makes the writeup read as real experience rather than a tutorial copy.)*
