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

```file``` outputs the details of a file


```sudo``` gives the ability to execute a command with as a super user (highest permission). Some commands are only executable as a super user.

* #### sudo variations 





### Checks and information


```whoami``` tells you which user you are

```pwd ``` shows directory or folder currently in

```ls``` prints/lists all files and folders. can be used when in specific directories to check their contents

```ls -a``` lists all folders & includes hidden folders (-a = all)

```ll``` gives added information on listed files and folders

```file``` outputs details on a specified file e.g ```file peach.jpg``` 

```uname``` tells you the OS you are currently using
 
* #### uname variations 

```uname -p```  gives information on the processor

```uname -n``` prints the private ip address

```uname -a``` prints all details regarding current system

### Checks continued and information 

```cat```  outputs all contents of a folder

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

```kill```ends a process at default level 





clear

curl

mv 

cp 
 
rm cat


rm -r stuff
rm -rf funny


head -2 chicken-joke.txt prints the first two lines in text file (number of lines can be changed by change number after - )
tail -2 chicken-joke.txt prints last to lines in text file
cat -n chicken-joke.txt
grep "chicken" chicken-joke.txt
sudo apt update -y 
apt install tree
apt upgrade
tree 


cd /
  
  
cd
cd root
sudo su
 
nano provision.sh

   27  chmod +x provision.sh

sudo chmod 777 provision.sh
./provision.sh
systemctl status nginx
sudo apt install nginx -y
systemctl status nginx
systemctl stop nginx