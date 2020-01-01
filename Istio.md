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
