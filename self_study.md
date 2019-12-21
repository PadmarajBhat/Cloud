# Answer to My Random Questions :
* How do we give a domain name to to GCP App Engine ? 
  * Ans: https://www.mcafee.com/blogs/other-blogs/mcafee-labs/creating-custom-domain-name-google-app-engine-application/
    * app engine instance has direct option create a domain name

* Can we give a domain name to GCP Function or Run?
  * Ans : https://stackoverflow.com/questions/45850375/use-custom-domain-for-google-cloud-function
    * functions rely on firebase hosting for the same
    * cloud run seems to have direct option of domain name

* Can I try out gcp projects locally before I run in GCP?
  * Like if I want to run pubsub coding run locally, it is not possible
    ```
    # TODO: Create Topic Object to reference feedback topic

    topic_path = publisher.topic_path(project_id, 'feedback')
    ```
  * gcp project id is required to run so irrespective of the program running locally or on gcp cloud shell, we would be needing gcp account.
  
* Why do we need Kubernetes when we have app engine?
  * app engine is for auto scaling single service
    * yaml file created for deployment indicates environment (variables) for the single app
      * like environment variable
      * run time configuration : python version
      * entry point : how do you run the app
  * Kubernetes has also the auto scaling functionality but that is meant for multiple micro services.
     * you are focussed on scaling all or at least one micro services
     * you need to explicitly create images using dockerfile 
     * kubernetes yaml file will use those images and deploy the service 

* how does the kubernetes differentiate deployment and service request
  * ```kind: Deployment``` : indicates the yaml file which has this has environment instruction
    * you would indicate that the instances are exposed to perticular port
  * ```kind: Service``` : indicates the yaml file which has this has load balancer exposing the services (usually front end application)
    * you would indicate load balancer to route the traffic to a server which has multiple instances running in the kubernetes cluster 
  
