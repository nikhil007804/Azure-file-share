# Azure-File-Share
 
## Overview
 
This project provisions an Azure File Share using Terraform.  

Azure File Share is a fully managed file share in the cloud that you can mount simultaneously via the SMB protocol from Windows, Linux, and macOS. It is ideal for scenarios where you need shared storage accessible by multiple VMs or services.
 
## Key Features of Azure File Share
 
- Fully managed, highly available file storage in Azure.

- Supports SMB and NFS protocols.

- Can be mounted on-premises or in the cloud.

- Integrates with Azure Active Directory for access control.

- Scalable and secure.
 
## Project Structure
 
- `provider.tf` – Configures the Azure provider and subscription (give your subscription id).

- `main.tf` – Declares the resource group, storage account, and file share.

- `variables.tf` – Defines input variables for customization.

- `terraform.tfvars` – Sets actual values for the variables.

- `.github/workflows/deploy.yml` – GitHub Actions workflow for CI/CD.
 
## Step-by-Step Usage
 
### 1. Prerequisites
 
- Terraform installed.

- An Azure subscription.

- Azure CLI installed and authenticated (`az login`).

- (For CI/CD) A Service Principal and `AZURE_CREDENTIALS` secret set in your GitHub repository.
 
### 2. Clone the Repository
 
```bash

git clone https://github.com/Pratyushaa94/Azure-File-Share.git

cd Azure-File-Share

```
 
### 3. Configure Variables
 
Edit `terraform.tfvars` to set your desired values:
 
```hcl

resource_group_name   = "my-resource-group"

location              = "East US"

storage_account_name  = "prathyusha9sj2"

file_share_name       = "prathyushafileshare"

file_share_quota      = 10

```
 
> **Note:** Storage account names must be globally unique and use only lowercase letters and numbers.
 
### 4. Initialize Terraform
 
```bash

terraform init

```
 
### 5. Review the Execution Plan
 
```bash

terraform plan

```
 
### 6. Apply the Configuration
 
```bash

terraform apply

```
 
Type `yes` when prompted to confirm.
 
### 7. (Optional) Destroy Resources
 
```bash

terraform destroy

```
 
## ⚠️ Important: Subscription ID Must Be Set
 
Make sure to update your `provider.tf` to include your Azure Subscription ID:
 
```hcl

provider "azurerm" {

  features {}

  subscription_id = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  # Replace with your actual Subscription ID

}

```
 
Get your Subscription ID using:
 
```bash

az account show --query id --output tsv

```
 
Alternatively, you can set it using environment variables (especially useful in CI/CD):
 
```bash

export ARM_SUBSCRIPTION_ID="your-subscription-id"

export ARM_CLIENT_ID="your-client-id"

export ARM_CLIENT_SECRET="your-client-secret"

export ARM_TENANT_ID="your-tenant-id"

```
 
## CI/CD with GitHub Actions
 
This project includes a workflow (`.github/workflows/deploy.yml`) that automatically deploys your infrastructure on push or pull request to the `main` branch.
 
To use:
 
1. Create an Azure Service Principal.

2. Add its credentials as the `AZURE_CREDENTIALS` secret in your GitHub repository.

3. Push your changes to GitHub. The workflow will run Terraform commands automatically.
 
## Theory: How Does Azure File Share Work?
 
Azure File Share provides a cloud-based SMB file share that can be mounted by multiple clients.
 
- **Resource Group**: Logical container for Azure resources.

- **Storage Account**: Securely stores your file shares, blobs, queues, and tables.

- **File Share**: The actual SMB/NFS share you can mount and use for file storage.
 
### Use Cases
 
- Shared storage for applications running on multiple VMs.

- Lift-and-shift of on-premises file shares to the cloud.

- Centralized file storage for distributed teams.
 
## Important Notes
 
- Storage account names must be unique across Azure.

- File share quota is specified in GB.

- Ensure your Azure credentials are set up for both local and CI/CD deployments.
 
## ⚠️ Common Terraform Warnings
 
When running `terraform plan` or `terraform apply`, you may see warnings like:
 
```

Warning: Value for undeclared variable
 
The root module does not declare a variable named "account_tier" but a value was found in file "terraform.tfvars".

...

The root module does not declare a variable named "account_replication_type" but a value was found in file "terraform.tfvars".

```
 
These warnings are common and can be safely ignored if your deployment succeeds. They simply mean that there are extra variables in your `terraform.tfvars` file that are not used in your Terraform configuration. You can remove these variables from `terraform.tfvars` to silence the warnings, but they do not affect the deployment.
 
## References
 
- [Azure Files Documentation](https://learn.microsoft.com/en-us/azure/storage/files/)
 
