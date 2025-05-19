# Multi-cloud Serverless Inventory
This project inventories serverless services across AWS, GCP, Azure, and Alibaba Cloud, and generates JSON, CSV, and HTML reports sent via email.


# Prerequisites
Ensure the following CLI tools and Ansible collections are installed:
Ansible Documentation

CLI Tools
AWS CLI

Google Cloud SDK (gcloud)

Azure CLI (az)

Alibaba Cloud CLI (aliyun)

Ansible Collections
Define required collections in requirements.yml:
```
yaml

collections:
  - name: amazon.aws
  - name: google.cloud
  - name: azure.azcollection
  - name: community.general
```
Install them using:
```
ansible-galaxy collection install -r requirements.yml
```

# Region Support
Specify regions for each cloud provider in vars/main.yml:

yaml
Copy
Edit
aws_regions:
  - us-east-1
  - us-west-2

gcp_regions:
  - us-central1
  - europe-west1

azure_regions:
  - eastus
  - westeurope

alibaba_regions:
  - cn-hangzhou
  - ap-southeast-1
Each role will iterate over its respective regions to gather serverless service information.

#  Validation and Testing
Implement validation tasks in each role to ensure required CLI tools are available and credentials are configured. For example, in roles/aws_serverless/tasks/main.yml:

yaml
Copy
Edit
- name: Ensure AWS CLI is installed
  command: aws --version
  register: aws_cli
  failed_when: aws_cli.rc != 0
  changed_when: false

- name: Validate AWS credentials
  shell: aws sts get-caller-identity --region {{ item }}
  loop: "{{ aws_regions }}"
  register: aws_identity
  failed_when: aws_identity.rc != 0
  changed_when: false
Similar validation tasks should be implemented for GCP, Azure, and Alibaba roles.

#  Reporting
The reporting role will compile the collected data into JSON, CSV, and HTML formats and send them via email.

## Sample Task: Generate CSV Report
```
yaml

- name: Generate CSV report
  copy:
    content: |
      cloud,service,name,region,trigger
      {% for item in serverless_inventory %}
      {{ item.cloud }},{{ item.service }},{{ item.name }},{{ item.region }},{{ item.trigger }}
      {% endfor %}
    dest: "{{ report_dir }}/serverless_inventory.csv"

```
## Sample Task: Send Email
```
yaml

- name: Send report via email
  mail:
    host: "{{ smtp_host }}"
    port: "{{ smtp_port }}"
    username: "{{ smtp_user }}"
    password: "{{ smtp_pass }}"
    to: "{{ email_to }}"
    subject: "Multi-Cloud Serverless Inventory Report"
    body: "Please find the attached serverless inventory reports."
    attach:
      - "{{ report_dir }}/serverless_inventory.csv"
      - "{{ report_dir }}/serverless_inventory.json"
      - "{{ report_dir }}/serverless_inventory.html"

```

#  Test Playbooks
Create test playbooks in the tests/ directory to validate each role independently. For example, tests/test_aws.yml:
```
yaml

- name: Test AWS Serverless Role
  hosts: localhost
  gather_facts: no
  roles:
    - aws_serverless
```

Run the test playbook using:
```
bash

ansible-playbook tests/test_aws.yml

```
# roles/aws_serverless/tasks/main.yml
AWS Serverless Services Covered:
AWS Lambda

AWS Step Functions

Amazon EventBridge

AWS App Runner

AWS Fargate (for ECS Tasks)

Amazon S3 (event-based serverless trigger)

# roles/gcp_serverless/tasks/main.yml
GCP Serverless Services Covered:
Cloud Functions (1st & 2nd gen)

Cloud Run

Workflows

# roles/azure_serverless/tasks/main.yml
Azure Serverless Services Covered:
Azure Functions

Azure Logic Apps

Azure Container Apps

# roles/alibaba_serverless/tasks/main.yml
Alibaba Cloud Serverless Services Covered:
Function Compute (FC)

Serverless App Engine (SAE)
