### Task 1
The Nautilus DevOps team is planning to migrate a portion of their infrastructure to the Azure cloud incrementally. As part of this migration, you are tasked with creating an Azure Virtual Machine (VM).

The requirements are:

1) Use the existing resource group.

2) The VM name must be xfusion-vm, it should be in westus region.

3) Use the Ubuntu 24.04 LTS image for the VM.

4) The VM size must be Standard_B1s.

5) Attach a default Network Security Group (NSG) that allows inbound SSH (port 22).

6) Attach a 30 GB storage disk of type Standard HDD.

7) The rest of the configurations should remain as default.

After completing these steps, make sure you can SSH into the virtual machine.


Use below given Azure Credentials: (You can run the showcreds command on the azure-client host to retrieve credentials)
```
Portal URL	https://portal.azure.com
Username	kk_lab_user_main-f07d218f62444ab0@azurefreekmlprod.onmicrosoft.com
Password	pE3nPcrc
Start Time	Tue May 19 22:51:43 UTC 2026
End Time	Tue May 19 23:51:43 UTC 2026
```

### Solution

Day 1 — Create an Azure Virtual Machine (Ubuntu 24.04 LTS)

Challenge: KodeKloud 100 Days of Azure
Topic: Azure Virtual Machines
Difficulty: Beginner


- Task Summary
Create an Azure Virtual Machine for the Nautilus DevOps team as part of a cloud migration exercise. The VM must meet specific naming, region, image, size, networking, and storage requirements.

- Requirements
RequirementValueResource GroupUse existing (do not create a new one)VM Namedatacenter-vmRegioneastusOS ImageUbuntu 24.04 LTSVM SizeStandard_B1sNetwork Security GroupDefault NSG with inbound SSH (port 22) allowedOS Disk Size30 GBDisk TypeStandard HDD (Standard_LRS)

- Concepts to Know

- Resource Group — A logical container for Azure resources. Always deploy into the correct one.
- VM Size — Standard_B1s is a burstable, low-cost size (1 vCPU, 1 GB RAM). Good for dev/test.
- NSG (Network Security Group) — Acts as a firewall for your VM. 
- The SSH rule opens port 22 for remote access.
- Storage SKU — Standard_LRS = Standard HDD with Locally Redundant Storage. Cheapest disk option.
- Image Alias — Azure uses Ubuntu2404 as the short alias for Ubuntu 24.04 LTS.
--generate-ssh-keys — Auto-creates an SSH key pair at ~/.ssh/id_rsa if one doesn't exist.


🛠️ Step-by-Step Solution
Step 1 — Log into Azure CLI
az login \
  --username <your-username> \
  --password <your-password>

Step 2 — Find the Existing Resource Group
az group list --output table
Note the Name column. You will use it in the next step.

Step 3 — Create the VM
Replace <RESOURCE_GROUP> with the name from Step 2.
az vm create \
  --resource-group <RESOURCE_GROUP> \
  --name datacenter-vm \
  --location eastus \
  --image Ubuntu2404 \
  --size Standard_B1s \
  --nsg-rule SSH \
  --os-disk-size-gb 30 \
  --storage-sku Standard_LRS \
  --generate-ssh-keys

Step 4 — Get the Public IP Address
az vm show \
  --resource-group <RESOURCE_GROUP> \
  --name datacenter-vm \
  --show-details \
  --query publicIps \
  --output tsv

Step 5 — SSH into the VM
ssh -i ~/.ssh/id_rsa azureuser@<PUBLIC_IP>

The default admin username for Ubuntu Azure VMs is azureuser.


Step 6 — Verify Inside the VM
bash# Confirm OS version
lsb_release -a

# Confirm disk size
lsblk

# Confirm VM size via instance metadata
curl -s -H Metadata:true \
  "http://169.254.169.254/metadata/instance/compute/vmSize?api-version=2021-02-01&format=text"



-Key Takeaways

Always use --nsg-rule SSH when you need SSH access from the CLI — it handles NSG creation automatically.
Standard_LRS is the correct SKU name for Standard HDD in Azure CLI (not HDD or StandardHDD).
--generate-ssh-keys is the fastest way to set up key-based auth on a lab VM.
The [tool.ruff.lint] section placement matters — Azure CLI flag names are exact and case-sensitive.
Azure assigns azureuser as the default admin account for Ubuntu images unless overridden with --admin-username.




### Task 2
- Create a Virtual Network (VNet) named nautilus-vnet in the southcentralus region with any IPv4 CIDR block.

### Solution
- use azure cli to solve it 
- login using az login
- get the resource using az group list
```
az network vnet create \
  --name nautilus-vnet \
  --location southcentralus \
  --resource-group kml_rg_main-356c29092c1b4987 \
  --address-prefix 10.0.0.0/16
```

### Track 3
- The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the Azure cloud.
- Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition.
- Their approach involves creating Virtual Networks (VNets) as the initial step, as they will be provisioning various services under different VNets.

- Create a Virtual Network (VNet) named devops-vnet in the southcentralus region with 192.168.0.0/24 IPv4 CIDR.



- Use below given Azure Credentials: (You can run the showcreds command on the azure-client host to retrieve credentials)

### Solution
# Login with the service principal, you get this from using showcreds on the cli- get the azure application client id and  azure client secret
az login --service-principal \
  --username d6c4c35c-1992-430f-a440-a2c1834fcffc \
  --password "azure client secret" \
  --tenant azurefreekmlprod.onmicrosoft.com

# Find the resource group name and copy it
az group list --query "[].name" -o table

# Create the VNet
az network vnet create \
  --name devops-vnet \
  --resource-group <resource-group-name> \
  --location southcentralus \
  --address-prefix 192.168.0.0/24

# Verify it was created
az network vnet show \
  --name devops-vnet \
  --resource-group <resource-group-name> \
  --query "{name:name, location:location, addressSpace:addressSpace}" \
  -o table

```
