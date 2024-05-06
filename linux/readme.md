# Linux commands 

### Basics 

```-- help``` is used after a command to list different applications and variations for a command. `

```-- version``` is an alternative method of the help command

```cd``` is change directory/folder and used to move around in folders. A directory name must be specified e.g ```cd images```
* ```cd``` with no following path takes you back to the home folder and is used if in numerous folders and sub folders
* ```cd ..``` moves back one directory/folder

```mkdir``` creates a directory 

```touch``` creates blank file. you must give a file name and type e.g ```touch fruits.txt```

```nano```  opens a blank file and lets you edit if a file name and type is given e.g ```nano script.sh```

```cat``` outputs the details of a specified object

```ping``` send packets (traffic) to a specified destination (IP)

```traceroute``` shows the route of the packets/traffic

```sudo``` gives the ability to execute a command with as a superuser (highest permission). Some commands are only executable as a superuser.



* #### sudo variations 

```sudo apt update -y``` this checks to make sure the system has the latest packages available. This is a safe command. The (-y) is to say yes to all questions asked by system.
```sudo apt upgrade -y``` this installs the updated versions of the newly updated packages. This command can be dangerous as it can cause clashes as new packages may conflict.

```sudo apt install``` intsalls a package when named. 

```sudo su``` makes current user a super or root user. Use exit to quit. (Only be a root user if necessary)

There are many sudo commands these are some of the most common. Two of the commands are used on boot up, with update is first and upgrade after. 


### Checks and information

```whoami``` tells you which user you are

```pwd ``` shows directory or folder currently in

```ls``` prints/lists all files and folders. can be used when in specific directories to check their contents

```ls -a``` lists all folders & includes hidden folders (-a = all)

```ll``` gives added information on listed files and folders

```file``` outputs details on a specified file e.g ```file peach.jpg``` 

```uname``` tells you the OS you are currently using
 
* ##### uname variations 

```uname -p```  gives information on the processor

```uname -n``` prints the private ip address

```uname -a``` prints all details regarding current system

### Checks continued and information 

```cat```  outputs all contents of a folder or file

```cat /etc/shells``` given a folder path, all contents of that folder is outputted

```history``` lists all used commands. Up to 500 are stored before deletion of commands as new ones are inputted

```history -c``` clears all commands. Useful for removing sensitive information that has been entered like passwords. Assisting in security and privacy
 
 ```ps ``` prints currently running processes

```ps -p $$``` prints information on a specific process (-p) in the example the shell ($$)

```ps aux``` gives a snapshot of current processes

```jobs -l``` lists processes

```top``` lists all current processes with updates in a specific order
* ```top -M``` lists process by which uses the most memory
* ```top -N``` lists processes by newest first
* ```top -P``` list by which takes up the most cpu

```kill```sends a signal to kill a process at default signal level (15) which ends processes gracefully.  You must give the process id. -9 is a brute force kill.
* ending a process gracefully tries to end child processes first

```kill -1``` lists all kill signals

### More basic commands

```clear``` clears the screen 

```curl``` gives the ability to download from the internet 

```mv ``` moves files or can be used to rename

```cp``` copies files  
 
```rm``` removes a file or directory giving its name

```rm -r``` removes a directory and all the files within it 

```rm -rf``` is to forcefully remove even if files or directory are write protected, and skips prompts. (This is a dangerous command) 

```tree``` outputs directory information in a more visually clear manner. Must first be installed.


```head -2``` prints the first two lines in text file (number of lines can be changed by change number after - ) eg. ```head -5 file.txt```

```tail -2``` prints last two lines in text file

```grep``` does a search for a specified entry or pattern e.g ```grep "chicken" chicken-joke.txt```

```sleep``` causes a delay giving a number 


cd /
  
  
cd
cd root
sudo su
 


   27  chmod +x provision.sh

sudo chmod 777 provision.sh
./provision.sh
systemctl status nginx
sudo apt install nginx -y
systemctl status nginx
systemctl stop nginx