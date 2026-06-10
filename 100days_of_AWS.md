### Task 1
- create a key pair with the following requirements:
- Name of the key pair should be datacenter-kp.
- Key pair type must be rsa

### Solution
```
- login using aws cli
aws configure

- get the credentials in another terminal
cat ~/.aws/credentials

- copy the aws id and acess key
- use this to login to the aws configure output

- create the rsa key pair with the above given requirements
aws ec2 create-key-pair \
  --key-name datacenter-kp \
  --key-type rsa \
  --region us-east-1

- check that it has been created
aws ec2 describe-key-pairs \
  --key-names datacenter-kp \
  --region us-east-1
```
### Task 2
- For this task, create a security group under default VPC with the following requirements:

* Name of the security group is `devops-sg`.
* The description must be `Security group for Nautilus App Servers`
* Add the inbound rule of type `HTTP`, with port range of `80`. Enter the source CIDR range of `0.0.0.0/0`.
* Add another inbound rule of type `SSH`, with port range of `22`. Enter the source CIDR range of `0.0.0.0/0`.

### Solution
```
A Security Group is essentially a virtual firewall for your AWS resources. It controls what traffic is allowed in (inbound) and out (outbound) of your servers.

Security Group vs Firewall
Traditional setup:          AWS setup:
Physical Firewall           Security Group
      ↓                           ↓
  Your Server               Your EC2/VM
Same idea — just virtual and attached directly to each resource.

Inbound vs Outbound Rules
DirectionMeaningExampleInboundTraffic coming INTO your serverSomeone visiting your websiteOutboundTraffic going OUT from your serverYour server downloading updates

By default AWS blocks all inbound and allows all outbound traffic.


Understanding the Rules in This Challenge
HTTP  port 80   0.0.0.0/0  → anyone on the internet can visit your website
SSH   port 22   0.0.0.0/0  → anyone can attempt to SSH into your server
0.0.0.0/0 means "from anywhere" — all IP addresses on the internet.

What is the Default VPC?
When you create an AWS account, Amazon automatically creates a default VPC in every region. It's a ready-to-use network so you can deploy resources immediately without setting up networking from scratch — similar to what you learned with Azure VNets.

How to Solve It

Option 1 — AWS Console (UI)

Log into AWS Console → search EC2 → left sidebar find Security Groups
Click Create Security Group
Fill in:

FieldValueNamedevops-sgDescriptionSecurity group for Nautilus App ServersVPCSelect the default VPC

Under Inbound Rules click Add Rule:

TypeProtocolPortSourceHTTPTCP800.0.0.0/0SSHTCP220.0.0.0/0

Click Create Security Group


Option 2 — AWS CLI (Recommended for DevOps)
bash# Step 1 — Get the default VPC ID
aws ec2 describe-vpcs \
  --filters "Name=isDefault,Values=true" \
  --query "Vpcs[0].VpcId" \
  --output text
This returns something like vpc-0abc123def456. Use it in the next command.
bash# Step 2 — Create the security group
aws ec2 create-security-group \
  --group-name devops-sg \
  --description "Security group for Nautilus App Servers" \
  --vpc-id <your-default-vpc-id>
This returns a GroupId like sg-0123456789abcdef0. Use it below.
bash# Step 3 — Add HTTP inbound rule (port 80)
aws ec2 authorize-security-group-ingress \
  --group-id <your-sg-id> \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0
bash# Step 4 — Add SSH inbound rule (port 22)
aws ec2 authorize-security-group-ingress \
  --group-id <your-sg-id> \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0
bash# Step 5 — Verify everything looks correct
aws ec2 describe-security-groups \
  --group-names devops-sg \
  --query "SecurityGroups[0].{Name:GroupName,Desc:Description,Rules:IpPermissions}" \
  --output table

The Full Picture — How it all connects
Internet (0.0.0.0/0)
        ↓
  Security Group: devops-sg
  ┌─────────────────────────┐
  │ Inbound: port 80 ✓      │  ← web traffic allowed
  │ Inbound: port 22 ✓      │  ← SSH allowed
  │ All else: BLOCKED ✗     │
  └─────────────────────────┘
        ↓
   Default VPC
        ↓
   EC2 App Servers

Key Things to Remember
ConceptWhat it meansSecurity GroupVirtual firewall attached to resourcesInbound ruleWho can connect TO your serverPort 80 / HTTPWeb traffic — browsers use thisPort 22 / SSHRemote terminal access to server0.0.0.0/0Allow from any IP addressDefault VPCAuto-created network in your AWS account
```



