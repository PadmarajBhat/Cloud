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
                     
               
