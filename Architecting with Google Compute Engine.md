# Essentials of Cloud Infrastructure: Fundamentals
* VPC: 1 GCP Project has 5 network by default ( which can be changed). These 5 networks can be shared across other projects.
  * It enables GCP resources to communicate and isolate with each other
  * There are 3 types of VPC network: Default, Auto and Custom
     * Default is Auto mode
     * Auto mode: 1 subnet per region and hence spans acrosses multiple zones ( region > zones )
         * Regiional IP allocation
         * Fixed /20 per region which can be expanded to /16
            * network > subnetwork > IP addresses
            
  * if resources are under same network, the resources can interact with each other with internal ip addresses even if the resources are in different region. On the contrary, if the resources are different network but are geographically in same region , both have to communicate with each other with external ip addresses. Here, the interation do not go out to internet but get routed through gcp edge routers (which has different pricing). 
  * every resources have 2 ip address: internal and external address.
      * internal : all the resources gets their ip from DHCP when they register with the internal DNS.
      * DNS can translate host and url names in the same network and not the different network.
  
         
  
