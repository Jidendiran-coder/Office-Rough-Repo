Got it! Since we are dealing with **all types of data** and you have control over the **VPN, Sophos Firewall, and Microsoft 365 apps**, here’s how we can design the solution:  

### 🔑 **Solution Breakdown**  

#### **Step 1: Identify Office Network (Trusted Locations) in Azure AD**  
We will define the office network as a **trusted location** in Azure AD, so Conditional Access policies can differentiate between home and office.  

✅ **Action:**  
1. Go to **Azure AD > Security > Conditional Access**  
2. Click **Named locations > New location**  
3. Name it "**Corporate Network**"  
4. Choose **IP ranges** and enter your office **public IP** (You can get this from your firewall).  
5. Click **Create**  

⏭ **Reasoning:** This ensures that any Conditional Access policy can detect whether the user is in the office or not.  

---

#### **Step 2: Enforce VPN for Office Access (Always On VPN or Conditional Access)**  
We need a way to detect if users are outside the office. Since you have a **Sophos firewall**, we have two options:  
1️⃣ **Enforce VPN** (so they can only access company resources via VPN)  
2️⃣ **Block access to corporate data if not on the office network**  

✅ **Option 1: Enforce VPN (Preferred Approach)**  
- Configure **Always On VPN** on company devices  
- Enforce **Intune Compliance Policy** to check VPN connectivity  
- If VPN is disconnected, access is blocked  

✅ **Option 2: Conditional Access Policy to Block External Networks**  
- Go to **Azure AD > Security > Conditional Access**  
- Click **New Policy**  
- Name it "**Block External Data Transfer**"  
- Assign it to **All users** (or a specific group)  
- Under **Cloud apps**, select:  
  - Exchange Online (Email)  
  - SharePoint Online (File sharing)  
  - OneDrive  
  - Teams  
- Under **Conditions**, choose **Locations > Exclude > Corporate Network (from Step 1)**  
- Under **Grant**, choose **Block Access**  
- Click **Create**  

⏭ **Reasoning:** This ensures that data transfer via email, OneDrive, or SharePoint is **blocked unless they are on the office network or VPN**.  

---

#### **Step 3: Prevent File Uploads & External Sharing (DLP Policy)**  
Even if users can access company resources, we need to block:  
- Uploading files to **Google Drive, Dropbox, WeTransfer**  
- Copying files to **USB drives**  
- Copying content from corporate apps to **personal email or chat apps**  

✅ **Action (Microsoft Purview DLP Policy):**  
1. Go to **Microsoft Purview Compliance Portal**  
2. Click **Data Loss Prevention > Policies > Create Policy**  
3. Choose **Microsoft 365 services** (OneDrive, SharePoint, Teams, Exchange)  
4. Under **Policy Scope**, choose **Devices**  
5. Add the rule:  
   - Block file transfer to external domains  
   - Block file copy to USB  
   - Block **copy-paste** from corporate apps to personal apps  
6. Under **Exceptions**, exclude **Corporate Network (trusted location)**  
7. Click **Save and Deploy**  

⏭ **Reasoning:** This ensures that files cannot be uploaded, copied, or transferred **outside** of the office network.  

---

#### **Step 4: Restrict USB & Local Transfers (Intune Policy)**  
Users might try copying files to USB or an external HDD. We can block this via **Intune Device Configuration Profile**.  

✅ **Action:**  
1. Go to **Intune > Devices > Configuration Profiles**  
2. Click **Create Profile** > **Windows 10/11**  
3. Select **Endpoint Protection** > **Removable Storage Access**  
4. Set **Deny Write Access to Removable Storage** → **Enabled**  
5. Click **Create**  

⏭ **Reasoning:** This prevents employees from copying data to external devices when working remotely.  

---

#### **Step 5: Monitor & Alert Suspicious Activity**  
Some users might try **bypassing VPN, using personal hotspots, or remote desktop connections**. We need **Defender for Endpoint & SIEM alerts** for this.  

✅ **Action:**  
1. Go to **Defender for Endpoint > Policies > Create a new rule**  
2. Set **"Alert when accessing corporate data from an unknown IP"**  
3. Configure **SIEM integration** to monitor VPN logs  

⏭ **Reasoning:** This ensures that we get alerts when users try to **bypass security rules**.  

---

### **Final Check & Testing** ✅  
🔘 Test access with an employee in the **office network** → Data transfer should be allowed  
🔘 Test access with an employee **without VPN from home** → Should be blocked  
🔘 Test uploading files to **Google Drive, Dropbox** → Should be blocked  
🔘 Test copying files to **USB drive** → Should be blocked  

---

### **Conclusion**  
By combining **Conditional Access, VPN enforcement, Intune policies, and DLP rules**, we successfully **block external data transfers** when employees work from home but **allow data sharing inside the office**.  

This setup ensures that even if users find a way to **access files remotely, they cannot transfer them out**. 🎯  

---
