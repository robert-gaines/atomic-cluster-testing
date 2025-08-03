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

| Category    | Technique   | Sub-technique | Outcome |
| :------:    | :--------:  | :-----------: | :-----: |
| Header      | Title       | Here's this   | X       |
| Paragraph   | Text        | And more      | X       |


