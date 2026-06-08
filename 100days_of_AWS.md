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



