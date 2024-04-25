# Hosting a Mongo DB app 

First set your EC2 instance, with the ssh security group and add security group with a custom tcp with port 27017 (which mongodb runs on) and allow connection from all 0.0.0.0. this is not best practice but is being done as we are just testing.

We do not add any HTTP access as we do not want anybody to be able to connect to the database. It is a security issue and we are just hosting.

### updating and upgrading

When logging into our ubuntu virtual machine it is custom we update it and upgrade, though upgrading is a command we wish to be careful about.

```sudo apt update -y```

```sudo apt upgrade -y```

### install mongo db 7.0.6
* #### Download 

This step is used if the two commands aren't installed already. Even if they are it doesn't hurt to run this command just incase. 
```sudo apt-get install gnupg curl```

* ####  Get public key for mongodb

```
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor --yes
```
* ####  Create list file 
```echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list```

* #### Reload local package files to update
```sudo apt-get update```

* #### install mongodb packages (specific 7.0.6)
```sudo DEBIAN_FRONTEND=noninteractive apt-get install -y mongodb-org=7.0.6 mongodb-org-database=7.0.6 mongodb-org-server=7.0.6 mongodb-mongosh=2.2.4 mongodb-org-mongos=7.0.6 mongodb-org-tools=7.0.6```

* #### hold at installed version so that no clashes with updates (as update is automatic)

```echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
```
### Mongodb checks 

#### - check if mongo db is running 
```sudo systemctl status mongodb```

### configure bind IP in mongo db config file - change it 0.0.0.0 (for testing only)

go into configuration folder

```sudo nano /etc/mongod.conf```

in the bind ip section change the ip to desired 0.0.0.0

or automate via

```sed -i -r 's/(\b[0-9]{1,3}\.){3}[0-9]{1,3}\b'/"0.0.0.0"/ /etc/mongod.conf```

The sed command searches text file and replaces it with given text

Necessary so that the mongo db database can be accessed from outside the local machine.

### restart mongo db (or start if not yet started) to update configuration

```sudo systemctl restart mongodb```

restart is the better option as if connections or the like fail it will restart itself

### enable mongo db 

```sudo systemctl enable mongodb```

This allows for the app to be launched on boot of system.




