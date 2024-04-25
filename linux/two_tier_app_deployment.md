# Deploying a tier 2 app

Make sure you have correctly configured your EC2 instances and security groups for both nginx and mongodb. Check the hosting nginx and mongodb readmes.

A tier 2 application/architecture is one where we use two machines that communicate with each other to host our app. In this case it is two AWS EC2 instances. One to host the nginx webserver and the other for the mongo database. We'll be using nginx to reverse proxy so that our public IP hosts the database

### Reason to use a tier 2 application

This is for segmentation which helps with fault tolerance, scalability and security (covered in more depth in the Two tier vs Monolithic readme).

### First things first

Again double check that the app and database instances are correctly configured, and we have ssh'd into them. 

*This next part is important* WE MUST SET UP OUR DATABASE INSTANCE WITH THE SCRIPT FIRST BEFORE THE APP INSTANCE. DO NOT RUN THE APP SCRIPT FIRST. 

This will cause us errors, and we'll have to restart, as the app instance connects to the database, if there is no database first set up to connect to will get errors

## Run mongodb script in database instance

Create the script by using ```nano <script name>.sh```. Always remember we do each part manually first before making it a script. 

The section where we enter the mongodb config files to change the bind ip and nginx config files to change the location ip to host our database would need ```sudo nano <file path>```, as a normal user doesn't have the permissions to access them.

In the script be aware of spaces and especially tabs, as linux is sensitive to these and errors will arise

### Mongo db script

```
#!/bin/bash

# non interactive when updating and installing
echo updating...
sudo apt update -y
echo done

#upgrading 
echo upgarde packages...
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo done

# install mongo db 7.0.6
# (initially could install latest version of mongo db (just so works, but has to change)
#curl used only if not installed already
sudo apt-get install gnupg curl

# - get public key for mongodb
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor --yes

# - create list file 
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

# - reload local package files to update
sudo apt-get update

# - install mongodb packages (specific 7.0.6)
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y mongodb-org=7.0.6 mongodb-org-database=7.0.6 mongodb-org-server=7.0.6 mongodb-mongosh=2.2.4 mongodb-org-mongos=7.0.6 mongodb-org-tools=7.0.6

# hold version at installed version so that no clashes with updates
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections

# - check if mongo db is running 
sudo systemctl status mongod


# configure bind IP in mongo db config file - change it 0.0.0.0 (for testing only)
# go into configuration folder
# sudo nano /etc/mongod.conf (manual) or another automation - sudo sed -i "s,\\(^[[:blank:]]*bindIp:\\) .*,\\1 0.0.0.0," /etc/mongod.conf

sudo sed -i -r 's/(\b[0-9]{1,3}\.){3}[0-9]{1,3}\b'/"0.0.0.0"/ /etc/mongod.conf 

# restart mongo db (or start if not yet started) to update configuration
sudo systemctl restart mongod

# enable mongo db 
sudo systemctl enable mongod
```

Check your mongodb database is running with ```sudo systemctl status mongod```. Only when it is available can you move to your app instance and run the app script there.

## Run app script in app instance

### App script 

```
#!/bin/bash
 
# echo update
echo updating...
sudo DEBIAN_FRONTEND=noninteractive apt update -y
echo done
 
# echo upgrade packages...
echo upgarde packages...
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo done
 
# install nginx
echo installing ngnix..
sudo DEBIAN_FRONTEND=noninteractive apt install nginx -y
echo done

# configure reverse proxy by changing config file
# manually is sudo nano /etc/nginx/sites-available/default then
# go to location and change (try_files $uri $uri/ =404;) to (proxy_pass http://localhost:3000;) scroll to bottom first for backup instructions

sudo sed -i '51s/.*/\t  proxy_pass http:\/\/localhost:3000;/' /etc/nginx/sites-enabled/default

 
# restart nginx
echo restarting ngnix..
sudo systemctl restart nginx
echo done
 
# enable nginx
echo enabling nginx...
sudo systemctl enable nginx
echo done

# install nodejs v20 
echo install node js v20...
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo DEBIAN_FRONTEND=noninteractive -E bash - && sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs
echo done

# check node js version
echo checking node js verison...
node -v
echo done

#set DB_HOST env var. use ip of your database (this will change with each instance)
export DB_HOST=mongodb://172.31.57.241:27017/posts
printenv DB_HOST

# get the app folder with the app code inside 
echo getting app folder...
git clone https://github.com/bitbanditt/tech258_test_app.git
echo done

# cd into app folder
echo going to app folder...
cd ~/tech258_test_app/app
echo inside app folder

# install app
echo installing app...
npm install
echo finished installing app

# start npm (dont run as clash, running/starting npm n pm2 causes issues)
#echo starting npm...
#npm start

# install pm2
echo installing pm2...
sudo -E npm install -g pm2
echo finished installing pm2

# stop processes
echo stopping processes...
sudo pm2 stop all
echo processes stopped

# run app
echo running app
pm2 start app.js
echo done

# put in correct place for reverse proxy
# go to /etc/nginx/sites-available/default use ls and copy the default file to back it up with
# sudo cp default default.bk (this copies the file), got into it and cat the files, check it out
# make the changes beneath 

# sudo nano /etc/nginx/sites-available/default
# back up the file and if all is well can swap the files
# go to location and change (try_files $uri $uri/ =404;) to (proxy_pass http://localhost:3000;)
#sudo sed -i '51s/.*/\t  proxy_pass http:\/\/localhost:3000;/' /etc/nginx/sites-enabled/default
```
Now when we go to the public IP of our app instance it will be hosting the database.

#### What is reverse proxy?

We enter the config files of nginx so that it hosts the database on its public IP (app instance).

#### what is sudo sed?

The ```sudo sed``` command allows us to automate the process of going into the config files and changing what we need to.


### Blockers experienced

*blocker: process clashing.* We need to install npm (node package manager) to install pm2 (process package manager 2) however we must take care as to how we start npm as it will take up a port trying to run nodejs where we need pm2 to do this process, as it runs it in the background. 

It is important to check running processes. We can search for a process via ```ps aux | grep (process name)``` and can kill it via ```sudo kill (process ID)```

*blocker 2:* In the app (nginx) script We need to make sure our DB_HOST environment variable is properly set via the script as the app won't go and find our database. It needs to be set to the private IP of the database instance, which will change if we terminate the instance. In the main terminal we can check if it is set with ```printenv DB_HOST```

*blocker 3:* As the app needs to find the database via the export environment variable we set, we need to make sure the database is up and running first before we run the app instance to connect to it. 

*blocker 4:* We have to make sure the correct IPs are set for the mongodb bind IP, and nginx location for reverse proxy 