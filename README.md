# **README: Intune-Based Data Restriction Solution**

## **📌 Objective**
This solution prevents users from sending company data outside the organization when working from home while allowing data transfer when connected to the office network. The approach is cost-effective, leveraging **Microsoft 365 Business Premium** and existing **Sophos Firewall** to achieve the required security.

---

## **🔷 Solution Overview**
| **Requirement** | **Solution** |
|----------------|-------------|
| Block access to Microsoft 365 services from home | Conditional Access Policy (Azure AD) |
| Prevent USB file transfers | Intune Device Restriction Policy |
| Block external cloud storage and file uploads | Microsoft Defender Web Content Filtering |
| Ensure users cannot bypass security restrictions | Sophos Firewall Home Network Blocking |

---

## **🔷 Step 1: Restrict Microsoft 365 Services Outside the Office**
### **🔹 Configure Conditional Access Policy**
1. **Go to** Microsoft Entra ID (Azure AD) → Security → Conditional Access.
2. **Create a new policy**:
   - **Users**: Apply to all company-owned devices.
   - **Apps**: Select Microsoft 365 apps (Exchange, OneDrive, SharePoint, Teams).
   - **Conditions**:
     - Configure IP locations → **Allow access only from corporate IP range**.
     - Exclude trusted office locations.
   - **Access Control**: Block access if outside the office.
3. **Enable session monitoring** to detect abnormal login patterns.
4. **Save & Deploy.**

📄 **Reference:** [Microsoft Docs - Conditional Access](https://learn.microsoft.com/en-us/azure/active-directory/conditional-access/)

✅ **Effect:** Blocks Microsoft 365 access outside the office while allowing normal use in-office.

---

## **🔷 Step 2: Block USB File Transfers Using Intune**
### **🔹 Configure Intune Device Restriction Policy**
1. **Go to** Microsoft Intune Admin Center → Devices → Configuration Profiles.
2. **Create a new policy**:
   - Platform: Windows 10/11 → Device Restrictions.
   - **Settings**:
     - **Storage**: Block **USB Storage Access** ✅
     - **Data Protection**: Block **Copy-Paste to Unmanaged Apps** ✅
     - **Network Protection**: Disable access to non-corporate networks ✅
3. **Assign to all company-owned devices**.
4. **Save & Deploy.**

📄 **Reference:** [Microsoft Docs - Device Restriction Policies](https://learn.microsoft.com/en-us/mem/intune/configuration/device-restrictions-windows-10)

✅ **Effect:** Prevents users from copying files to USB storage devices or transferring data outside the company network.

---

## **🔷 Step 3: Block External Cloud Storage & File Uploads**
### **🔹 Configure Microsoft Defender for Business**
1. **Go to** Microsoft Defender Admin Center → Web Content Filtering.
2. **Create a new policy**:
   - **Select Categories to Block**:
     - Cloud Storage: Google Drive, Dropbox, WeTransfer, Box, Mega.nz.
     - Personal Email: Gmail, Yahoo Mail, Outlook.com.
     - Social Media & File Sharing Sites.
3. **Assign the policy to all company-owned devices**.
4. **Enable logging for policy violations**.
5. **Save & Deploy.**

📄 **Reference:** [Microsoft Docs - Web Content Filtering](https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/web-content-filtering)

✅ **Effect:** Prevents users from uploading files to unauthorized cloud storage services.

---

## **🔷 Step 4: Restrict Home Network File Transfers Using Sophos Firewall**
### **🔹 Configure Firewall Web Filtering Rules**
1. **Log in to Sophos Firewall Admin Console**.
2. **Create a new Web Filter Rule**:
   - **Inside Office**:
     - Allow all corporate traffic ✅
   - **Outside Office (Home/Public Networks)**:
     - Block file transfers 🚫
     - Block access to non-corporate VPNs 🚫
     - Block access to unauthorized remote desktop services 🚫
   - **Enable Deep Packet Inspection (DPI)** to detect file uploads.
3. **Enable logging & alerts for violations**.
4. **Apply changes & monitor policy enforcement.**

📄 **Reference:** [Sophos Docs - Web Filtering](https://doc.sophos.com/)

✅ **Effect:** Enforces additional protection at the network level, ensuring company data is not sent out via personal networks.

---

## **💰 Cost-Effective Licensing Plan**
| **Feature** | **Solution Used** | **Cost (Per User)** |
|------------|----------------|----------------|
| Microsoft 365 Apps Restriction | Entra ID Conditional Access | ✅ Included in M365 Business Premium (₹1,975/user) |
| USB File Transfer Blocking | Intune Device Restriction | ✅ Included in M365 Business Premium |
| Cloud Storage Upload Blocking | Microsoft Defender Web Filtering | ✅ Included in M365 Business Premium |
| Extra Security (Home Network Restriction) | Sophos Firewall Rules | ✅ No extra cost (already in use) |

💡 **Total Cost Per User:** ₹1,975/month (Much cheaper than Microsoft 365 E5 ₹4,555/user!)

---

## **🎯 Key Benefits of This Approach**
✅ **Cost-effective** – Uses Microsoft 365 Business Premium instead of expensive E5 licenses.  
✅ **Enforces data protection policies** – Prevents file transfers outside the corporate network.  
✅ **Enhances security using existing tools** – Utilizes Intune, Conditional Access, and Firewall controls.  
✅ **No impact on usability** – Employees can work normally when in the office.  
✅ **Ensures compliance with industry security standards** – Meets ISO 27001 & GDPR compliance needs.  

---

## **📌 Conclusion**
This solution ensures that company data **cannot be shared outside the office** while maintaining usability inside the network. By leveraging **Intune, Conditional Access, Microsoft Defender, and Sophos Firewall**, we achieve **enterprise-grade security at a lower cost**.

🚀 **Would you like any additional security controls or modifications?** 😊



---
# README: Restricting Data Transfer Based on Network Location Using Intune

## Overview
This document outlines the approach to block data transfers outside the corporate network using Microsoft Intune while allowing data transfer when connected to the office network. This solution leverages Microsoft Entra ID (Conditional Access), Microsoft Defender for Endpoint, and Sophos Firewall.

## Solution Components & Implementation Steps

### 1. Restricting Access to Microsoft 365 Services Outside the Office
**Method:** Implement Conditional Access policies in Microsoft Entra ID to block access from non-corporate networks.

**Official Documentation:**  
[Microsoft Conditional Access - Block by Location](https://learn.microsoft.com/en-us/entra/identity/conditional-access/policy-block-by-location?utm_source=chatgpt.com)

---

### 2. Blocking USB File Transfers Using Intune
**Method:** Utilize Intune's Administrative Templates to restrict the use of removable storage devices.

**Official Documentation:**  
[Restrict USB Devices via Intune](https://learn.microsoft.com/en-us/mem/intune-service/configuration/administrative-templates-restrict-usb?utm_source=chatgpt.com)

---

### 3. Blocking External Cloud Storage & File Transfers Using Microsoft Defender
**Method:** Configure web content filtering in Microsoft Defender for Endpoint to block access to unauthorized cloud storage services.

**Official Documentation:**  
[Microsoft Defender for Endpoint - Device Control](https://learn.microsoft.com/en-us/defender-endpoint/device-control-overview?utm_source=chatgpt.com)

---

### 4. Enhancing Security with Sophos Firewall
**Method:** Implement firewall rules to prevent data exfiltration when devices are connected to non-corporate networks.

**Official Documentation:**  
Refer to Sophos' official documentation or support for detailed firewall configuration guidance.

---

## Summary
By following these steps and leveraging the capabilities of Microsoft 365 and Intune, organizations can effectively prevent data exfiltration outside the corporate network while allowing secure data transfers within the office environment.


