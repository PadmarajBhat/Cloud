# Google Cloud Run Serverless Workshop Notes:
* The architecture picture sumarized view:
    * a file or a set of files at real time are loaded to bucket
    * this triggers a pubsub notification topic
    * a subscriber listening to it triggers the conversion (doc to pdf) program (written in node.js)
    * the output is stored back into to different bucket.

* The above case can be deployed in a server based architecture where a dedicated server does run the conversion program. However, in this workshop it is deployed as "cloud run" driven app. Now, Cloud run is categorized as "compute" technique in GCP. This means it is more into IaaS. However, it actually falls in between PaaS and SaaS.
   * In the lab, you first create an app which does the conversion
   * you automate the steps to build and package it. Here, Node.JS (and hence npm) is used for it.
   * though the app built through npm which takes care of the other packages required for it in the package creation, the thrid party application should be there (duh, for invocation of its service). Here libreOffice is the tool that actually converts the doc to pdf. The node js app is just program which invokes the libreoffice capability to conver the doc to pdf through libreOffice exposed packages.
      * Nodejs app --> libre office --> conversion.
   For this we need libre office to be installed in the server (serverless server / dynamically created server) Now libreoffice is not supplied through npm and hence the instruction to install the software has to be outside the package build. This is mentioned in *Dockerfile*
   * Dockerfile: encompasses all the instruction that are required as required for application program. The order of the instruction is also important. The instruction to run the application program must also be present. Now the idea here is to create an image with the 3rd party applications and client driven business applications to one image. This is kind of install and keep a copy of all that is required and then use it during bringing up a physical server like loading laptop with an OS in DVD. Note that since the image already has the required software, when a new server is launched it is not going to install as per the docker file instructions. it is just raw write of image to bootable disk. Dockerfile has the section in the file called as "CMD" which does the server start. Thus if the image is present it just does the running those "CMD".
   
