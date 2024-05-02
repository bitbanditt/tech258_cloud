# Securing database with Azure

### Making database truly private

Hosting our database in an instance in our private subnet, does not make it secure. Even though we have network security groups (nsg) specifying the allowed ports, it still has a public IP and can be accessed (We don't want a way for our database to be accessed from the outside).

Furthermore, without any extra configurations, the communication inside our virtual network is too free and undirected. This means traffic is able to go to subnets or apps we may not want them to. 

There is also no filter to tell which traffic is safe or not.

To make the subnet truly private, and secure our database there are a few things to put in place. Stricter nsg rules, to only allow traffic from ssh and mongodb (database), and ignore everything else. 

We also need a way to filter traffic, and direct the safe traffic where it needs to go and reject the rest. Then finally, we need to remove the databases public IP, so it is inaccessible from outside our virtual network.

Having multiple layers of security like this is important as we don't want a *single point of failure*. Meaning if one measure goes down our whole architecture is no longer secure. Think of it like having multiple locks on your door, or a gate.

### DMZ subnet

To add some extra layers of security we create a Demilitarised zone subnet (DMZ), and set up a Network Virtual Appliance (NVA) virtual machine (VM) inside. The NVA acts as filter/firewall and only allows traffic thorough if it meets the rules we set.

In essence our NVA is held here.

* #### NVA (Network Virtual Appliance)

The NVA like other virtual instances has a Network Security Group (NSG) and a Network Interface Card (NIC). We can ssh into the NVA Virtual Machine (VM) to configure it.

The NVA makes sure only traffic from right source talks to e.g. the database subnet. It forwards the filtered traffic, making it follow a certain route, ensuring only the right database traffic gets through. (This is how it acts as a filter/firewall)  

Currently, any device in our virtual network can talk to each other (system route) via any path. While a user route says communication can only can go this way. Directing it to its next destination.

Using an NVA should not cause any delay in accessing the app database page

### Routing and Route tables

A *route* is a path traffic is allowed to take. Think of it like train tracks. The movement is restricted, and it can only reach the destination set.

A *route table*  enforces the path/route traffic has to make. This can make sure, for example, the next place (next hop) traffic goes is the NVA, so it can be filtered, and sure its meant for the right destination.

* nsg rules have priority lower 

  


### IP forwarding

IP tables rules

### First steps



