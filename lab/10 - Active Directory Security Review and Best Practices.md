# Objective
Conduct a security review of the Nexora Technologies Active Directory environment, identify potential security weaknesses, and implement improvements to domain password and account lockout policies.
This lab focuses on privileged access management, Active Directory account security, password policy hardening, and verification of security changes.

# Environment
| Component | Configuration |
| :--- | :--- |
| Domain | ad.nexora.test |
| Domain Controller | DC01 (Windows Server 2025) |
| Directory Services | Active Directory Domain Services |
| Management Tools | PowerShell, Active Directory Users and Computers, Group Policy Management |
| Group Policy | Default Domain Policy |

# I. Reviewing Privileged Active Directory Groups
Reviewed the membership of several privileged Active Directory security groups to identify accounts with elevated permissions.

Used PowerShell (Administrator) to execute the command:
  - Get-ADGroupMember -Identity "Domain Admins"
    
Repeated the review for other administrative groups

# Privileged Group Membership Results
| Security Group | Direct Members |
| :--- | :--- |
| Domain Admins | Administrator |
| Enterprise Admins | Administrator |
| Schema Admins | Administrator |
| Account Operators | None |
| Backup Operators | None |
| Server Operators | None |
| Group Policy Creator Owners | Administrator |

Findings:
- No regular employee accounts were found as direct members of the reviewed privileged groups.
- The built-in Administrator account holds highly privileged roles.
- Account operators, Backup operators, and Server operators have no direct members.
- Schema Admins includes the built-in Administrator account, representing a potential opportunity to reduce standing privileges.

Security recommendation:
- Apply the principle of least privilege by restricting administrative group membership to accounts that require those permissions. Review privileged group memberships regularly and remove unnecessary standing privileges.

# II. Reviewing Active Directory Accounts
Used PowerShell (Administrator) to enumerate domain user accounts and review their status:
- Get-ADUser -Filter * | Select-Object Name, SamAccountName, Enabled | Sort-Object Name | Format-Table -AutoSize

# Account Inventory
| Username | Account Type |
| :--- | :--- |
| Administrator | Built-in administrator |
| jsmith | Sales employee |
| sjohnson | HR employee |
| mbrown | Accounting employee |
| dwilson | IT employee |
| edavis | Management employee |
| Guest | Built-in guest |
| krbtgt | Kerberos service account |

Confirmed that the Guest and krbtgt were disabled. The 5 employee accounts were present in Active Directory.

# Password Expiration Review
Used PowerShell (Administrator) to check accounts using:
- Get-ADUser -Filter * -Properties PasswordNeverExpires | Select-ObjectName, SamAccountName, Enabled, PasswordNeverExpires | Format-Table -AutoSize

Identified two accounts with Password Never Expires enabled:
- Administrator
- Guest

# III. Reviewing the Built-In Administrator Account
Used PowerShell (Administrator) to inspect the built-in Administrator account.
- Get-ADUser Administrator -Properties Enabled, PasswordNeverExpires, PasswordLastSet, LastLogonDate, SmartcardLogonRequired | Select-Object Name, Enabled, PasswordNeverExpires, PasswordLastSet, LastLogonDate, SmartcardLogonRequired | Format-List

# Findings
| Property | Result |
| :--- | :--- |
| Enabled | True |
| Password Never Expires | True |
| Password Last Set | October 19, 2026 |
| Smartcard Logon Required | False |

The account was enabled and belonged to highly privileged administrative groups.

Security recommendations:
- Reserve the built-in Administrator account for recovery and exceptional administrative tasks.
- Use a separate, dedicated administrative account for normal administration.
- Protect privileged credentials using long, unique passwords.
- Monitor privileged authentication activity.
- Consider phishing-resistant multifactor authentication where supported.
- Review unnecessary membership in Enterprise Admins and Schema Admins.

# IV. Reviewing the Defeault Domain Policy
Used PowerShell (Administrator) to inspect the effective domain password policy:
- Get-ADDefaultDomainPasswordPolicy | Select-Object MinPasswordLength, ComplexityEnabled, PasswordHistoryCount, MaxPasswordAge

# Initial Password Policy
| Setting | Initial Value |
| :--- | :--- |
| Minimum password length | 7 characters |
| Password complexity | Enabled |
| Password history | 24 passwords |
| Maximum password age | 42 days |

Security Finding:
- The miminum password length of 7 characters was considered insufficient for the intended security baseline.
- Longer passwords provide increased resistance to password-guessing attacks.

# V. Strengthening the Password Policy
Updated the Default Domain Policy, linked directly to ad.nexora.test

Navigated to:
Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Account Policies -> Password Policy

Configured:
| Setting | Updated Value |
| :--- | :--- |
| Relax minimum password length limits | Enabled |
| Minimum password length | 15 characters |
| Password complexity | Enabled |
| Password history | 24 passwords |
| Maximum password age | 42 days |

# VI. Configuring Account Lockout Protection
Reviewed the initial Account Lockout Policy.

# Initial Configuration
| Setting | Initial Value |
| :--- | :--- |
| Account lockout threshold | 0 invalid attempts |
| Account lockout duration | 10 minutes (inactive) |
| Reset lockout counter | 10 minutes (inactive) |

A threshold of 0 meant that the default domain policy did not automatically lock accounts after repeated incorrect password attempts.

# Security Improvement
Updated the Default Domain Policy under:

Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Account Policies -> Account Lockout Policy

Configured:
| Setting | Updated Value |
| :--- | :--- |
| Account lockout threshold | 3 invalid attempts |
| Account lockout duration | 15 minutes |
| Reset lockout counter | 15 minutes |

This configuration provides account lockout protection against repeated incorrect password attempts.
A 3 attempt threshold is strict and may increase accidental or deliberately induced account lockouts. Production environments should evaluate this trade-off before adopting the same threshold.

# VII. Security Recommendations
Based on the review, the following improvements are recommended for a production Active Directory environment:

| Area | Recommendation |
| :--- | :--- |
| Privileged accounts | Uses dedicated administrative identities and minimize standing privileges.
| Schema administration | Remove unnecessary permanent Schema admins membership.
| Built-in Administrator | Restrict routine use and monitor authentication activity.
| Password security | Encourage long, unique passwords and use compromised-password screening.
| Password expiration | Review routine expiration requirements against modern guidance.
| Account lockout | Balance password-guessing protection against accidental lockouts.
| Account lifecycle | Review inactive, disabled, and unnecessary accounts periodically.
| Privileged authentication | Implement phishing-resistant MFA where supported.
