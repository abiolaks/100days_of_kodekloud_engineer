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
### Task 6
- The `Nautilus` system admins team has prepared scripts to automate several day-to-day tasks.
- They want them to be deployed on all app servers in `Stratos DC` on a set schedule. Before that they need to test similar functionality with a sample cron job. - Therefore, perform the steps below:
a. Install `cronie` package on all `Nautilus` app servers and start `crond` service.
b. Add a cron `*/5 * * * * echo hello > /tmp/cron_text` for `root` user.

### Solution
```
What is Cron?
A cron is a time-based job scheduler in Linux. It runs commands/scripts automatically at specified times/intervals — without you being there.
What is Cronie?
Cronie is the package that provides the cron daemon (crond) on RHEL/CentOS systems. It's the service that watches your cron jobs and executes them.
cronie = the package you install
crond  = the service/daemon that runs in background
crontab = the file where you define your scheduled jobs
Reading the Cron Syntax
*/5 * * * *  echo hello > /tmp/cron_text

 │   │ │ │ │
 │   │ │ │ └── Day of week (0-7, Sun=0)
 │   │ │ └──── Month (1-12)
 │   │ └────── Day of month (1-31)
 │   └──────── Hour (0-23)
 └──────────── Minute - */5 means "every 5 minutes"
So this job runs every 5 minutes, echoes "hello" into /tmp/cron_text.

How to Solve It
The Nautilus lab typically has 3 app servers (stapp01, stapp02, stapp03). You need to do this on all three.

Step 1 — SSH into each app server
From the jump host:
bash# App Server 1
ssh tony@stapp01

# App Server 2  
ssh steve@stapp02

# App Server 3
ssh banner@stapp03

Passwords are usually Ir0nM@n, Am3ric@ and BigGr33n respectively — check your lab credentials.


Step 2 — On EACH app server, run these commands
bash# Switch to root
sudo su -

# Install cronie
yum install -y cronie

# Start the crond service
systemctl start crond

# Enable it so it survives reboots
systemctl enable crond

# Verify it's running
systemctl status crond

Step 3 — Add the cron job for root user
bash# Open root's crontab
crontab -e
This opens a text editor (vi). Add this line:
*/5 * * * * echo hello > /tmp/cron_text
Save and exit — in vi: press Esc, type :wq, hit Enter.
OR do it without opening the editor (cleaner for automation):
bash(crontab -l 2>/dev/null; echo "*/5 * * * * echo hello > /tmp/cron_text") | crontab -

Step 4 — Verify the cron job was added
bashcrontab -l
Expected output:
*/5 * * * * echo hello > /tmp/cron_text
```

