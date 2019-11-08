* SSH can be initiated from the google cloud shell and thus you can see gcloud/gsutil commands to the VM in the GCLOUD UI as and when it is executed in a single window.
* When you create the window based VM you connect through RDP !!!! perhaps the trick connect to google internet at very low cost :P
* Cloud Shell:
    * $HOME directory contents are retained even if the vm is closed or restarted and accessible to all the gcloud components.
    * ```gcloud beta interactive``` : helps to complete the command.
    * Debian based: 5gb storage
* Kubernetes Engine:
   * ```gcloud container clusters create [CLUSTER-NAME]``` : This creates a cluster with default 1 master and 2 workers for kubernetes engine. i.e. each node/ (master and workers) / VMs has the kubernetes engine installed in them.
   * ```gcloud container clusters get-credentials [CLUSTER-NAME]``` : will get the auth data of the cluster so that further communication can be established. i.e. to interact with the cluster. I guess all the auth data related data goes into shell environment and subsequent calls will fetch from it.
   * ```kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0``` : Now since cluster data are present with us, it is time to run kubectl. Note that here no cluster information is given. Here it just deploys the app in the nodes.
   * ```kubectl expose deployment hello-server --type=LoadBalancer --port 8080``` : Here those services (or server from app perspective) comes live and exposed through tcp port. ```kubectl get service``` shows the external ip. When you hit the external ip for the "service" [name is as determined by the app], kubernetes master takes the request and runs the same in one of the worker and the reply from the same is sent back. Here, hello-server is the kubernetes engine object name and service name is "hello-web". HTTP request hits the app service and not kubernetes object.
   
   * terminating is again through gcloud command which started the cluster ```gcloud container clusters delete [CLUSTER-NAME]```


* Load Balancer:
   * Network Load Balancing: Load Balances traffic among the VM instances in the same region in VPC network. So forwarding rule is created which gives us an ip which abstracts the vm ip addresses. Routing / Load balancing is the magic.
   * HTTPS Load Balancing: Best explained in the url [https://cloud.google.com/load-balancing/images/https-load-balancer-diagram.svg]
      * first define health check of vm instance to http
      * create a map of http service to port number
      * create a URL for map
      * direct a proxy to the url map
      * create a forwarding rule to the proxy
