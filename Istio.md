# Istio Self Study notes
* Microservices in the Cloud with Kubernetes and Istio (Google I/O '18)
  * https://www.youtube.com/watch?v=gauOI0O9fRM
    * create a simple app to indicate time
    * dockerize it
    * save the image to google container registry
    * start a kubernetes cluster
    * scale the app upto 5 instances with only 4 vms.
    * finally Istio to manage the interconnection etween micro services.  

 * Istio:
   * He had the yaml file with which he issued command similart to kubectl
   * first he whitelisted a external website.
     * egress.yaml --> spec --> destination --> service
   * his nodejs code could act like a middle ware and backend based on the environment variable. This variable was set using kubernetes yaml file.
   * now with this he created 3 layers: frontend calling middleware calling backend. Backend gets the time and sends it back to middleware and then to front end from middleware.
   * he then demonstrates the failure (500 error) in the app and explains the cascading effect.
   * He later shows the http retry capabilities of the Istio to solve the 500 error. 
   * Tools that Istio Provides:
     * service graph: identifies the flow between the services. Note that not all services call other services.
     * middleware routing : to route the transaction to the newer version of a service which might be at the middle of the service flow to do the integration testing
       * you can also have weighted routing  like 80:20 etc.
     * Istio charts: which gives various metrics on the overall system like overall success rate etc.
     * stack driver logs
     
   * Istio is a layer on the top of kubernetes where api communication can be managed. The YAML input are executed through kubectl because **apiVersion** would include the Istio version.
   * Knative sits on the top of Istio and helps to build, deploy and manage serverless workloads.
   * Istio can also work between hybrid cloud environment.
   * Can also help in API management. 

* 4 imortant components of a dashboard:
 * traffic: both ingress and egress
 * error rate: out of those traffic how much is succussfull.
 * latency: how fast are we in executing. Note that it is fine drilling on where the time takes other wise egress traffic indicates more or less same info.
 * saturation: by when the application would explode if not scaled.

* There are 3 components in the Istio:
 * Pilot : 
 * Mixer
 * cidadel : for security
 
 * Understanding Kubernetes Pods and Services: https://thenewstack.io/kubernetes-way-part-one/
  * Services : acts as the common label for differnt pods
  * pods : containers at execution and can be scaled.
# Installing the Istio on GKE Add-On with Kubernetes Engine
* the lab focusses on creating kubernetes cluster with Istio add on
* istioctl installation
* deploying book review application on kubernetes cluster using ```istioctl kube-inject```.
* the above command do not installs app in the cluster but in the contrary it modifies the yaml file to include the ISTIO related proxy details. Now with the above command we have new yaml file suitable for deployment through Istio
* the deployment is done through kubectl apply command itself. Here it would take new yaml created by istioctl 
* samples/bookinfo/networking/bookinfo-gateway.yaml file defines the 
  * from where the ingress traffic can come (http/80)
  * virtual services which indicates which URI from user lands to the which routing page
  
# Installing Open Source Istio on Kubernetes Engine
* again the same application deployment but through Istio installing as software on kubernetes cluster.
