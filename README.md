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

Testing related to this repository has been conducted on a k3s deployment. The following table tracks the success or failure of each test relative to the Container Matrix associated with MITRE ATT&CK.

| Category                      | Technique                             | Technique ID  | Sub-technique                                        | Sub-technique ID | Outcome  | Additional Information |
| :------:                      | :--------:                            | :-----------: | :-----------:                                        | :--------------: | :-----:  | :--------------------: |
| Initial Access                | Exploit Public Facing Application     | T1190         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Initial Access                | External Remote Services              | T1133         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Initial Access                | Valid Accounts                        | T1078         | Default Accounts                                     | T1078.001        |    ✘     | No Applicable Tests    |
| Initial Access                | Valid Accounts                        | T1078         | Domain Accounts                                      | T1078.002        |    ✘     | No Applicable Tests    |
| Initial Access                | Valid Accounts                        | T1078         | Local Accounts                                       | T1078.003        |    ✔     |                        |
| Initial Access                | Valid Accounts                        | T1078         | Cloud Accounts                                       | T1078.004        |    ✔     |                        |
| Execution                     | Container Administration Command      | T1609         | None                                                 | None             |    ✔     | Control Node Oriented  |
| Execution                     | Deploy Container                      | T1610         | None                                                 | None             |    ✔     | Control Node Oriented  |
| Execution                     | Scheduled Task/Job                    | T1053         | At                                                   | T1053.002        |    ✘     | At Daemon Inactive     |
| Execution                     | Scheduled Task/Job                    | T1053         | Cron                                                 | T1053.003        |    ✔     | Great success          |
| Execution                     | Scheduled Task/Job                    | T1053         | Scheduled Task                                       | T1053.005        |    ✘     | No Applicable Tests    |
| Execution                     | Scheduled Task/Job                    | T1053         | Systemd Timers                                       | T1053.006        |    ✔     |                        |
| Execution                     | Scheduled Task/Job                    | T1053         | Container Orchestration Job                          | T1053.007        |    ✔     | Control Node Oriented  |
| Execution                     | User Execution                        | T1204         | Malicious Link                                       | T1204.001        |    ✘     | No Applicable Tests    |
| Execution                     | User Execution                        | T1204         | Malicious File                                       | T1204.002        |    ✘     | No Applicable Tests    |
| Execution                     | User Execution                        | T1204         | Container Orchestration Job                          | T1204.003        |    ✘     | No Applicable Tests    |
| Execution                     | User Execution                        | T1204         | Container Orchestration Job                          | T1204.004        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                  | T1098         | Additional Cloud Credentials                         | T1098.001        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                  | T1098         | Additional Email Delegate Permissions                | T1098.002        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                  | T1098         | Additional Cloud Roles                               | T1098.003        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                  | T1098         | SSH Authorized Keys                                  | T1098.004        |    ✔     |                        |
| Persistence                   | Account Manipulation                  | T1098         | Device Registration                                  | T1098.005        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                  | T1098         | Additional Container Cluster Roles                   | T1098.006        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation                  | T1098         | Additional Local or Domain Groups                    | T1098.007        |    ✘     | No Applicable Tests    |
| Persistence                   | Create Account                        | T1136         | Local Account                                        | T1136.001        |    ✔     |                        |
| Persistence                   | Create Account                        | T1136         | Domain Account                                       | T1136.002        |    ✘     | AD Oriented            |
| Persistence                   | Create Account                        | T1136         | Cloud Account                                        | T1136.003        |    ✘     | AWS/AAD Oriented       |
| Persistence                   | Create or Modify System Process       | T1543         | Launch Agent                                         | T1543.001        |    ✘     |                        |
| Persistence                   | Create or Modify System Process       | T1543         | Systemd Service                                      | T1543.002        |    ✔     |                        |
| Persistence                   | Create or Modify System Process       | T1543         | Windows Service                                      | T1543.003        |    ✘     | No Applicable Tests    |
| Persistence                   | Create or Modify System Process       | T1543         | Launch Daemon                                        | T1543.004        |    ✘     | No Applicable Tests    |
| Persistence                   | Create or Modify System Process       | T1543         | Container Service                                    | T1543.005        |    ✘     | No Applicable Tests    |
| Persistence                   | External Remote Services              | T1133         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Persistence                   | Implant Internal Image                | T1525         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Privilege Escalation          | Escape to Host                        | T1611         | None                                                 | None             |    ✔     | Partial Execution      |
| Privilege Escalation          | Exploitation for Privilege Escalation | T1068         | None                                                 | None             |    ✘     | No Applicable Tests    |
| Defense Evasion               | Build Image on Host                   | T1612         | None                                                 | None             |    ✔     | Partial Execution      |
| Defense Evasion               | Impair Defenses                       | T1562         | Disable or Modify Tools                              | T1562.001        |    ✔     | Partial Execution      |
| Defense Evasion               | Impair Defenses                       | T1562         | Disable Windows Event Logging                        | T1562.002        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                       | T1562         | Impair Command History Logging                       | T1562.003        |    ✔     |                        |
| Defense Evasion               | Impair Defenses                       | T1562         | Disable or Modify System Firewall                    | T1562.004        |    ✔     |                        |
| Defense Evasion               | Impair Defenses                       | T1562         | Indicator Blocking                                   | T1562.006        |    ✔     |                        |
| Defense Evasion               | Impair Defenses                       | T1562         | Disable or Modify Cloud Firewall                     | T1562.007        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                       | T1562         | Disable or Modify Cloud Logs                         | T1562.008        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                       | T1562         | Safe Mode Boot                                       | T1562.009        |    ✘     | No Applicable Tests    |
| Defense Evasion               | Impair Defenses                       | T1562         | Downgrade Attack                                     | T1562.010        |          |       |
| Defense Evasion               | Impair Defenses                       | T1562         | Spoof Security Alerting                              | T1562.011        |          |       |
| Defense Evasion               | Impair Defenses                       | T1562         | Disable or Modify Linux Audit System                 | T1562.012        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | Clear Windows Event Logs                             | T1070.001        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | Clear Linux or Mac System Logs                       | T1070.002        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | Clear Command History                                | T1070.003        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | File Deletion                                        | T1070.004        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | Network Share Connection Removal                     | T1070.005        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | Timestomp                                            | T1070.006        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | Clear Network Connection History and Configurations  | T1070.007        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | Clear Mailbox Data                                   | T1070.008        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | Clear Persistence                                    | T1070.009        |          |       |
| Defense Evasion               | Indicator Removal                     | T1070         | Relocate Malware                                     | T1070.010        |          |       |







