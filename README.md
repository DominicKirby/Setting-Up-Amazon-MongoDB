# Running MongoDB on an Amazon EC2 cloud server

## Requirements

You will need access to the following tools to follow this instruction
- Access to EC2 on Amazon cloud,
- Access to Git Bash,
- Administrative rights over the server

## Setting up the cloud server

### Initial setup

Once logged into Amazon cloud services, launch a new instance with a unique and recognisable title. 

Then set each of the following choices up to the following options:
- Application to Ubuntu
- Version to 24.04 for compatibility
- Any preferred instance type
- Key pair file downloaded
- Set the security groups to My IP in the custom settings
- Add a new security group for port 27017 for MongoDB

## Git Bash Setup 

#### Moving the file
First step is moving the key file into .ssh folder in your directory to be accessed later.

Use the following command: 
```
mv ~/download_folder/key_title.pem ~/.ssh
```

#### Accessing the Linux cloud server 
This next line of code connects you into the server online:

```
ssh -i ~/.ssh/key_title.pem ubuntu@public_ip
```
Where public IPv4 address found on the instance information online, and the key title is the 
title you gave the key in the setup steps.

#### Updating and Upgrading the server
The next step is updating and upgrading the cloud server to ensure the setup is correct for
further use.

Computing the following 2 commands do this:
```
sudo apt update
sudo apt upgrade -y
```
Where -y skips a step of confirmation and allows for simpler progression.

## MongoDB installation
The first step is installing the correct packages to allow us to download everything onto the
Linux instance.
```
sudo apt instal gnupg curl
```

This installs gnupg and curl packages to be used later

The next step is creating the server and putting it in the correct directory

```
curl -fsSL https://pgp.mongodb.com/server-9.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-9.gpg \
   --dearmor
```

The next step is creating the list file for this version of Ubuntu:
```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-9.gpg ]
https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/9.0 multiverse" |
sudo tee /etc/apt/sources.list.d/mongodb-org-9.0.list
```

Finally we ensure everything is up to date and ready
```
sudo apt update
```
Then,
```
sudo apt install -y mongodb-org
```

#### Starting MongoDB in this instance

This code starts Mongo Daemon and checks it correctly loaded up
```
sudo systemctl start mongod
sudo systemctl status mongod
```

## Python Section 
The final step into accessing the Mongo Client from your local server, in your python editor
input the following code and run to connect to the server
```{python}
import pymongo

client = pymongo.MongoClient("mongodb://public_ip:27017")

db = client["ec2]
```

You can then deal with this in python and it will all be updated to the online server while 
it is running.

