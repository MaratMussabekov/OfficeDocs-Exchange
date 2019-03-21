---
localization_priority: Normal
ms.author: chrisda
manager: serdars
ms.topic: article
author: chrisda
ms.service: exchange-online
ms.assetid: ca7b7416-92ed-40ad-abdb-695be46ea2e4
ms.collection: 
- exchange-online
- M365-email-calendar
description: Admins can learn how to configure Directory-Based Edge Blocking (DBDB) to reject messages sent to invalid recipients in Exchange Online and Exchange Online Protection during a migration.
ms.audience: ITPro
title: Use Directory Based Edge Blocking to reject messages sent to invalid recipients

---

# Use Directory Based Edge Blocking to reject messages sent to invalid recipients

Directory Based Edge Blocking (DBEB) in Exchange Online and Exchange Online Protection (EOP) lets you reject messages for invalid recipients at the service network perimeter. DBEB lets admins add mail-enabled recipients to Office 365 and block all messages sent to email addresses that aren't present in Office 365.

If a message is sent to a valid email address in Office 365, the message continues through the rest of the service filtering layers: antimalware, antispam, and mail flow rules (also known as transport rules). If the address isn't, the service blocks the message before filtering even occurs, and a non-delivery report (also known as an NDR or _bounce message_) is returned to the sender. The NDR looks like this: `550 5.4.1 [<InvalidAlias>@\<Domain>]: Recipient address rejected: Access denied`.

**If all recipients for your domain are in Exchange Online, DBEB is already in effect, and you don't need to do anything**. If you're migrating from another email system to Exchange Online, you can use the procedure in this topic to enable DBEB for the domain before the migration.

> [!NOTE]
> In hybrid environments, in order for DBEB to work, email for the domain must be routed to Office 365 first (the MX record for the domain must point to Office 365).

## What do you need to know before you begin?

- Estimated time to complete: 5 to 10 minutes

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../exchange-admin-center.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Configure DBEB

1. Verify that your accepted domain in Exchange Online is to **Internal relay**:
  a. In the EAC, go to **Mail flow** \> **Accepted domains**.
  2. Select the domain and click **Edit**.
  3. Ensure that the domain type is set to **Internal relay**. If it's set to **Authoritative**, change it to **Internal relay** and click **Save**.

2. Add users to Office 365. For example:
  - **Directory synchronization**: Add valid users to Office 365 by synchronizing from your on-premises Active Directory environment to [Azure Active Directory](https://technet.microsoft.com/library/hh967611.aspx) in the cloud. For more information about how to set up directory synchronization, see "Use directory synchronization to manage recipients" in [Manage Mail Users in EOP](https://technet.microsoft.com/library/4bfaf2ab-e633-4227-8bde-effefb41a3db.aspx).
  - **Add users via PowerShell or the EAC**: For more information about how to do this, see [Manage Mail Users in EOP](https://technet.microsoft.com/library/4bfaf2ab-e633-4227-8bde-effefb41a3db.aspx) or [Manage mail users in Exchange Online](../recipients-in-exchange-online/manage-mail-users.md).
3. Set your accepted domain in Exchange Online to **Authoritative**:
  a. In the EAC, go to **Mail flow** \> **Accepted domains**.
  b. Select the domain and click **Edit**.
  c. Set the domain type to **Authoritative**.
4. Choose **Save** to save your changes, and confirm that you want to enable DBEB.

**Notes**:

- Until all of your valid recipients have been added to Exchange Online and replicated through the system, you should leave the accepted domain configured as **Internal relay**. Once the domain type has been changed to **Authoritative**, DBEB is designed to allow any SMTP address that has been added to the service (except for mail-enabled public folders). There might be infrequent instances where recipient addresses that do not exist in your Office 365 organization are allowed to relay through the service.

- For more information about DBEB and mail-enabled public folders, see [Office 365 Directory Based Edge Blocking support for on-premises Mail Enabled Public Folders](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders/).

