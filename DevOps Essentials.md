# Notes on Featured Learning -1 : DevOps essentials

### Managing Deployments Using Kubernetes Engine
* The quest first focusses on the kubernetes because CI/CD requires automated orchestrator.
* The lab program covers following :
    * Continuous Deployment : stacking the versions on the older
    * Canary Deployment : splits the traffic between 2 versions. 
         * sessionAffinity: enables us to route the same user get the same version. When you have a split there will be bunch of users getting the newer version but if they flip off and on i.e. if the revisit the website then it is not guaranteed (with naive canary deployment) that same user gets the newer version again. To get this consistency, under ```spec:``` set ```sessionAffinity: ClientIP```.
         
    * Blue-Green Deployment : creates primary and back up with equal strength of the server for faster rollback. Here transaction is routed to Blue if the green set is has issues and has to be rollback. Cost is more as equal strength of resources has to be maintained.
         * Below 2 files are identical but just varies only version difference. Note that these are yaml file for service. These files enabling toggle between 2 services( for 2 versions).
               * https://github.com/googlecodelabs/orchestrate-with-kubernetes/blob/master/kubernetes/services/hello-blue.yaml
                     * This points to v1
               * https://github.com/googlecodelabs/orchestrate-with-kubernetes/blob/master/kubernetes/services/hello-green.yaml
                     * Here the service reference (points to) v2.
                     
               
### Deploy Kubernetes Load Balancer Service with Terraform
* there are 2 terraforms used in the lab
   * both have 
      * provider: A provider is responsible for understanding API interactions and exposing resources. https://www.terraform.io/docs/providers/kubernetes/index.html
      * resource: Each resource block indicates the infrastructural components like instance group , DNS, virtual network etc
         * https://www.terraform.io/docs/configuration/resources.html
      * output:   https://www.terraform.io/docs/commands/output.html
         ```
         $terraform output
         cluster_name = tf-gke-k8s
         cluster_region = us-west1
         cluster_zone = us-west1-b
         load-balancer-ip = 35.203.128.46
         network = https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-00-bda415ee9725/global/networks/tf-gke-k8s
         subnetwork_name = tf-gke-k8s
         $ terraform output cluster_name
         tf-gke-k8s
         ```
       * variable: which can be used within the scope of tf file.
       * data : to operate on variables and then save the output on a variable.
       
# Site Reliability Troubleshooting with Stackdriver APM
* skaffold : manages the workflow for kubernetes application and functionality for testing locally. This enables us to test code and ( kubernetes yaml + Dockerfile + project descriptor file) in local machine.
* SLI focus : 
   * request latency : time take for the recieve the response to a request
   * error rate: fractional request recieved
   * system throughput: requests / second
   * durability : likelihood that data will be retained for a long period of time.
