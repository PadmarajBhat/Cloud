##### GCP Core Infrastructure:
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
 * UI for Datastore gives datasheet like filter options to query.
 * GQL gives SQL like syntax to access the data 
    * SELECT * FROM President 
      * to fetch all entities from President kind.
 
