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

# Building an IoT Analytics Pipeline on Google Cloud Platform
* same focus as that of last but this time querying the sensor data through big query
* dataflow template is changed to route the sensor data to big query table.
  * empty big query table is created with 3 columns (timestamp, device, temperature)
  
