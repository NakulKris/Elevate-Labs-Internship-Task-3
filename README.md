# README.md  
## Vulnerability Scan – Task 3  
This repository documents the full process and results of performing a **basic vulnerability scan** on my personal machine using **Nessus Essentials**, as required in Task 3. The objective was to identify vulnerabilities, analyse their impact, prioritise them, and implement appropriate remediation steps.

---

## 1. Scanner Used  
**Tool:** Nessus Essentials  
**Scan Type:** Basic Network Scan  
**Target:** `192.168.1.7` (local machine)  
**Scan Mode:** Authenticated via SSH (valid credentials provided)  
**Report Files:**  
- PDF report  

---

## 2. Summary of Results  
From the Nessus scan, the system returned **72 total findings**, broken down as follows:

| Severity | Count |
|---------|-------|
| Critical | 0 |
| High | 1 |
| Medium | 2 |
| Low | 0 |
| Informational | 69 |

The findings are typical for a Linux host running various development libraries and utilities.

Source: Nessus Essentials scan results. 0

---

## 3. Key Vulnerabilities Identified (Prioritised)

Below are the **three actionable vulnerabilities** (High + Medium) identified on the system, along with their descriptions and remediation steps.

---

### **1. Python Library Brotli ≤ 1.1.0 DoS Vulnerability**  
- **Severity:** High  
- **Plugin ID:** 274433
- **Description:** An outdated Brotli compression library could allow remote attackers to trigger a denial of service through specially crafted input.  
- **Impact:** Potential service interruption or abnormal resource consumption when the library processes malicious data.  
- **Remediation:**  
  ```
  sudo apt update
  sudo apt --only-upgrade install python3-brotli
  sudo apt upgrade
  ```
  If no secure version is available and the library is not required:  
  ```
  sudo apt remove python3-brotli
  ```

---

### **2. SSL Certificate Cannot Be Trusted**  
- **Severity:** Medium  
- **Plugin ID:** 51192
- **Description:** A service running on the host uses a self-signed or untrusted certificate. Nessus correctly flags this even for local or testing environments.  
- **Impact:** Susceptible to man-in-the-middle attacks if exposed beyond localhost.  
- **Remediation Options:**  

  **If Apache/Nginx is not intentionally used:**  
  ```
  sudo systemctl stop apache2
  sudo systemctl disable apache2
  ```

  **If the service is intentional:**  
  Regenerate a certificate:  
  ```
  sudo openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes
  ```

---

### **3. Ruby REXML < 3.4.2 DoS Vulnerability**  
- **Severity:** Medium  
- **Plugin ID:** 265895
- **Description:** An outdated Ruby XML parser library contains a vulnerability that can cause denial of service via malicious XML.  
- **Impact:** Applications relying on the library may crash or become unresponsive.  
- **Remediation:**  
  ```
  sudo apt update
  sudo apt upgrade ruby-rexml
  ```  
  If the repository does not provide a patched version:  
  ```
  sudo gem update rexml
  ```

---

## 4. Methodology  
1. Installed and activated Nessus Essentials.  
2. Created a **Basic Network Scan** targeting the local host.  
3. Configured **SSH authenticated scanning** to enable accurate detection of packages and system-level vulnerabilities.  
4. Ran the full scan and monitored progress.  
5. Exported results as PDF.  
6. Reviewed findings, isolated the high- and medium-risk items, and implemented remediation steps directly on the system.  


---


## 6. Conclusion  
The scan detected no critical vulnerabilities, but it did identify an outdated Brotli library and two medium-severity issues involving SSL configuration and Ruby REXML. These issues were addressed through system updates or configuration adjustments. Informational findings represented standard system software and service enumeration commonly seen on Linux hosts and did not require remediation. The system is now in a more secure and updated state following the applied fixes.
