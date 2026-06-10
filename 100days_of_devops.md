### Task 1
__Day 1: Linux User Setup with Non-Interactive Shell__

#### Objective
- To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell.
- Here's your task:
- Create a user named rose with a non-interactive shell on App Server 3.

#### Solution
-Create a user with a non-interactive shell — this means the user can exist on the system (for service/backup purposes) but cannot log in interactively.
Solution
- Connect to App Server 3 first (via SSH), then run:
- useradd — command to create a new user
-s /sbin/nologin — sets the shell to /sbin/nologin, which is a non-interactive shell (it simply rejects any login attempt)
- rose — the username

```
  ssh banner@stapp3 - connct to App Server 3 via ssh
  sudo useradd -s /sbin/nologin rose 
  grep rose /etc/passwd  - to check wether it has been effected
  
```

### Task 2
- As part of the temporary assignment to the Nautilus project, a developer named kareem requires access for a limited duration. 
- To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:

- Create a user named kareem on App Server 2 in Stratos Datacenter. 
- Set the expiry date to 2027-01-28, ensuring the user is created in lowercase as per standard protocol.

### Solution
Step 1 — SSH into App Server 2
In the KodeKloud Stratos Datacenter, App Server 2 is stapp02. SSH into it:
```
ssh steve@stapp02
```
The default user for App Server 2 is steve. Use the lab-provided password when prompted.


- Step 2 — Switch to root
```
bashsudo su -
```
- Step 3 — Create the user with an expiry date
```
useradd -e 2027-01-28 kareem
```
-Flag breakdown:
-e 2027-01-28 Sets the account expiry date in YYYY-MM-DD

Step 4 — Verify the user was created correctly
```
chage -l kareem
```
- Look for the Account expires line in the output:
Account expires                         : Jan 28, 2027
You can also double-check with:
```
grep kareem /etc/passwd
```

### Task 3
- Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.
- Your task is to disable direct SSH root login on all app servers within the Stratos Datacenter.

### Solution
Here's how to disable direct SSH root login on all app servers:
What needs to change: In /etc/ssh/sshd_config, set PermitRootLogin no, then restart the SSH service.
Do this on each app server (stapp01, stapp02, stapp03):
bash# SSH into each server (example for stapp01)
ssh tony@stapp01

```
# Once in, switch to root
sudo su -

# Edit the SSH config
sed -i 's/^PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config

# Verify the change
grep PermitRootLogin /etc/ssh/sshd_config

# Restart SSH service
systemctl restart sshd
Repeat with steve@stapp02 and banner@stapp03.

# Should return "no"
sshd -T | grep permitrootlogin
```

### Task 4
- In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named xfusioncorp.sh. 
- While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 3 within the Stratos Datacenter.
- Your task is to grant executable permissions to the /tmp/xfusioncorp.sh script on App Server 3.
- Additionally, ensure that all users have the capability to execute it.

### Solution
```
# ssh into app server 3
ssh banner@stapp03
# enter the password

# check the script permission
ls -l /tmp/xfusioncorp.sh
# run give it read and execute permission. as we can't execute without read permission
sudo chmod a+rx /tmp/xfusioncorp.sh
```

### Task 5
- Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, - -- the following requirements have been established for App server 1 in the Stratos Datacenter:

- Install the required SELinux packages.
- Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
- No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
- Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

### Solution
```
- login into the app server
ssh tony@stapp01

- put the password
- switch to the root
sudo su -
-enter the password
- install the SElinus using yum
yum install -y selinux-policy selinux-policy-targeted libselinux-utils policycoreutils

- vi open the config files
vi /etc/selinux/config

- find this line
SELINUX=enforcing

- change it to disabled
SELINUX=disabled

- verify
cat /etc/selinux/config | grep SELINUX=

```
### Track 6
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
