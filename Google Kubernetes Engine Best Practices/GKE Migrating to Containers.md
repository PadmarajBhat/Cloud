# GKE Migrating to Containers Notes
* https://cloud.google.com/container-optimized-os/ : Open Source OS based on Chromium OS for kubernetes engine.
* In the lab session, there are 3 enviornment created:
    * VM session on Debian OS
      * number of instances of a sample server (factorial server) = 2
    * VM on COS
      * number of instances through docker deployement  = 3
      * this instance does not mean that it would auto scale, it would mean that there are security and docker/container libraries are available for the apps.
    * Kubernetes cluster
      * number of instances, initially 1 but later 3
      * ease of scaling is the highlight of the lab. Later may be in the future we will also see how it can also auto scale basis of request/sec or similar threshold value.
* In the non kubernetes cases, we would need a load balancer and fire wall set up to scale the server processes. However, kubernetes engine does that all for us.
