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
  * Cloud IAM
  * ACLs
  * signed url
  * signed policy document
