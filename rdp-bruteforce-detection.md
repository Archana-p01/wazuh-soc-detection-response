## UC1 – RDP Brute Force Detection and Blocking (Windows Endpoint)

### Objective
Detect and respond to RDP brute force attacks targeting a Windows system by monitoring repeated failed login attempts using Wazuh SIEM. This use case demonstrates how SOC teams identify credential access attempts and prevent unauthorized access.

---

### Environment
- **Wazuh Manager:** Ubuntu Server  
- **Target Machine:** Windows 10 (Wazuh Agent)  
- **Attacker Machine:** Kali Linux  
- **Detection Type:** Host-based (Windows Security Logs)  
- **Target Service:** RDP (Port 3389)  

---

### SOC Context
RDP brute force attacks are commonly used by attackers to gain initial access to Windows systems. Detecting repeated failed login attempts helps SOC teams identify credential-based attacks early and prevent unauthorized access.

---

## Step 1: Log Monitoring Configuration

- Wazuh agent installed on Windows  
- Security logs collected using Event Channel  

```xml
<localfile>
  <location>Security</location>
  <log_format>eventchannel</log_format>
</localfile>
