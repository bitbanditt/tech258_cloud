# Monitoring and alert management

### CPU Overload

If we have a vm hosting our app, and the cpu load becomes too high, it is extremely likely that vm hosting our app will go down if no one is watching it. See the diagram below explaining the worst to best in monitoring and responding to cpu load/traffic.



* If there is no monitoring our vm will go down.

* We can set up a dashboard so that we can see when the load gets too high, but that requires someone to be watching it. Meaning the cpu overload can be missed, and the same problem persists.

* The next best option is to set up an alarms so that when a threshold we set is reached, we will receive a notification, even to our phones. However, this like the other options before, requiring human intervention to come and fix. We need something better, and automatic.

* The automatic response is auto-scaling. It is an automatic response. Similar to when a fire alarm goes off and the sprinkler system also goes off and releases water. When the threshold we set is reached it auto-scales out more vms.

### Auto scaling 

Auto-scaling can be done out or in, and up or down.
* Out or in, refers to the addition or reduction of virtual machines as needed to meet demands.
* Up or down, refers to the switching of work load from one virtual machine to another with a bigger or smaller CPU depending on the demand.

### Creating a dashboard 

We create a dashboard so we can have a visual check on our resources, allowing us to monitor them easier. 

### Setting up an alert and action group



### Load testing the vm/app

 

ssh in and run sudo apt-get install apache2-utils