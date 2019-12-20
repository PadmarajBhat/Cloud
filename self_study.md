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
