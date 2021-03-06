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
   
* Alert policy is created then we can see the incidents where the alert policy condition is met. When we open the incident, you have an option to *acknowledge* the incident so that further escalation would not occur. This may be handy in collecting enough evidences on the issue and stopping it post that. Incidents are auto resolved when threshold does not reach for the observed period.

* Profiler : gives the flame graph of randomly sampled snapshots
   * x axis is the consumed metric
   * y axis indicates the parent child relationship of the functions.


# Continuous Delivery Pipelines with Spinnaker and Kubernetes Engine
* Now the lab could not be worked upon because there was a missing step; However, there are some lessons to be noted.
* Before we dive in there is an alternate lab for jenkins then what is the difference between jenkins and spinnaker
   * https://www.spinnakersummit.com/blog/spinnaker-vs-jenkins-for-continuous-delivery. Note that the post is an year old not sure if it is still true. Key points:
      * Spinnaker is focussed on cloud deployment where jenkins is not
      * Spinnaker does have dependency on jenkins( or alike) for build server capability. Hence,  either of them cannot replace other.
* So what about spinnaker vs kubernetes:
   : https://www.spinnakersummit.com/blog/why-spinnaker-and-kubernetes-work-better-together-for-continuous-delivery
      * Here also it is said that both work together better
      * there are easier monitoring and rollback scripts available.
      * Kubernetes was dependent on Jenkins for custom builds however spinnaker uses the cloud resources effectively.
      * spinnaker has something called as **Immutable Infrastructure** which manages the versioning of all the configuration files so the things do not break during roll back. Thus claims safer deployment through spinnaker.        
* In the lab **Helm** is used to deploy spinnaker for kubernetes. Helm is a package manager for kubernetes application. 
   * https://hackernoon.com/what-is-helm-and-why-you-should-love-it-74bf3d0aafc indicates clearly why would we need helm.
         * when we need to have large set of micro services scaling indivisual service may be cumbersome and helm using chart. 
   * https://boxboat.com/2018/09/19/helm-and-kubernetes-deployments/
      * A Helm chart is simply a collection of YAML template files organized into a specific directory structure. 
      ```
      ~>tree demo-chart/
         demo-chart/
         ├── Chart.yaml
         ├── templates
         │   ├── deployment.yaml
         │   ├── rbac.yaml
         │   └── service.yaml
         └── values.yaml
      ```
# Continuous Delivery with Jenkins in Kubernetes Engine
* Self studying on the jenkins on k8s because git repository used in lab indicates that build with error 
* Went through the video : https://www.youtube.com/watch?v=dUr1s9qTQlk
   * this focusses on creatin gthe Jenkinsfile which has steps to create docker image file 
      * few syntax of Jenkins file syntax which are either python or javascript friendly were noted.
      
* went through https://github.com/GoogleCloudPlatform/continuous-deployment-on-kubernetes (git repository used in qwiklabs)
   * architecture indicates how the external user interacts with jenkins
      * cluster does not only have the kubernetes engine but also the jenkins software installed in them
      * there is jenkins master and executor pods in the each of kubernetes node.
      * need to go through https://cloud.google.com/solutions/jenkins-on-kubernetes-engine in detail.
         * in search of answer to the questions:
            * where does the jenkins discover services run ?
            * where does the jenkins UI run ?
            * what is jenkins persistent disk ? where does it decide ? does it require dedicated distinct compute engine ?
            
         * K8s provide a ephemeral build executor which helps in clean build of deployment ( this includes code repository pull and containerization)
         * https://www.youtube.com/watch?v=IDoRWieTcMc video summarizes lot of what the lab does.
            * Jenkins for CI and spinnaker for CD
            * Helm for package manager for k8s
            * Jenkins pipeline for deployment : 
               * build image
               * create new deployment (changing the yaml file for the change in the version)
               * verify deployment (check for *sustained* healthy pods)
               * switch to new deployment (it can be blue-green or rolling update or canary)
               * verify the new deployment
               * delete the old deployment
         * K8s default deployment is rolling update where old pods will be automatically delete when newer ones are installed.
            * maxSurge: what is the maximum number of pods to be in the system.
            * maxUnavailable: what is the max number of deletion of pods when deleting it.
            * terminationGracePeriodSeconds: indicates the duration to which it has to wait for the deletion of pods for graceful completion of executing request. A signal is sent to an app which actually needs to be handled by application for closure of connection and wrapping up activities
         * readiness check and health check make the deployment healthy:
            * readiness need not be limited to response to dummy http request, it can be check to availability of the data / db which might be required for the successful response.
