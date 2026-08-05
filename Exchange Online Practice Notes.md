# Service Desk Analyst – Hands-on Summary on Exchange Online

## 1. Overview

Exchange Online is Microsoft’s cloud email system. It is part of Microsoft 365. These notes cover the main tasks a Tier 1 Service Desk analyst performs with Exchange Online.

## 2. Platforms We Used

### Microsoft 365 Admin Center

Main place to manage users, licenses, and groups. This is usually the starting point for most Service Desk work.

### Exchange Admin Center

Used for email-specific tasks: mailboxes, shared mailboxes, distribution lists, mail flow rules, and message trace.

### Microsoft Entra (entra.microsoft.com)

Identity platform. Manages users, groups, roles, and permissions across Microsoft 365 and Azure.

## 3. Setting Up the Lab

We used a free Microsoft 365 Business Basic 1-month trial. This gave us a real Exchange Online environment to practice safely without affecting a production system.

The trial includes Microsoft 365 admin center containing end-user apps (Microsoft 365 Copilot, Outlook, OneDrive, Word, Excel, PowerPoint, OneNote, SharePoint, Teams, Viva, Bookings for scheduling, Admin) and admin-centric apps containing Security for Cyber blue team, Microsoft Purview for data governance, data security, and compliance, Exchange, SharePoint, Teams and Power Platform.   
## 4. Creating Users

Steps:
1.              Go to Microsoft 365 Admin Center → Users → Active users
2.              Click Add a user
3.              Enter name and email address
4.              Assign a license
5.              Finish the wizard

**Note:** When you create a user and assign a license that includes Exchange, a mailbox is created automatically.
## 5. Shared Mailbox

A shared mailbox is used by multiple people (for example, a Service Desk inbox). It does not need its own license.

What we did:

•      Created a shared mailbox called “Service Desk”
•      Added an alias (help desk)
•      Gave Read and manage (Full Access) to admin and Send As permissions to newly created service desk people
•      Tested sending and receiving emails from the shared mailbox

## 6. Distribution List

A distribution list is used to send one email to many people at the same time.

What we did:

•      Teams & groups > Active teams > Distribution list (tab)
•      Created a distribution list called VAPT
•      Added users as members
•      Sent a test email to the group address and confirmed members received it
## 7. Mail Flow Rule

Mail flow rules evaluate messages in transit before they reach the inbox. They apply logic based on conditions, actions, and exceptions.

What we did:

•      Created a rule that blocks emails from outside the organization
•      Set the action to reject the message and include an explanation
•      Tested the rule — external emails were blocked successfully

<img src="assets/mail rule setup.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">

## 8. Message Trace

Message Trace shows the path of an email and why it was delivered, delayed, or blocked. This is one of the most useful tools for Service Desk when a user says "I didn’t receive the email."

We ran a Mail flow > Message trace and confirmed the blocked email appeared in results with 'Failed' status.


<img src="assets/message trace result.png" style="border: 2px solid green; padding: 10px;border-radius:20px;">

## 9. Connecting to Exchange Online with PowerShell

We tried to use PowerShell to manage Exchange Online.
### First attempt – Azure Cloud Shell

Opened Cloud Shell from the Microsoft 365 or Azure portal.

Ran: 
`PowerShellConnect-ExchangeOnline -Device`

Cloud Shell gave a link and a code (this is normal for device authentication).

After signing in, "you don't have access to this" appeared.

Even after assigning the Exchange Administrator role in Entra (entra.microsoft.com → Roles and administrators), Cloud Shell continued to fail.

### Working solution – Local Windows PowerShell

Because Cloud Shell was unreliable, we switched to local PowerShell on a Windows machine:

Install the module:
`PowerShellInstall-Module -Name ExchangeOnlineManagement -Scope CurrentUser -Force`

Connect:
`PowerShellConnect-ExchangeOnline -UserPrincipalName youradmin@yourdomain.onmicrosoft.com`

This method worked reliably.

### Important notes

Some PowerShell commands only work with on-premises Exchange Server. Some only work with Exchange Online. A few work in both environments. Always check the official documentation to confirm which commands apply to Exchange Online:

https://learn.microsoft.com/en-us/powershell/module/exchangepowershell/

### Takeaway for Service Desk

Most day-to-day Tier 1 work is done in the GUI (Microsoft 365 Admin Center and Exchange Admin Center). PowerShell is useful for bulk tasks or automation, but it is not required for basic support.

## 10. Key Differences to Remember

•         **User mailbox:** Belongs to one person and needs a license.
•         **Shared mailbox:** Used by several people. No separate license required (under normal limits).
•         **Distribution list:** Only used for sending emails to a group of people.

## 11. Common Service Desk Tasks

These are the main Exchange-related tasks a Tier 1 analyst usually handles:

•                Create or confirm a user account
•                Assign the correct license
•                Make sure the mailbox works
•                Add the user to the right groups or distribution lists
•               Grant access to shared mailboxes when needed
•                Help with basic email delivery problems using Message Trace
•                Support offboarding (disable account, convert mailbox, remove licenses, etc.)

## 12. Summary

We practiced the core Exchange Online skills needed for a Service Desk Analyst role using the Microsoft 365 and Exchange Admin Center. The focus was on real day-to-day tasks rather than advanced administration.
