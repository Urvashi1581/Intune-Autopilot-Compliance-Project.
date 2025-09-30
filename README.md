# Windows Autopilot + Intune Compliance Project

This project demonstrates how to **automatically enroll Windows 11 devices into Intune** using **Windows Autopilot**, and how to enforce security with **Compliance Policies + Conditional Access**. It mirrors a real-world setup used by IT teams to provision laptops and protect M365 data.

---

## Goals
- Autopilot enrollment for Windows 11 VM (VMware Workstation)
- Intune **Compliance Policies**: Defender, Firewall, PIN/Password, BitLocker
- **Conditional Access**: allow M365 access only if device is **compliant**
- Test: make device non-compliant → blocked → remediate → allowed

---

## Steps Performed

### 1. Export Windows Autopilot Device Hash
- Used PowerShell script to generate hardware hash.
- Exported as `DeviceHash.csv`.
- Uploaded hash into **Intune Autopilot Devices**.



---

### 2. Create Autopilot Deployment Profile
- Configured **Deployment profile** in Intune.
- Assigned the profile to uploaded device(s).
- Verified that the profile applied correctly.



---

### 3. Configure Compliance Policies
- Created compliance rules (e.g., Require BitLocker, Antivirus on, Minimum OS version).
- Assigned compliance policy to Autopilot group.
- Device evaluated as **Compliant** or **Non-Compliant**.



---

### 4. Test Conditional Access
- Set Conditional Access rule in Entra ID:  
  → *Only compliant devices can access M365 apps (Outlook, Teams, SharePoint).*
- Non-compliant device got blocked.
- Guided remediation steps fixed issues and restored access.



---

### 5. Final Device Status in Intune
- Verified device enrollment under **Intune → Devices**.
- Checked Compliance status (green = compliant, red = non-compliant).
- Confirmed successful Autopilot + Intune MDM flow.





## 🏗High-Level Flow

```mermaid

flowchart TD

  VM["Windows 11 VM"] -->|OOBE enroll| Intune["Intune (MDM)"]
  Intune --> Policy["Compliance Policies"]
  Policy -->|Compliant| M365["Access to Outlook / Teams / SharePoint"]
  Policy -->|Non-Compliant| Blocked["Blocked by Conditional Access"]
  Blocked --> Guide["User fixes settings"]
  Guide --> CompliantAgain["Compliant → Access restored"]

