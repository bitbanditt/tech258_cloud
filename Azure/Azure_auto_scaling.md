# Auto scaling

### What is high availability and scalability

* *High availability:* is the ability to have our instances hosting our app available to users with little to no downtime (as  much as possible).

* *Scalability:* is the ability to produce more or less instances as is necessary. 

### High availability and scalability using Azure virtual machine scale set


*How is this achieved?* By using a vm scale set we are able to set up multiple virtual machines by default (known as "initial") and never have less than the initial set. This means that our app is always available as it is launched with multiple vms. 

* So, in case one goes down there is another available. And in response to a policy set e.g CPU Load we can add an extra vm. The scale set will take an average of CPU load of all healthy vms in use and if above the set threshold it will scale up (add a vm).

This makes scalability possible as we can increase or decrease vms as necessary. And  is  possible as we always have a machine with our app deployed.

If we host our vm with app in one availability zone, if it goes down so does our app. Even if we deploy multiples vms, if they are in the same Availability Zone, they will all go down.

By having our vms in multiple availability zones, if an occurance was to happen that brings it down (e.g. power cut, fire, natural disaster), we have vms in other availability zones which are unaffected. This means our app is available even if unforseen circumstances occur we have a layer of protection.

(Can use custom image to create vm or use vm to create a custom image

We have options for our starting image

Virtual Machine scale set)

We set a policy (custom auto scale), when will it scale? parameters, average cpu load, minimum vms (2), max vms
* * minimum set to 2 so we have a back up in case one goes down, app always available. vms in same subnet but different Availablty zones. We are setting up high availabilty and scalability

vm scale set is responsible for creating virtual machines

need a load balancer (internet facing) directs traffic from outside to a vm dependent on policy set

Load balancer has front end IP (traffic is sent there not directly to vms so load balancer can do its job)

### Setting up a scale set

* #### Preparation of scale set

Before starting a scale set it is vital to test and make sure the User Data and AMI work, as that is what the scale set will be using to create the virtual machines. So make sure you have full confidence in you User Data, make sure they are both thoroughly test.

* * Keep in mind the Azure portal regularly changes so some tabs may move, some features may be named differently.

#### Creating the scale set

Now we are fully confident in our User Data and AMI we can begin creating the scale set. 

* First we search for the scale set creation, and choose Virtual machine scale sets.

![scale_set_search](images/scale_set_search.png)

* Next we click create after being taken to the virtual machine scale set page.

![vmsc_create](images/vmsc_create.png)

* Then we go through our basic settings. We want to make sure to select all Availability zones.

![VM_AZ](images/VM_AZ.png)



ssh in - we are given private IP, have to use IP of load balancer, which is IP where we see app

create unhealthy instance stop one and start (app and user data doesn't re-run so instance will be unhealthy)

### Unhealthy

while 
### Clean up

load balancer, new network security group, load balancer public IP, and scle set

networking, load balancer, frontend IP
delete scale group, then load balancer (as public IP is being used by it so must delete first before deleting public IP)