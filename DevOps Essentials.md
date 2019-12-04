# Notes on Featured Learning -1 : DevOps essentials

### Managing Deployments Using Kubernetes Engine
* The quest first focusses on the kubernetes because CI/CD requires automated orchestrator.
* The lab program covers following :
    * Continuous Deployment : stacking the versions on the older
    * Canary Deployment : splits the traffic between 2 versions. 
    * Blue-Green Deployment : creates primary and back up with equal strength of the server for faster rollback. Here transaction is routed to Blue if the green set is has issues and has to be rollback. Cost is more as equal strength of resources has to be maintained.
