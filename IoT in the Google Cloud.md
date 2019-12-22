# Internet of Things: Qwik Start
* Message Queue Telemetry Transport (MQTT) : is a protocol used for transferrring data between devices in the IOT.
* devices register itself in the centralized location IOT core in GCP. All the telemetry data is sent to the core which publishes the message recieved in a topic under pubsub.
* the lab focusses on creating a device registry and creating a public key for it.
  * intent is devices will use the key to device registry. Device registry is the first step, post which it can send the data to IoT Core.
* a node.js program acts as a device and uses the *pem*(key) to register and communicate a message to IoT Core.
* subscriber to pubsub reads the message to a topic and prints it on the screen
