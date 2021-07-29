# Overview
Use an Azure DevOps pipeline to create a golden image, which can be used for deployment to Virtual Machines, or Virtual Desktops.

Leveraging HashiCorp Packer, this Repo will demonstrate how to automate the lifecycle of your VM Images. I like to consider this the Docker Build of Virtual Machines..

## Prerequisite
You'll need the following pre-reqs before getting started.

### Azure DevOps

- Account (Can be free version!)
- Access to an existing or new Azure Devops project

### Azure

- Service Principal information

`az ad sp create-for-rbac --name Converge2020`
- Storage Account
    - Document Storage account access key
- Container within Storage Account for media installation files
    - Download and upload [Citrix VDA](https://www.citrix.com/downloads/citrix-cloud/product-software/xenapp-and-xendesktop-service.html) *VDAServerSetup_2006.exe*
    - Download and upload [Citrix Optimizer](https://support.citrix.com/article/CTX224676) (*CitrixOptimizer.zip*)
- Container within Storage Account for built gold images to reside

## Steps
Follow the below process to build a Windows 2019 VDA

1. From Azure DevOps. Select Repos

2. From Pipelines create new
    - Select "Azure Repos Git"
    - Select imported repo (eg Converge2020)
    - Existing Azure Pipelines YAML file
    - Select `/pipeline.yml`
    - Select Variables and add the below table
3. Run pipeline

### Pipeline Variables
| Variable | Description | Example |
| -------- | ------------| ------- |  
| client_id | SPN Client ID (appid) | Secret |
| client_secret | SPN Client Secret (password) | Secret |
| goldimage_container | Storage Account Container that will contain gold image | goldimages |
| location | Location to deploy temp VM  | centralus |
| media_container | Storage account container that contatins install media | media|
| rg_name | Resource group where storage account is located | rg-converge-central |
| sa_key | Storage Account Key | Secret |
| storage_account | Storage Account Name | mysupercoolpackerstorage |
| subid | Azure Subscription ID | Secret |
| tenantid | SPN Tenant ID (tenant) | Secret |
| vda | VDA File Name | VDAServerSetup_2006.exe |
| vdacontrollers | Comma seperated list of cloud connectors | CTX-CC01.LAB.LOCAL |


# SIG

```bash
az sig image-definition create --resource-group p-no1cx3-images --gallery-name pno1cx3imagesgallery --gallery-image-definition p-intwp1-a --publisher Innofactor --offer CitrixVDAs --sku p-intwp1-a --os-type Windows
```
