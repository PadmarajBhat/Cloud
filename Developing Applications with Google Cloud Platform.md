#### GCP Core Infrastructure:
* Resource Hierarchy:
  * Org -> Folder -> Resources
  * Policies at higher level are inherited to lower level.
  * Hence there is need to keep the generic/low restricted policies at top
  * Policies at lower level overrides the policies on higher one. So whats the use of top level policies ? It just provides the initial policies, creator at that level defines the concious policies.
  * Org node is mandatory only when folder is required.
  * Service account provide policies to resource instances. They use keys to authorize the resource usage. Other polices are to the roles and password driven.
  * Primitive roles affect all resources in a GCP project. Predefined roles apply to a particular service in a project. A answer that took 8 attempts :P
  
* compute engine:
  * 2 vm can interact with each other. i.e.
    * i can ping other vm from one
    * I can do ssh to access the other vm
    * you cannot ssh from gcloud to vm

* Storage:
 * Datastore is the miniature version of BigTable.
 * ACL configuration command for Bucket
   * ```gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png```
     * here public access is given to an image file
   * File is accessed through url.  

* SQL :
  * just like we have ACL and then we can access "Bucket" through url,  we have "add network" where we can specify the VM instance ip which would have access to SQL. Further SQL is accessed through ip address, user name and password. User name and password are usually configured soon after SQL creation. In bucket, you can merely refer to the url, however, in sql you have to make connection through allowed network and with user credentials.

* Containers: decouples the os in which app runs
* pod: contains one or more related containers
* kubernetes engine: executes the deployment, scaling, securing the pods
* kubernetes: manages the container cluster across multiple cloud provider
* Deployment manager is for infrastructure management for gcp resources
  * your changes to gcp resources can version controlled through version controlling the yaml with which you created the instance.
  * you can version control the yaml file in the cloud source repository.
  * this can be used to replicate the environment for a different project or prod/dev/test environment.
  * Note that there is version of the app/code here, it is just a infrastructure management.

* stackdriver can also intake print statement in the application

* Canary deployments are a pattern for rolling out releases to a subset of users or servers. The idea is to first deploy the change to a small subset of servers, test it, and then roll the change out to the rest of the servers. (wiki)

* post testing at development and testing env, jenkins can be used for CI and CID
 * Jenkins is a free and open source automation server. Jenkins helps to automate the non-human part of the software development process, with continuous integration and facilitating technical aspects of continuous delivery

* Datastore is no sql and is queried through GQL (Graph Query Language)
 * "table" which holds record is referred as "Kind"
 * It stores the record as entities of varied length
 * "columns" are called properties.
 * UI for Datastore gives datasheet like filter options to query.
 * GQL gives SQL like syntax to access the data 
    * SELECT * FROM President 
      * to fetch all entities from President kind.
 * So what is the difference between SQL and Datastore;
   * to query that has multiple properties should define composite index to support  multiple values for a property in the query.
     * ```SELECT * FROM President WHERE year < 2000``` : supported
     * ```SELECT * FROM President WHERE year < 2000 and lastName = "Bush" ``` : is not supported 
     
* From the Datastore Lab:
  * package.json file is required to execute ```npm install```
  * package.json should have an entry with key as "scripts" which should have 
    ```"scripts": {
                   "start": "node server/app.js"
                  },```
  * the way node js create Datastore connect : ```const ds = Datastore({projectId: config.get('GCLOUD_PROJECT')})```
  * entity and key declaration : ```const key = ds.key(kind);```
  * entity key and data declaration : ```const entity = {
   key,
// The entity's members are represented in a data property.
// This is an array where each element represents one
// member in the entity. Each element is an object with a // name and a value
   data: [
     { name: 'quiz', value: quiz },
     { name: 'author', value: author },
     { name: 'title', value: title },
     { name: 'answer1', value: answer1 },
     { name: 'answer2', value: answer2 },
     { name: 'answer3', value: answer3 },
     { name: 'answer4', value: answer4 },
     { name: 'correctAnswer', value: correctAnswer },
   ]
 };```
 
  *   to save the read input : ```return ds.save(entity);```
  * This would enable a web page to take the data and save it in kind with said key and data.

#### Security and Integration Components of Application
* IAM Roles:
  * Google account: identifies a user
  * Google Group: helps in applying policies for multiple google account. Has no email id of its own.
  * Service Account: associated with application rather to a indivisual
  * G suite domain: virtual group of all the indivisuals of an organization
  * Cloud identity domain: virtual group of all the indivisual of an organization but do not have G suite access.
  
  * Permission:  which application has what access on which resources
  * role: is collection of permission. User is given with a role so that set of permissions are allowed to him
      * Primitive : roles across project
      * Predefined : roles associated with specific resources
      
  * Service accounts can have keys which can help in authenticating resources for a resource. 
  * when a on behalf authentication has to be done, then OAuth is used. Here, user approves the request for a resource access and then post that resource is allocated.
  * IAP: Identify Aware Proxy: controls the user to cloud application access
  * Firebase authentication: is an intermediate layer which collects user credentials and authenticates the same with client (google or facebook) then return authentication token and basic info from client. This authentication token can be used for app driven verification.
  
  * why does pub/sub changes does not guarantee the order the messages delivered? what I think is
    * there are VMs to manage the pub sub operations. in the asyncronous communication, there might be chance of failure and back up restoring and more over, there may be difference in the queue due to round robin scheduling or wait due to autoscaling
    * solution to that is ,  windowing stream for processing. Handle late events through watermark(huerastic to measure late event).
    * as at least once is guaranteed, application has to take care of duplicate entries if any.
  * cloud functions are even driven. The code to execute can be from zip file in cloud storage. Thus when it autoscales take the standard location to get the code and performs the necessaty actions. This also enables us to configure cloud function perhaps gcloud commands. Any console statement goes to stackdriver and can be viewed with filters (at least timestamp filters). In case of subscribe, how it will scale ?

* Cloud Endpoints :  let you expose the api to the external world. This could have been done through cloud function too but here you have monitoring, versioning ,securing and easy deployment APIs
   * it supports both grpc and rest based api support
   * open API specification for rest apis and protocol buffer for grpc support
   * api functionality can run on vm, kubernetes cluster or app engine

#### App Deployment, Debugging, and Performance
* Continuous Integration: Automation of Code, Build, Deploy and Test
  * Build process gets triggered through the Jenkins which monitor the source repository. It uses container build tools to build the image and stores the artifacts in the container registry. When all the tests are passed, it is then feature branch is merged with master branch.
  * Spinnamer a deployment system installs the artifacts into cloud.
* Continuous Deployment: Automation of Code, Build, Deploy, Test, Release and Monitor
  * Continuous deployment is triggered when code gets checked into master. Where build process builds it and deploys it in the staging area. After all the integration testing, it is marked for release and then deployed in production. There after it can monitored and in case of issues, can be rolled back to previous version.
* gcloud comput scp : command lets you to copy the local file to vm instance
* ```require('@google-cloud/debug-agent').start(
     { allowExpressions: true });``` : puts the debugger indication in the code
* ```npm install --save @google-cloud/debug-agent``` : node js agent for stack driver debugger
  * you can open the repository in google shell.
      * either you have to clone the repository into google shell and then through code editor edit it.
      * go to the directory from which last git push was done in the google code editor and then start editing.
      * in either case you should be in directory where code resides.
* Similarly for error reporting:
  * ```npm install --save @google-cloud/error-reporting```
  * ```const ErrorReporting = require(
       '@google-cloud/error-reporting').ErrorReporting;```
* Debugging in GCP:
  
