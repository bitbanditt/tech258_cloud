# Using EC2 to deploy Nginx

### First steps

First is to log in to AWS and make sure you are in the correct region for where you'd like to use AWS services.

In your console will be a search bar or a services tab where you can choose the AWS services you'd like. Choose or search EC2.

### Launching an EC2 instance

Now you are in the EC2 console in orange will be the launch button. Press it to begin configuring your EC2 instance. 

You'll first be asked to name your instance. Give it a name that is descriptive of what service it is and what for, so it stands out to you if you have multiple services running.

Now you choose an AMI (Amazon Machine Image). An AMI is essentially a replicated model of a system or OS, that will contain usual packages it may include. This can also be configured. 

Next is choosing your instance type. There are many instance types to choose from, but to be efficient it is best to only use instances that have only the resources you need and nothing more.

Following this is choosing the key pair login. This is how you will access your instance. It gives you access to the server via an ssh key stored in a .pem file. This is an encrypted private key created by Amazon. This is a file that must be kept secure, and thus kept in your .ssh folder. 

After securely keeping your .pem file in your .ssh folder, you will move on to Network Settings. This is where you configure your security groups, which is  an AWS version of a firewall for EC2. Here is where access to and from the instance is configured, which network traffic is allowed to flow in and out of the instance. 

You have to make sure ssh access is ticked, so you can access your instance with the .pem file given. <b>
You can then set up more security groups by pressing add security group rules. Use the tab and find and add HTTP, which allows internet access. You are able to choose which IP addresses can connect to the instance also. 
* during this portion you will see *ports* and a number beside them. 
* *Ports* are accessible parts of a system and different protocols and processes use them.

Storage choice is next, here you can choose how much storage the instance has. Again, it's best to only use an amount you need and nothing more.

To the right should be the summary of the instance you are creating. Scroll through and double check all is well. 

After your checks press launch and now you have launched an EC2 instance!.

It will take a while to initialise. Once it is done you can press the link to go to the instance and see the details of it, check the status and much more. click the link of the instance

# Hosting Nginx via EC2

Now we are in the AMI that would be our username. Use the command ```sudo apt update -y``` when this process is complete use the command ```sudo apt upgrade```. These commands are used when first starting up a linux based system. 
* ```sudo```: is a command that gives the user permissions of an administrator for the command they are doing.
* ```apt```: is the package/software manager for Linux and is how you access the many array of packages.
* ```update```: is a safe command that checks the system has all the latest packages available. It can also be used to test internet connection.
* ```upgrade```: is the command the makes sure all the packages installed are at their latest version. This command can cause system breaks/crashes as installed software may conflict with newer versions of each other. 
* ```-y```: this command tells the system to choose yes for any questions we may be asked during the running of the process. It is used for automation purposes. 

### Installing Nginx 

The command ```sudo apt install nginx -y``` installs nginx. The process runs and when done use the command ```systemctl status nginx -y```  to launch and check health of the web server, press q to quit so you can keep navigating.

### Seeing the web server

Now we are hosting a webserver via our EC2 instance. We can go to our EC2 page and navigate to details. Copy the public IP and if you paste it you will see the nginx welcome page. If there were any information placed it will be shown. 

# What is Nginx

So what did we just install? 
* Nginx is an open source, high performance web server software used to serve web content, handle HTTP requests, and act as a reverse proxy and load balancer. Nginx has become popular for its efficiency, scalability, and low resource usage.