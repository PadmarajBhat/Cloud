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
      * external : ip addresses can be taken out of pool making it ephermal or it can be reserved ip address making it static
      * cloud DNS : helps to host and map dns mappings. It has 100% availability.
  
  
  * Carefully go through the output:
   ```
   ping mynet-eu-vm
   PING mynet-eu-vm.c.qwiklabs-gcp-00-5ade1a4a95e9.internal (10.132.0.2) 56(84) bytes of data.

   64 bytes from mynet-eu-vm.c.qwiklabs-gcp-00-5ade1a4a95e9.internal (10.132.0.2): icmp_seq=1 ttl=64 time=109 ms
   64 bytes from mynet-eu-vm.c.qwiklabs-gcp-00-5ade1a4a95e9.internal (10.132.0.2): icmp_seq=2 ttl=64 time=108 ms
   64 bytes from mynet-eu-vm.c.qwiklabs-gcp-00-5ade1a4a95e9.internal (10.132.0.2): icmp_seq=3 ttl=64 time=108 ms
   64 bytes from mynet-eu-vm.c.qwiklabs-gcp-00-5ade1a4a95e9.internal (10.132.0.2): icmp_seq=4 ttl=64 time=108 ms
   64 bytes from mynet-eu-vm.c.qwiklabs-gcp-00-5ade1a4a95e9.internal (10.132.0.2): icmp_seq=5 ttl=64 time=108 ms
   64 bytes from mynet-eu-vm.c.qwiklabs-gcp-00-5ade1a4a95e9.internal (10.132.0.2): icmp_seq=6 ttl=64 time=108 ms
   ^C
   --- mynet-eu-vm.c.qwiklabs-gcp-00-5ade1a4a95e9.internal ping statistics ---
   6 packets transmitted, 6 received, 0% packet loss, time 5007ms
   rtt min/avg/max/mdev = 108.750/108.978/109.969/0.443 ms
   student-00-fa272d900394@mynet-us-vm:~$ ping managementnet-us-vm
   ping: managementnet-us-vm: Name or service not known
   student-00-fa272d900394@mynet-us-vm:~$ 
   student-00-fa272d900394@mynet-us-vm:~$ ping privatenet-us-vm
   ping: privatenet-us-vm: Name or service not known
   student-00-fa272d900394@mynet-us-vm:~$ 
   ```
         
      * The reason later two pings failed is because they belong to different network
      
    ```
    student_00_fa272d900394@cloudshell:~ (qwiklabs-gcp-00-5ade1a4a95e9)$ gcloud compute instances list --sort-by=ZONE
    NAME                 ZONE            MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP     STATUS
    mynet-eu-vm          europe-west1-c  f1-micro                   10.132.0.2   35.187.105.103  RUNNING
    managementnet-us-vm  us-central1-c   f1-micro                   10.130.0.2   34.69.243.14    RUNNING
    mynet-us-vm          us-central1-c   f1-micro                   10.128.0.2   35.188.90.55    RUNNING
    privatenet-us-vm     us-central1-c   f1-micro                   172.16.0.2   34.68.7.184     RUNNING
    ```
      * To make it little confusing:
   ```
   student-00-fa272d900394@mynet-us-vm:~$ ping 34.69.243.14
   PING 34.69.243.14 (34.69.243.14) 56(84) bytes of data.
   64 bytes from 34.69.243.14: icmp_seq=1 ttl=62 time=1.56 ms
   64 bytes from 34.69.243.14: icmp_seq=2 ttl=62 time=1.78 ms
   64 bytes from 34.69.243.14: icmp_seq=3 ttl=62 time=1.89 ms
   ^C
   --- 34.69.243.14 ping statistics ---
   3 packets transmitted, 3 received, 0% packet loss, time 2003ms
   rtt min/avg/max/mdev = 1.561/1.744/1.892/0.145 ms
   student-00-fa272d900394@mynet-us-vm:~$ ping 35.187.105.103
   PING 35.187.105.103 (35.187.105.103) 56(84) bytes of data.
   64 bytes from 35.187.105.103: icmp_seq=1 ttl=56 time=106 ms
   64 bytes from 35.187.105.103: icmp_seq=2 ttl=56 time=106 ms
   64 bytes from 35.187.105.103: icmp_seq=3 ttl=56 time=106 ms
   64 bytes from 35.187.105.103: icmp_seq=4 ttl=56 time=106 ms
   64 bytes from 35.187.105.103: icmp_seq=5 ttl=56 time=106 ms
   64 bytes from 35.187.105.103: icmp_seq=6 ttl=56 time=106 ms
   64 bytes from 35.187.105.103: icmp_seq=7 ttl=56 time=106 ms
   64 bytes from 35.187.105.103: icmp_seq=8 ttl=56 time=106 ms
   ^C
   --- 35.187.105.103 ping statistics ---
   8 packets transmitted, 8 received, 0% packet loss, time 7011ms
   rtt min/avg/max/mdev = 106.242/106.393/106.564/0.195 ms
   student-00-fa272d900394@mynet-us-vm:~$ ping 35.188.90.55
   PING 35.188.90.55 (35.188.90.55) 56(84) bytes of data.
   64 bytes from 35.188.90.55: icmp_seq=1 ttl=76 time=0.690 ms
   64 bytes from 35.188.90.55: icmp_seq=2 ttl=76 time=0.269 ms
   64 bytes from 35.188.90.55: icmp_seq=3 ttl=76 time=0.209 ms
   ^C
   --- 35.188.90.55 ping statistics ---
   3 packets transmitted, 3 received, 0% packet loss, time 2038ms
   rtt min/avg/max/mdev = 0.209/0.389/0.690/0.214 ms
   student-00-fa272d900394@mynet-us-vm:~$ ping 34.68.7.184
   PING 34.68.7.184 (34.68.7.184) 56(84) bytes of data.
   64 bytes from 34.68.7.184: icmp_seq=1 ttl=62 time=1.63 ms
   64 bytes from 34.68.7.184: icmp_seq=2 ttl=62 time=1.75 ms
   64 bytes from 34.68.7.184: icmp_seq=3 ttl=62 time=1.59 ms
   ^C
   --- 34.68.7.184 ping statistics ---
   3 packets transmitted, 3 received, 0% packet loss, time 2003ms
   rtt min/avg/max/mdev = 1.597/1.664/1.758/0.068 ms
   student-00-fa272d900394@mynet-us-vm:~$ 
   ```
     * The reason the ping worked fine when ip address was given is due to the fact that they allow icmp protocol
     * There cannot be host name resolution for a different network and hence earlier host name ping failed.
     
     * inter connectivity between the resource in the same network is exploited in bastion host concept.
        * here external ip is taken out and hence it is no longer access through internet even if the http connection is enabled while creating the VM instance where a web browser is hosted
        * no a parallel vm is created in the same network and has external ip. Connect to this bastion host through ssh and then connect to the first vm instance through ssh. Since both vm are in same network, ssh between vm and bastion vm is allowed.
        
* IAM : a person can be given only storage view access with which no other resources are available for viewing.

* Bucket can have following security features:
  * Cloud IAM: roles defining the actions
  * ACLs : user to action mapping
  * signed url : time restricted access
  * signed policy document: defines the items that allowed for signed url

# Elastic Cloud Infrastructure: Scaling and Automation
* External IP Addresses can be consolidatedly viewed at VPC -> external ip addresses
* In the VPN lab: ( VPN gateways + tunnels)
   * static ip address given to respective network of VMs. Now the network has internal, ephermal and static ip address !!!!
     *  Static ip address, kind of hides the external and internal ip address of VM instances
   * tunnel is created along with when VPN is created.
      * VPN indicates the remote network range
* HTTP Load balancing routes the traffic nearer to the user request region.
   * in the lab 2 instance groups were created at different group regions. both these groups were of same disk images. The disk image was customized to have the apache2 installed and start at boot configuration.
   * creating image does not mean that the services will be autostared but in this example it was configured to auto run. In custom application it should be startup script perhaps to load the environment variables and then start the respective applications in the same order.
   * instance group lets us autoscale instances based on cpu utilization or request per second. **ab** is the tool used to test the autoscale performance.
   * https load balancing can work on both ipv4 and ipv6. This ip is what being called to in the browser which actually encapsulates instance group from external work. does that mean it is a static ip ?
   * in the lab session us-west1-c zone was used and hence it was expected that first load would go to instances at us-central1 region( and hence its zones instances). what if the test host was at us-central1-c, would it route first transaction to us-centra1-c instance in the group ? can HTTP load balances at zone level ?
   
   * Internal routing between subnets spread across 2 zones.
     * 2 zones are selected for 2 subnetworks. Now the task is to route the request between them.
     * there are set of instances in each subnet which can scale upto 5 with the cpu utilization as trigger. In our manual testing case there were no additional instances created. 
        * is it good config?
            * it would only scale up only if n-2 request is still keeping the cpu busy. n being current request and n-1 would go to other instance group.
            * it might not be a good scaling condition for now but can always be good combination to flip arround by keeping the 2 region equally occupied.
     * we have configured 2 firewalls almost with same setting.
        * 1 firewall rule indicates it can recieve TCP/80 request to custom network from anywhere (0.0.0.0/0)
        * 2nd firewall indicates it can recieve TCP/any port  request to custom network from 2 instances ip range.
            * isnt it that 2nd firewall is the subset of 1st ? Later in the same lab we create health check with tcp/80 to configure load balancer.
            * can we see firewall invocation ? as in to see if the http request got approved through which firewall?
              * I suspect that since we have 2 firewalls  
                  * 2nd firewall may be invoked when there is health check from those ips.
              * if there were only 1 firewall then it would have been invoked for all the health checks. 
                * would this mean that health check requests will be queued along with other requests in the firewall ?
                
         * health check enables the load balancer to determine if one zone is slow in response or not. However, in our case, CPU should not be busy and health check should always succeed. 
            * Then why does the load balancer toggle between 2 instance groups ? 
            * If there was one more instance group, would load balancer choose round robin fashion to route?
            
            
# Reliable Cloud Infrastructure: Design and Process
* Stateless micro services are the best design
  * can easily migratable and scalable.
  * avoids the issues with managing states
  * can be easily integrated with other application as they are usually reusable components.
