### Resources
- [Vault | HashiCorp Developer](https://developer.hashicorp.com/vault)
- [HashiCorp Vault | Pentester's Promiscuous Notebook](https://ppn.snovvcra.sh/pentest/infrastructure/devops/hashicorp-vault#get-secret)

---
### Tools & Techniques
- `vault` CLI
- Burp Suite (for API fuzzing)
- Custom scripts using Vault HTTP API
- Secrets scanning tools (e.g., TruffleHog, Gitleaks)
- Kubernetes integration testing (if Vault is used with K8s)
#### Example Attack Scenario
1. **Compromise a pod** with Vault integration.
2. **Extract service account token** from the pod.
3. **Authenticate to Vault** using the token:
```bash
vault write auth/kubernetes/login \  
	role=my-role \  
	jwt=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)  
```
1. **Access secrets**:
```bash
vault kv get secret/myapp/config
```

---
### 1. Reconnaissance
- [ ]  Identify Vault endpoints (default port: `8200`)
- [ ]  Check for HTTP headers like `X-Vault-Token`, `X-Vault-Namespace`
- [ ]  Enumerate exposed API endpoints (`/v1/sys/health`, `/v1/sys/seal-status`)
- [ ]  Determine Vault mode (dev vs production)
- [ ]  Check TLS configuration and certificate validity

---
### 2. Authentication & Access Control
- [ ]  Test for unauthenticated access to Vault API
- [ ]  Enumerate enabled auth methods (`/v1/sys/auth`)
- [ ]  Attempt login via:
    - AppRole
    - LDAP
    - GitHub
    - Token
    - Kubernetes
- [ ]  Check for hardcoded or leaked tokens in:
    - Source code
    - Environment variables
    - CI/CD pipelines
- [ ]  Test token reuse or privilege escalation

---
### 3. Secrets Enumeration & Extraction
- [ ]  List enabled secrets engines (`/v1/sys/mounts`)
- [ ]  Attempt to read secrets from:
    - KV store (`/v1/secret/data/...`)
    - Database credentials
    - AWS/GCP tokens
    - SSH keys
- [ ]  Check for versioned secrets (KV v2)
- [ ]  Abuse overly permissive policies

---
### 4. Vault Configuration & Policy Review
- [ ]  Enumerate policies (`/v1/sys/policies/acl`)
- [ ]  Identify wildcard or overly broad permissions
- [ ]  Check for misconfigured namespaces
- [ ]  Review audit logs (if accessible)

---
### 5. Exploitation & Post-Exploitation
- [ ]  Use stolen tokens to access secrets
- [ ]  Abuse AppRole to mint new tokens
- [ ]  Inject malicious payloads into secrets (e.g., for CI/CD poisoning)
- [ ]  Pivot using credentials (e.g., AWS keys, DB creds)
- [ ]  Attempt to unseal Vault if keys are exposed

---
