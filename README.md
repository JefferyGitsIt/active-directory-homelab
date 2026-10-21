# Nexora Technologies - Active Directory Homelab
A hands-on IT infrastructure project focused on building, configuring, securing, and documenting a fictional company's Windows Server and Active Directory environment.

	Project status: In Progress
	Environment: Personal homelab / virtual lab
	Company: Nexora Technologies (fictional)

# Project Overview
Nexora Technologies is a fictional company with approximately 50 employees across multiple departments. This project simulates the work of an IT administrator responsible for managing the company's users, computers, network resources, and security.
The goal is to build the environment from scratch and document each stage, including configuration decisions, testing, troubleshooting, and lessons learned.
This repository will serve as a record of my practical learning journey in Windows Server administration and IT infrastructure.

# Company Profile
	Company: Nexora Technologies
	Industry: Business technology services
	Company size: Approximately 50 employees
	Headquarters: Denver, Colorado (fictional)
	Planned Active Directory domain: ad.nexora.test
	Environment: Isolated virtual homelab

# Departments
	Human Resources (HR): Employee records and personnel documents
	Accounting: Financial records and invoices
	Sales: Customer accounts and sales documents
	IT: Infrastructure administration and technical support
	Management: Company operations and oversight

# Project Objectives
- [x] Install and configure Windows Server
- [x] Deploy Active Directory Domain Services (AD DS)
- [x] Configure DNS and domain authentication
- [x] Create Organizational Units (OUs) for departments and computers
- [x] Create and manage fictional employee accounts
- [ ] Configure security groups and resource permissions
- [x] Join Windows client machines to the domain
- [x] Configure and test Group Policy Objects (GPOs)
- [ ] Deploy a file server with department-specific access
- [ ] Practice troubleshooting and review Windows event logs
- [ ] Document security improvements and lessons learned

# Planned Infrastructure

The following is the initial design, not a deployed configuration. IP addresses and system specifications will be confirmed during the network planning stage.

	DC01 - Domain controller and DNS
	CLIENT01 - Windows domain-joined workstation
	CLIENT02 - Additional workstation for testing
	FILE01 - File server and departmental shares
	Virtual network - Isolated network for the lab

# Lab Documentation
- [Lab 01 - Windows Server 2025 Installation and Configuration](lab/01%20-%20Windows%20Server%202025%20Installation%20and%20Configuration.md)
- [Lab 02 - Active Directory Domain Services Deployment](lab/02%20-%20Active%20Directory%20Domain%20Services%20Deployment.md)
- [Lab 03 - Active Directory Organizational Units](lab/03%20-%20Active%20Directory%20Organizational%20Units.md)
- [Lab 04 - Active Directory User Accounts and Security Groups](lab/04%20-%20Active%20Directory%20User%20Accounts%20and%20Security%20Groups.md)
- [Lab 05 - Windows 11 Domain Joining and Authentication](lab/05%20-%20Windows%2011%20Domain%20Joining%20and%20Authentication.md)
- [Lab 06 - Multiple Domain Workstations and Active Directory Trust Troubleshooting](lab/06%20-%20Multiple%20Domain%20Workstations%20and%20Active%20Directory%20Trust%20Troubleshooting.md)
- [Lab 07 - Group Policy Configuration and Verification](lab/07%20-%20Group%20Policy%20Configuration%20and%20Verification.md)

# Technologies and Skills
- Windows Server
- Active Directory Domain Services (AD DS)
- DNS
- Group Policy
- Windows client administration
- PowerShell
- NTFS and shared-folder permissions
- Virtualization and networking
- Windows event logging and troubleshooting
- Git and GitHub documentation

# Skills being developed
- User and computer account management
- Identity and access management
- Least-privilege access control
- Centralized workstation administration
- Infrastructure documentation
- Systematic troubleshooting
