# atomic-cluster-testing
This project repurposes Red Canary's container oriented tests for Atomic Red Team to allow for the conduct of adversarial simulation within cluster deployments operating on ARM64 systems.

For testing on standalone ARM64 systems use the following container image:

```
docker pull robertgaines/atomic-cluster-testing:arm64
```

Testing with the deployment will require manual intervention, per my understanding. 

Consider using the following syntax:

```
kubectl exec -it atomicred-pod -- pwsh /root/RunTests.ps1 T1053.002
```

A Jenkinsfile is also included, as manual execution is somewhat time consuming. Edit the AtomicRed-Job.yaml file's command section to specify the desired tests.

```
command: ["pwsh", "/root/RunTests.ps1", "comma", "separated", "list", "of", "technique IDs" ]
```

Testing related to this repository has been conducted on a k3s deployment. The following table tracks the success or failure of each test relative to the Container Matrix associated with MITRE ATT&CK.

| Category                      | Technique                              | Technique ID  | Sub-technique                                        | Sub-technique ID | Outcome  | Additional Information |
| :------:                      | :--------:                             | :-----------: | :-----------:                                        | :--------------: | :-----:  | :--------------------: |
| Initial Access                | Exploit Public Facing Application      | T1190         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Initial Access                | External Remote Services               | T1133         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Initial Access                | Valid Accounts                         | T1078         | Default Accounts                                     | T1078.001        |    ✘     | No Applicable Tests    |
| Initial Access                | Valid Accounts                         | T1078         | Domain Accounts                                      | T1078.002        |    ✘     | No Applicable Tests    |
| Initial Access                | Valid Accounts                         | T1078         | Local Accounts                                       | T1078.003        |    ✔     |                        |
| Initial Access                | Valid Accounts                         | T1078         | Cloud Accounts                                       | T1078.004        |    ✔     |                        |
| Execution                     | Container Administration Command       | T1609         | None                                                 | None             |    ✔     | Control Node Oriented  |
| Execution                     | Deploy Container                       | T1610         | None                                                 | None             |    ✔     | Control Node Oriented  |
| Execution                     | Scheduled Task/Job                     | T1053         | At                                                   | T1053.002        |    ✘     | At Daemon Inactive     |
| Execution                     | Scheduled Task/Job                     | T1053         | Cron                                                 | T1053.003        |    ✔     | Great success          |
| Execution                     | Scheduled Task/Job                     | T1053         | Scheduled Task                                       | T1053.005        |    ✘     | No Applicable Tests    |
| Execution                     | Scheduled Task/Job                     | T1053         | Systemd Timers                                       | T1053.006        |    ✔     |                        |
| Execution                     | Scheduled Task/Job                     | T1053         | Container Orchestration Job                          | T1053.007        |    ✔     | Control Node Oriented  |
| Execution                     | User Execution                         | T1204         | Malicious Link                                       | T1204.001        |    ✘     | No Applicable Tests    |
| Execution                     | User Execution                         | T1204         | Malicious File                                       | T1204.002        |    ✘     | No Applicable Tests    |
| Execution                     | User Execution                         | T1204         | Container Orchestration Job                          | T1204.003        |    ✘     | No Applicable Tests    |
| Execution                     | User Execution                         | T1204         | Container Orchestration Job                          | T1204.004        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                   | T1098         | Additional Cloud Credentials                         | T1098.001        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                   | T1098         | Additional Email Delegate Permissions                | T1098.002        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                   | T1098         | Additional Cloud Roles                               | T1098.003        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                   | T1098         | SSH Authorized Keys                                  | T1098.004        |    ✔     |                        |
| Persistence                   | Account Manipulation                   | T1098         | Device Registration                                  | T1098.005        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                   | T1098         | Additional Container Cluster Roles                   | T1098.006        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                   | T1098         | Additional Local or Domain Groups                    | T1098.007        |    ✘     | No Applicable Tests    |
| Persistence                   | Create Account                         | T1136         | Local Account                                        | T1136.001        |    ✔     |                        |
| Persistence                   | Create Account                         | T1136         | Domain Account                                       | T1136.002        |    ✘     | AD Oriented            |
| Persistence                   | Create Account                         | T1136         | Cloud Account                                        | T1136.003        |    ✘     | AWS/AAD Oriented       |
| Persistence                   | Create or Modify System Process        | T1543         | Launch Agent                                         | T1543.001        |    ✘     |                        |
| Persistence                   | Create or Modify System Process        | T1543         | Systemd Service                                      | T1543.002        |    ✔     |                        |
| Persistence                   | Create or Modify System Process        | T1543         | Windows Service                                      | T1543.003        |    ✘     | No Applicable Tests    |
| Persistence                   | Create or Modify System Process        | T1543         | Launch Daemon                                        | T1543.004        |    ✘     | No Applicable Tests    |
| Persistence                   | Create or Modify System Process        | T1543         | Container Service                                    | T1543.005        |    ✘     | No Applicable Tests    |
| Persistence                   | External Remote Services               | T1133         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Persistence                   | Implant Internal Image                 | T1525         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Privilege Escalation          | Escape to Host                         | T1611         | None                                                 | None             |    ✔     | Partial Execution      |
| Privilege Escalation          | Exploitation for Privilege Escalation  | T1068         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Defense Evasion               | Build Image on Host                    | T1612         | None                                                 | None             |    ✔     | Partial Execution      |
| Defense Evasion               | Impair Defenses                        | T1562         | Disable or Modify Tools                              | T1562.001        |    ✔     | Partial Execution      |
| Defense Evasion               | Impair Defenses                        | T1562         | Disable Windows Event Logging                        | T1562.002        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                        | T1562         | Impair Command History Logging                       | T1562.003        |    ✔     |                        |
| Defense Evasion               | Impair Defenses                        | T1562         | Disable or Modify System Firewall                    | T1562.004        |    ✔     |                        |
| Defense Evasion               | Impair Defenses                        | T1562         | Indicator Blocking                                   | T1562.006        |    ✔     |                        |
| Defense Evasion               | Impair Defenses                        | T1562         | Disable or Modify Cloud Firewall                     | T1562.007        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                        | T1562         | Disable or Modify Cloud Logs                         | T1562.008        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                        | T1562         | Safe Mode Boot                                       | T1562.009        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                        | T1562         | Downgrade Attack                                     | T1562.010        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                        | T1562         | Spoof Security Alerting                              | T1562.011        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                        | T1562         | Disable or Modify Linux Audit System                 | T1562.012        |    ✔     | No Applicable Tests    |
| Defense Evasion               | Indicator Removal                      | T1070         | Clear Windows Event Logs                             | T1070.001        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Indicator Removal                      | T1070         | Clear Linux or Mac System Logs                       | T1070.002        |    ✔     |                        |
| Defense Evasion               | Indicator Removal                      | T1070         | Clear Command History                                | T1070.003        |    ✔     |                        |
| Defense Evasion               | Indicator Removal                      | T1070         | File Deletion                                        | T1070.004        |    ✔     |                        |
| Defense Evasion               | Indicator Removal                      | T1070         | Network Share Connection Removal                     | T1070.005        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Indicator Removal                      | T1070         | Timestomp                                            | T1070.006        |    ✔     |                        |
| Defense Evasion               | Indicator Removal                      | T1070         | Clear Network Connection History and Configurations  | T1070.007        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Indicator Removal                      | T1070         | Clear Mailbox Data                                   | T1070.008        |    ✔     |                        |
| Defense Evasion               | Indicator Removal                      | T1070         | Clear Persistence                                    | T1070.009        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Indicator Removal                      | T1070         | Relocate Malware                                     | T1070.010        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Masquerading                           | T1036         | Invalid Code Signature                               | T1036.001        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Masquerading                           | T1036         | Right-to-Left Overview                               | T1036.002        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Masquerading                           | T1036         | Rename Legitimate Utilities                          | T1036.003        |    ✔     |                        |
| Defense Evasion               | Masquerading                           | T1036         | Masquerade Task or Service                           | T1036.004        |    ✔     |                        |
| Defense Evasion               | Masquerading                           | T1036         | Match Legitimate Resource Name or Location           | T1036.005        |    ✔     |                        |
| Defense Evasion               | Masquerading                           | T1036         | Space after Filename                                 | T1036.006        |    ✔     |                        |
| Defense Evasion               | Masquerading                           | T1036         | Double File Extension                                | T1036.007        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Masquerading                           | T1036         | Masquerade File Type                                 | T1036.008        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Masquerading                           | T1036         | Break Process Tree                                   | T1036.009        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Masquerading                           | T1036         | Masquerade Account Name                              | T1036.010        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Masquerading                           | T1036         | Overwrite Process Arguments                          | T1036.011        |    ✘     | No Applicable Tests    |
| Defense Evasion               | User Alternate Authentication Material | T1550         | Application Access Token                             | T1550.001        |    ✘     | No Applicable Tests    |
| Defense Evasion               | User Alternate Authentication Material | T1550         | Pass the Hash                                        | T1550.002        |    ✘     | No Applicable Tests    |
| Defense Evasion               | User Alternate Authentication Material | T1550         | Pass the Ticket                                      | T1550.003        |    ✘     | No Applicable Tests    |
| Defense Evasion               | User Alternate Authentication Material | T1550         | Web Session Cookie                                   | T1550.004        |    ✘     | No Applicable Tests    |
| Credential Access             | Brute Force                            | T1110         | Password Guessing                                    | T1110.001        |    ✘     | No Applicable Tests    |
| Credential Access             | Brute Force                            | T1110         | Password Cracking                                    | T1110.002        |    ✘     | No Applicable Tests    |
| Credential Access             | Brute Force                            | T1110         | Password Spraying                                    | T1110.003        |    ✘     | No Applicable Tests    |
| Credential Access             | Brute Force                            | T1110         | Credential Stuffing                                  | T1110.004        |    ✔     |                        |
| Credential Access             | Steal Application Access Token         | T1528         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Credential Access             | Unsecured Credentials                  | T1552         | Credentials in Files                                 | T1552.001        |    ✘     | No Applicable Tests    |
| Credential Access             | Unsecured Credentials                  | T1552         | Credentials in Registry                              | T1552.002        |    ✘     | No Applicable Tests    |
| Credential Access             | Unsecured Credentials                  | T1552         | Bash History                                         | T1552.003        |    ✔     |                        |
| Credential Access             | Unsecured Credentials                  | T1552         | Private Keys                                         | T1552.004        |    ✔     |                        |
| Credential Access             | Unsecured Credentials                  | T1552         | Cloud Instance Metadata API                          | T1552.005        |    ✘     | No Applicable Tests    |
| Credential Access             | Unsecured Credentials                  | T1552         | Group Policy Preferences                             | T1552.006        |    ✘     | No Applicable Tests    |
| Credential Access             | Unsecured Credentials                  | T1552         | Container API                                        | T1552.007        |    ✔     | Partial Execution      |
| Credential Access             | Unsecured Credentials                  | T1552         | Chat Messages                                        | T1552.008        |    ✘     | No Applicable Tests    |
| Discovery                     | Container and Resource Discovery       | T1613         | None                                                 | None             |    ✔     |                        |
| Discovery                     | Network Service Discovery              | T1046         | None                                                 | None             |    ✔     |                        |
| Discovery                     | Permission Groups Discovery            | T1069         | Local Groups                                         | T1069.001        |    ✔     |                        |
| Discovery                     | Permission Groups Discovery            | T1069         | Domain Groups                                        | T1069.002        |    ✔     |                        |
| Discovery                     | Permission Groups Discovery            | T1069         | Cloud Groups                                         | T1069.003        |    ✘     | No Applicable Tests    |
| Impact                        | Data Destruction                       | T1485         | Lifecycle-Triggered Deletion                         | T1485.001        |    ✘     | No Applicable Tests    |
| Impact                        | Endpoint Denial of Service             | T1499         | OS Exhaustion Flood                                  | T1499.001        |    ✘     | No Applicable Tests    |
| Impact                        | Endpoint Denial of Service             | T1499         | Service Exhaustion Flood                             | T1499.002        |    ✘     | No Applicable Tests    |
| Impact                        | Endpoint Denial of Service             | T1499         | Application Exhaustion Flood                         | T1499.003        |    ✘     | No Applicable Tests    |
| Impact                        | Endpoint Denial of Service             | T1499         | Application or System Exploitation                   | T1499.004        |    ✘     | No Applicable Tests    |
| Impact                        | Inhibit System Recovery                | T1490         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Impact                        | Network Denial of Service              | T1498         | Direct Network Flood                                 | T1498.001        |    ✘     | No Applicable Tests    |
| Impact                        | Network Denial of Service              | T1498         | Reflection Amplification                             | T1498.002        |    ✘     | No Applicable Tests    |
| Impact                        | Resource Hijacking                     | T1496         | Compute Hijacking                                    | T1496.001        |    ✘     | No Applicable Tests    |
| Impact                        | Resource Hijacking                     | T1496         | Bandwidth Hijacking                                  | T1496.002        |    ✘     | No Applicable Tests    |
| Impact                        | Resource Hijacking                     | T1496         | SMS Pumping                                          | T1496.003        |    ✘     | No Applicable Tests    |
| Impact                        | Resource Hijacking                     | T1496         | Cloud Service Hijacking                              | T1496.004        |    ✘     | No Applicable Tests    |





