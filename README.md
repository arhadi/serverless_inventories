# Cloud Serverless Inventory Tool (Multi-Cloud)

This Ansible-based tool collects all serverless services from AWS, GCP, Azure, and Alibaba Cloud, then exports reports to CSV, JSON, and HTML formats and optionally emails them.

## 🧰 Prerequisites

| Cloud     | CLI Required | IAM Permissions (Read-only)                             |
|-----------|--------------|----------------------------------------------------------|
| AWS       | aws-cli      | `AWSLambda_ReadOnlyAccess`, `AmazonEC2ContainerServiceReadOnlyAccess`, `AWSStepFunctionsReadOnlyAccess` |
| GCP       | gcloud       | `roles/cloudfunctions.viewer`, `roles/run.viewer`, `roles/viewer` |
| Azure     | az-cli       | Reader role on Function Apps, Logic Apps                |
| Alibaba   | aliyun-cli   | `AliyunFCReadOnlyAccess`, `AliyunSAEReadOnlyAccess`     |

## 🚀 How to Run

```bash
ansible-playbook -i inventory/localhost site.yml
