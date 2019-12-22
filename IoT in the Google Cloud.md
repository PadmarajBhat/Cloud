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
