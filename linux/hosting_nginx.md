# Deploy an app via nginx

Follow the readme to configure EC2 instance and when at the security groups, add an extra group of custom TCP and make the port 3000. This is the port nginx runs on. 

Once in your Ubuntu EC2 instance we can automate the steps by creating a BASH (Borne Again SHell) script to do the process for us (As DevOps engineers we always want to automate where possible). However, always remember to manually complete steps first before turning it into a script to make sure the commands run as they should. 

When you've created the script, run it on multiple instances making sure it works each time (ident potent). You can troubleshoot by manually checking your script commands if your facing errors. Make sure to read the error messages, they help and point you in the direction to look into and research via Google or AI etc.

### Creating script/installation steps

## 1. #### nano in and create script

Use the command ```nano script.sh``` to open the nano text editor. The file name can be whatever you wish to call it. What is important is that you use ```.sh``` to indicate to linux it is a script file

Remember to give execute permissions after the script is created. In this case it is easier to use ```chmod 777 <script name>``` which gives all permissions (read write and execute).

## 2. #### Nginx script/installation

```#!/bin/bash```

This lets the system know we want to run our script in the BASH shell not the system shell
 
#### echo update
```
echo updating...
sudo DEBIAN_FRONTEND=noninteractive apt update -y
echo done
```
The ```echo``` command is similar to the print command in python. We use it here to help track the progress of our installation.

The  ```DEBIAN_FRONTEND=noninteractive``` portion of our command allows us to avoid manually having to accept the yes/no portions that appear when having to accept or install in parts of the code.

#### echo upgrade packages...

The "pending kernel upgrade" is a message we usually receive that the above explanation of using  Debian noninteractive handles.

```
echo upgarde packages...
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo done
```

Always remember we first update and upgrade (take care with upgrading to avoid package clashes) when entering a linux based system. In this case Ubuntu.
 
#### install nginx

At this step we would receive another user accept input for "pending kernel upgrade" but we have delt wit it.

```
echo installing ngnix..
sudo DEBIAN_FRONTEND=noninteractive apt install nginx -y
echo done
```

#### restart nginx

```
echo restarting ngnix..
sudo systemctl restart nginx
echo done
```
We use restart to start nginx so if for any reason nginx goes down it can reboot itself, using start instead wouldn't give that extra functionality
 
#### enable nginx

```
echo enabling nginx...
sudo systemctl enable nginx
echo done
```
 We use enable so that nginx can start on boot up of the system without prompting.

#### install nodejs v20 

```
echo install node js v20...
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo DEBIAN_FRONTEND=noninteractive -E bash - && sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs
echo done
```

#### check node js version

```
echo checking node js verison...
node -v
echo done
```

#### get the app folder with the app code inside 

```
echo getting app folder...
git clone https://github.com/bitbanditt/tech258_test_app.git
echo done
```

#### cd into app folder

```
echo going to app folder...
cd ~/tech258_test_app/app
echo inside app folder
```
We go into the app folder to install the dependencies we need. If we need to troubleshoot, sometimes it wil be from here we need to do so.

#### install app

```
echo installing app...
npm install
echo finished installing app
```
Using this command from inside the app folder is a way to check our node js was properly installed if it is not outputting the expected page when we go to our public IP

#### start npm

```
echo starting npm...
npm start
echo npm started...
```
We use npm (node package manager) to install pm2 (process manager 2). For now we can do this step, but in our two tier app deployment we must take it out as it uses the port pm2 needs to run our app, causing a clash.

#### install pm2

```
echo installing pm2...
sudo npm install -g pm2
echo finished installing pm2
```

#### stop processes

```
echo stopping processes...
sudo pm2 stop all
echo processes stopped
```

#### run app

```
echo running app
pm2 start app.js
echo done
```

Now when you paste the public IP of your in your browser it will take you to the desired page.