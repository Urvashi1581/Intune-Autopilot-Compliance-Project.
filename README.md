# 🚀 Windows Autopilot + Intune Compliance Project

This project demonstrates how to **automatically enroll Windows 11 devices into Intune** using **Windows Autopilot**, and how to enforce security with **Compliance Policies + Conditional Access**. It mirrors a real-world setup used by IT teams to provision laptops and protect M365 data.

---

## 📌 Goals
- Autopilot enrollment for Windows 11 VM (VMware Workstation)
- Intune **Compliance Policies**: Defender, Firewall, PIN/Password, (optional) BitLocker
- **Conditional Access**: allow M365 access only if device is **compliant**
- Test: make device non-compliant → blocked → remediate → allowed

---



## 🏗️ High-Level Flow

```mermaid

flowchart TD

  VM["Windows 11 VM"] -->|OOBE enroll| Intune["Intune (MDM)"]
  Intune --> Policy["Compliance Policies"]
  Policy -->|Compliant| M365["Access to Outlook / Teams / SharePoint"]
  Policy -->|Non-Compliant| Blocked["Blocked by Conditional Access"]
  Blocked --> Guide["User fixes settings"]
  Guide --> CompliantAgain["Compliant → Access restored"]

