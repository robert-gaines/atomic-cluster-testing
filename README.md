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

| Category                      | Technique                         | Technique ID  | Sub-technique                         | Sub-technique ID | Outcome  | Additional Information |
| :------:                      | :--------:                        | :-----------: | :-----------:                         | :--------------: | :-----:  | :--------------------: |
| Initial Access                | Exploit Public Facing Application | T1190         | None                                  | None             |    ✘     | No Applicable Tests    |
| Initial Access                | External Remote Services          | T1133         | None                                  | None             |    ✘     | No Applicable Tests    |
| Initial Access                | Valid Accounts                    | T1078         | Default Accounts                      | T1078.001        |    ✘     | No Applicable Tests    |
| Initial Access                | Valid Accounts                    | T1078         | Domain Accounts                       | T1078.002        |    ✘     | No Applicable Tests    |
| Initial Access                | Valid Accounts                    | T1078         | Local Accounts                        | T1078.003        |    ✔     |                        |
| Initial Access                | Valid Accounts                    | T1078         | Cloud Accounts                        | T1078.004        |    ✔     |                        |
| Execution                     | Container Administration Command  | T1609         | None                                  | None             |    ✔     | Control Node Oriented  |
| Execution                     | Deploy Container                  | T1610         | None                                  | None             |    ✔     | Control Node Oriented  |
| Execution                     | Scheduled Task/Job                | T1053         | At                                    | T1053.002        |    ✘     | At Daemon Inactive     |
| Execution                     | Scheduled Task/Job                | T1053         | Cron                                  | T1053.003        |    ✔     | Great success          |
| Execution                     | Scheduled Task/Job                | T1053         | Scheduled Task                        | T1053.005        |    ✘     | No Applicable Tests    |
| Execution                     | Scheduled Task/Job                | T1053         | Systemd Timers                        | T1053.006        |    ✔     |                        |
| Execution                     | Scheduled Task/Job                | T1053         | Container Orchestration Job           | T1053.007        |    ✔     | Control Node Oriented  |
| Execution                     | User Execution                    | T1204         | Malicious Link                        | T1204.001        |    ✘     | No Applicable Tests    |
| Execution                     | User Execution                    | T1204         | Malicious File                        | T1204.002        |    ✘     | No Applicable Tests    |
| Execution                     | User Execution                    | T1204         | Container Orchestration Job           | T1204.003        |    ✘     | No Applicable Tests    |
| Execution                     | User Execution                    | T1204         | Container Orchestration Job           | T1204.004        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation              | T1098         | Additional Cloud Credentials          | T1098.001        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation              | T1098         | Additional Email Delegate Permissions | T1098.002        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation              | T1098         | Additional Cloud Roles                | T1098.003        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation              | T1098         | SSH Authorized Keys                   | T1098.004        |    ✔     |                        |
| Persistence                   | Account Manipulation              | T1098         | Device Registration                   | T1098.005        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation              | T1098         | Additional Container Cluster Roles    | T1098.006        |    ✘     | No Applicable Tests    |
| Persistence                   | Account Manipulation              | T1098         | Additional Local or Domain Groups     | T1098.007        |    ✘     | No Applicable Tests    |






