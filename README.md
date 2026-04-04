# 🔐 Kubernetes RBAC & AWS IAM Integration (EKS)

## 📌 Overview

This project demonstrates **secure access control in Kubernetes (EKS)** using:

* Kubernetes RBAC (Role-Based Access Control)
* AWS IAM integration via `aws-auth` ConfigMap
* Service Accounts with IAM Roles (IRSA)

---

## 🧠 Key Concepts

* **RBAC** → Controls what users can do inside Kubernetes
* **IAM Mapping** → Maps AWS users to Kubernetes roles
* **Service Accounts** → Used by pods to access AWS securely

---

## 📂 Project Structure

```id="rbac123"
.
├── 01-role.yaml
├── 02-role-binding.yaml
├── 03-aws-auth.yaml
├── 04-admin-role.yaml
├── 05-admin-role-binding.yaml
├── 06-group-trainee-binding.yaml
├── 07-sa.yaml
├── 08-pod-test.yaml
```

---

## ⚙️ RBAC Implementation

### 🔹 Namespace Role (Trainee)

* Limited access to:

  * pods (get, list, watch)

```yaml id="role1"
resources: ["pods"]
verbs: ["get", "list", "watch"]
```

---

### 🔹 Admin Role

* Full access to all resources

```yaml id="role2"
resources: ["*"]
verbs: ["*"]
```

---

## 🔗 IAM to Kubernetes Mapping

Configured using `aws-auth` ConfigMap:

```yaml id="aws1"
mapUsers:
  - userarn: arn:aws:iam::<ACCOUNT_ID>:user/satya
    groups:
      - roboshop-trainee
```

---

## 👥 Access Model

| User   | Group            | Access Level |
| ------ | ---------------- | ------------ |
| satya  | roboshop-trainee | Read-only    |
| ramesh | roboshop-admin   | Full access  |

---

## 🔐 Service Account (IRSA)

* Created ServiceAccount with IAM role annotation:

```yaml id="sa1"
eks.amazonaws.com/role-arn: <IAM_ROLE_ARN>
```

* Enables pods to access AWS securely without hardcoded credentials

---

## 🧪 Testing Access

Used test pod:

```yaml id="test1"
serviceAccount: secret-reader
```

* Verified permissions using kubectl inside pod

---

## 🚀 Benefits

* Secure and scalable access control
* Least privilege enforcement
* Integration with AWS IAM
* No hardcoded credentials in pods

---

## 🔮 Future Improvements

* Add OPA/Gatekeeper policies
* Integrate with SSO (Okta/AWS SSO)
* Audit logging with CloudTrail

---

## 👨‍💻 Author

Pavan – DevOps Engineer
