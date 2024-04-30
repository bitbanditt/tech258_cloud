# Azure deployment via User Data

As devops engineers we want to automate all we can, and where possible. Once we manually execute the process, and we are confident it works, we then use scripts, the next step is to deploy our app without having to manually input our ssh keys. 

We do this when setting up our vms under the advanced tab. As in previous deployments remember to set up your database instance first

![ud_tab.png](images/ud_tab.png)

Then scroll down to user data, we tick the box and a larger box where we can paste our script appears.

![ud_script.png](images/ud_script.png)

The User data runs all commands in our script as sudo if we specify it or not.

When running/starting the app via user data we have to be aware of the file path it takes and the permissions needed to access the files for our app. Ideally we want to use a ```cd``` path the works no matter where we run the script from.
* * since we use ```gitclone``` in the script, with space repo after the url, it creates a folder called url and stores the app there. There are various ways to give us access to our app using recursively ```chmod -r``` for the app folder and everything inside permissions. ```chown -r``` to change owner of the folder and everything inside. Or using  ```sudo -E``` command.
*  We will use be using ```cd repo/app```. As no matter where we are, or which user, root or home we can ```cd``` into it.

User data runs only once, so make sure the script

You have to give the script time to run, in theory you don't have to ssh in. However, with the database instance there is no way to check the database is running but to ssh in. You can then check the log with ```nano /var/log/cloud-init-output.log``` to see what has been executed in nano. or ```cat /var/log/cloud-init-output.log``` to have it print to the terminal.

While the script is running you may get an agent not ready issue. This is just means the instance is not ready to be ssh'd into just yet. Either refresh, or click out to e.g connect and back to your instance overview incase it is a UI bug.

With your app instance you can check it runs via pasting the public IP 

### AMI image

#### First step

We ssh into our running virtual machine. To create a generalised image we use the command ```sudo waagent -deprovision+user```. This command is used to prepare a virtual machine for cloning. It retains the user data, but removes system-specific information, such as the SSH host keys and unique machine IDs.

Next we go back to the overview page of our virtual machine, and press capture. 

See all images, my images 

### Consider

* You have to be completely be confident in your script, harder to fix once you've run the user data.
* File paths will be different as it starts from root user. Make sure it can be cd path into (app folder can be in root) 
* * Create a script that runs when you ssh in or user data. App folder can be in different location but has to be able to cd into
* Double check everything, have the correct: AMI, subnets, ssh keys, network security groups selected. Things will not run correctly if they aren't configured properly.

## Azure clean up

when terminating an instance you need to manually tick and remove, IP, Disk, and NIC