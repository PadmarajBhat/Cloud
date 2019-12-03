# Google Cloud Run Serverless Workshop Notes:
* The architecture picture sumarized view:
    * a file or a set of files at real time are loaded to bucket
    * this triggers a pubsub notification topic
    * a subscriber listening to it triggers the conversion (doc to pdf) program (written in node.js)
    * the output is stored back into to different bucket.
