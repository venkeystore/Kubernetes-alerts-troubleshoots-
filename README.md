 Kubernetes-alerts-troubleshoots-



1️⃣ KubeContainerWaiting – Troubleshooting "ImagePullBackOff" & "CrashLoopBackOff" "Node NotReady"

🚨 "ImagePullBackOff" – Causes & Resolution  
"Based on my experience, the most common reasons for 'ImagePullBackOff' include authentication failures, incorrect image names, or network issues. I would troubleshoot this as follows:"  

✅Authentication Issues – Ensure the correct repository credentials are configured and accessible.  
✅Image Name Mismatch – Verify the image name and tag in the YAML file to prevent typos or missing tags.  
✅Network Issues – Check DNS resolution, firewall rules, and internet access for the node to pull the image successfully.  

🚨 "CrashLoopBackOff" – Causes & Resolution  
"A pod enters 'CrashLoopBackOff' when it continuously restarts due to application failures or misconfigurations. To resolve this, I would:"  

🔹Check Pod Logs – Use `kubectl logs <pod-name>` to identify application errors.  
🔹Describe the Pod – Run `kubectl describe pod <pod-name>` to check for readiness/liveness probe failures.  
🔹Inspect Resource Limits – Ensure CPU/Memory limits are adequate to prevent OOM (Out of Memory) kills.  

---

2️⃣ Node Missing from Kubernetes – "NodeMissingFromK8s"  
"When a node is missing or unresponsive, I follow these steps to diagnose and resolve the issue:"  

1️⃣Verify Node Status – Check `kubectl get nodes` to see if the node is in a `NotReady` state.  
2️⃣Check Connectivity – Test network connectivity using `ping` or `telnet`.  
3️⃣Inspect `kubelet` Logs – Run `journalctl -u kubelet -f` to identify errors.  
4️⃣Check System Resource Usage – Ensure sufficient CPU, memory, and disk space.  
5️⃣If Unresponsive – Drain and delete the node from the cluster (`kubectl drain <node>` and `kubectl delete node <node>`), then investigate and fix the issue before rejoining the node.  

---

3️⃣ Node Cordoned – "NodeCordoned"  
"Cordoning a node prevents scheduling new pods on it, which is useful for maintenance, resource constraints, or troubleshooting."  

✅To Cordone a Node: `kubectl cordon <node-name>`  
✅To Uncordon (Allow Scheduling Again): `kubectl uncordon <node-name>`  

💡 Use Case: Prevents unnecessary disruptions due to planned maintenance or node instability while allowing existing workloads to run.  

---

4️⃣ Node Filesystem Almost Out of Space – "NodeFilesystemAlmostOutOfSpace"  
"When a node is running low on disk space, it can cause pod failures and performance issues. To mitigate this:"  

✅Clear Unnecessary Logs & Temporary Files – `du -sh /var/log` and `rm -rf /var/log/.gz`  
✅Increase Disk Size – If using cloud-based infrastructure, resize the disk volume.  
✅Move Persistent Storage – Shift logs and data to an external volume to free up local space.  

---

5️⃣ Kube Job Failed – "KubeJobFailed"  
"A Kubernetes Job fails when it exceeds retry limits or encounters execution errors. I would debug as follows:"  

🔹Check Job Logs – Use `kubectl logs job/<job-name>` to diagnose issues.  
🔹Verify Resource Limits – Ensure the job has enough CPU and memory to avoid termination due to OOM.  
🔹Inspect Backoff Limit – Check if the job is retrying indefinitely (`spec.backoffLimit`).  

---

6️⃣ Deployment Replicas Mismatch – "KubeDeploymentReplicasMismatch"  
"If the number of running pods does not match the desired replicas, I investigate the following:"  

✅Check Node Availability – Ensure there are enough schedulable nodes.  
✅Look for Pending Pods – Use `kubectl get pods --field-selector=status.phase=Pending` to identify scheduling issues.  
✅Verify Resource Constraints – Ensure sufficient CPU, memory, and storage availability.  
✅Storage Volume Issues – Confirm that Persistent Volume Claims (PVCs) are bound and available.  
✅Update Deployment Spec – If required, update and restart the rollout using `kubectl rollout restart deployment <deployment-name>`.  

---

7️⃣ Pending CSR – "Pending Certificate Signing Request"  
"A Pending CSR indicates that a node or user requires certificate approval for authentication. To resolve this:"  

🔹List Pending CSRs – `kubectl get csr`  
🔹Approve the CSR – `kubectl certificate approve <csr-name>`  
🔹Deny a Malicious CSR – `kubectl certificate deny <csr-name>`  


