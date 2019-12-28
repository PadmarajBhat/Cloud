# Internet of Things: Qwik Start
* Message Queue Telemetry Transport (MQTT) : is a protocol used for transferrring data between devices in the IOT.
* devices register itself in the centralized location IOT core in GCP. All the telemetry data is sent to the core which publishes the message recieved in a topic under pubsub.
* the lab focusses on creating a device registry and creating a public key for it.
  * intent is devices will use the key to device registry. Device registry is the first step, post which it can send the data to IoT Core.
* a node.js program acts as a device and uses the *pem*(key) to register and communicate a message to IoT Core.
* subscriber to pubsub reads the message to a topic and prints it on the screen
* to decode the node.js code:
  * there are 3 activities the devices are emulated to perform through node.js code
    * mqttDeviceDemo : mqtt device connection to IoT core
    * sendDataFromBoundDevice : to send data from the device
      * the *publish* steps are:
         * const mqtt = require('mqtt');
         * client = mqtt.connect(connectionArgs); : here connectionArgs are passed during the call of the function *sendDataFromBoundDevice*.
         * client.publish(mqttTopic,....) : here a publish is done to a topic 
    * listenForConfigMessages : to listen to the config msgs from IoT core
    * listenForErrorMessages : to listen to the error msg from IoT Core
    * there are 2 topics or *messageType*:
      * event : for connection related
      * state : for message transmission
         * this will enable respective subscriber for listening activity
         * schedulePublishDelayMs : determines the timeout of the message write
            * 1000 milli second for connection 
            * 2000 milli second for send messge 

# Streaming IoT Data to Google Cloud Storage
* intent of the lab is to route the incoming sensor messages to a file in cloud storage. There are 2 simulated sensor (programs) which periodically send the the temperature at 2 locations. These temperature info along with timestamp is saved in the cloud storage bucket files.
   * why 2 files got created ?  
    * one window file : sensor data collected over 5 minutes window
    * another is consolidated : aggregated sensor data for 2 devices configured under IoT registry.
* architecture:
  * 2 devices registers to IoT core
  * IoT core publishes the inbound messages from the devices to pubsub topic
  * Dataflow job subscribes to the topic and loads the messages to a file in storage at periodically ( 5 min )
    * note that not even a single line of code is written.
      * there are job templates like  ```Cloud PubSub to Text Files on Cloud Storage``` which not only has the pipeline required for the actions ( name explains it all) but also has the codes to do the end point actions like reading/writing etc.
      * job creation takes topic as the input and also the bucket where the file has to created and updated.
      * note that file write in batch fashion: every 5 minutes. i.e. subscriber reads and forward to the next stage in the pipeline. The writer simply accumulates the messages for 5 minutes and flushes to file at once. This not only reduces frequent write operation and also makes the *window* safer for delayed message.
* note that 2 devices had same rsa certification created from the user id which is different from that of gcp project owner.
 * RSA certification is got through 
   ```openssl req -x509 -newkey rsa:2048 -keyout rsa_private.pem -nodes -out rsa_cert.pem -subj "/CN=unused"```
# Streaming IoT Core Data to Dataprep
* The lab focusses on extending last lab work, first it creates the cloud storage file from the sensor data. Dataprep loads this data and perform data transformation.
* transformation targetted in dataprep are:
  * renaming column
  * rounding the temperation to 2 decimal digits
  * hourly scheduling these 2 activities for the new data arrived.
  * these data are stored back to cloud storage for further processing (may be ml engines)
  * in dataprep, when column based transformation are stored as reciepe items and then shows the outcome of transformation immediately
  * **Run Job** actually runs the reciepe on the all the data indicated in the import data. You also have an option of randomly selecting data.

# Building an IoT Analytics Pipeline on Google Cloud Platform
* same focus as that of last but this time querying the sensor data through big query
* dataflow template is changed to route the sensor data to big query table.
  * empty big query table is created with 3 columns (timestamp, device, temperature)
  
# APIs Explorer: PubSub and IoT
* same focus as that of last lab but this time a server munches the data and give a *response* back to update the device.
* so far it has been one way, i.e. telemetric data was injested and analysed but nothing went back to the device but this lab focusses on updating the device
* for the use case sake, an ac sensor sends the temperature data and it is turned on or off based on the current temperature.
* *modifyCloudToDeviceConfig* : is used to send back the update to the device. It it said that it is built on top of *httplib2*. IoT core is configured to take both MQQT and HTTP requests.
  * as indicated in https://cloud.google.com/iot/docs/reference/cloudiot/rest/v1/projects.locations.registries.devices/modifyCloudToDeviceConfig; this function writes it back to cloud iot core which later translates into respective device.
  * inward flow : device --> cloud iot core --> pubsub publishing
  * outward flow : pubsub subscription --> data analysis and decision --> cloud iot core --> device
* subscriber indicates: how long the message has to be present in pubsub that a publisher has provided !!!!
  * in google console,  we can see the subscribers logs and health checks
* interesting read : https://cloud.google.com/iot/docs/reference/cloudiot/rest/v1/projects.locations.registries.devices.configVersions#DeviceConfig
  * if the device is disconnected, there may be multiple version of device config update but when it comes online, latest is sent 

# Using Stackdriver Logging with IoT Core Device
* lab focus remains the same, configure a IoT core registry to run on a perticular region and route all the devices messages that would come to a pubsub topic. Register 2 devices under it (IoT Core). 
* those 2 devices are programmed to send telemetry data and recieve device configuration over MQTT protocol.
* now here in this lab telemetry data is read from the pubsub topic and written into stackdriver logging. 
  * cloud function is configured to get triggered on **publishing** on pubsub topic. 
     * if there are multiple entries to pubsub concurrently, will the function scale and run in parallel to make update to stack logging?
           * **Ans**: Yes, a clound function run has only activities as follows:
             * read the inbound message (that was recieved at pubsub and passed to function through trigger)
             * format the data as required for the stack driver logging
             * push the data to logger.
           * Therefore, multiple triggers will be raised when there are multiple entries to pubsub each resulting in multiple parallel cloud function execution like threads but not exactly threads. 
* when there are multiple subscriber when all subscriber have acknowledged the topic message acknowledgement, then only the message is deleted from the pubsub repo.
* the messages to subscriber are the only ones which have come to pubsub post **subscripiton**.
* as indicated in https://cloud.google.com/pubsub/docs/subscriber:
   * pull or push are the 2 ways with which subscriber can get message
     * in case of push : subscriber can optionally change the detention period and then acknowledge the message recieved
     * in case of pull : subscriber has to initiate the pull request and post delivery of the message, should acknowledge the same.

# Using Firestore with Cloud IoT Core for Device Configuration
* the lab focusses on the configuration update through both CBOR and plain text format. And also introduces to firestore database.
  * CBOR : Concise Binary Object Representation : focusses on balancing the size the explainabilit of the data in binary format. Devices use internet and the CBOR format helps it condense it. 
  * Firestore: a fast no sql db : https://db-engines.com/en/system/Google+Cloud+Bigtable%3BGoogle+Cloud+Firestore as indicated in the link it is newer option and auto scales.
       * https://firebase.google.com/docs/firestore/query-data/queries: indicates queries are more or less similary to mongoDB
       
* https://firebase.google.com/products/: firebase provides wide range of products for app development focussed on either web or mobile. Since Google has acquired it on 2014, it has seemless access to gcp resources. More seemlessly integrated to js based. Guess node.js knowledge is a must. 

# Streaming IoT Kafka to Google Cloud Pub/Sub
* the lab focusses on the interconnect between kafka and pubbsub. Kafka is a popular streaming platform and integrating with it would enables access the external iot devices telemetry data
