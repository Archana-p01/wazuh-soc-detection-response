# SSH Brute Force Detection and Blocking

---

## Objective

Detect and respond to SSH brute force attacks targeting a Linux server by monitoring repeated failed authentication attempts using Wazuh SIEM. This use case demonstrates how SOC teams identify credential access attempts and prevent unauthorized access.

## Detection

Wazuh identifies SSH brute-force attempts by correlating multiple failed login events.

- Default rule IDs: **5763**
- Trigger condition: Multiple failed authentication attempts from the same source IP within a short time frame
- Rule source: `sshd_rules.xml` (enabled by default)
![SSH Detection Rule](screenshots/ssh/ssh-rule.png)
---

## Blocking with Active Response

When the brute force rule is triggered, Wazuh can automatically block the attacker's IP address using an active response script.

- Script used: `firewall-drop.sh`
- Mechanism: Uses `iptables` to drop all traffic from the attacker IP

---

## Step 1: Verify Command Configuration

Ensure the `firewall-drop` command is defined in:

`/var/ossec/etc/ossec.conf`

```xml
<command>
  <name>firewall-drop</name>
  <executable>firewall-drop.sh</executable>
  <timeout_allowed>yes</timeout_allowed>
</command>
```
## Step 2: Configure Active Response

Add the following configuration to:

`/var/ossec/etc/ossec.conf`

```xml
<active-response>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5763</rules_id>
  <timeout>180</timeout>
</active-response>
```
## Explanation

location: local → Executes the command on the monitored endpoint

timeout: 180 → Blocks the attacker IP for 180 seconds (3 minutes)

## Step 3: Restart Wazuh Manager

Apply the changes by restarting the manager:
```
sudo systemctl restart wazuh-manager
```
## Verification

## Step 4: Simulate Attack
Perform SSH brute force from Kali Linux:
```
hydra -t 4 -l <username> -P passwords.txt ssh://<target_ip>
```
