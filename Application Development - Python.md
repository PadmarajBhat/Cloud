# App Dev: Setting up a Development Environment - Python
* The lab focusses on the installing the python and its supporting libraries to run a server which displays hello gcp
* it also demostrantes the google library on python
* now the gcloud environment was with project owner credentials and hence the google api could talk to the same project and get the vm instances list.
* it is with the intent to installation process and friendly gcp library intro the simple program was targetted. The requirement.txt had only 2 version  but it has to be understood that it is equivalent of installing any python pip based packages installation on debian os.
* would it be possible to access the list of devices in the other network where the server is running ?
    * again it depends on the user with which server is running. If the user is project owner then the resources including network is accissible to him.
    
# App Dev: Storing Application Data in Cloud Datastore - Python
* https://exploreflask.com/en/latest/organizing.html indicates the different compoonents of the flask. 
* folder structure of the app:
   * quiz project (folder)
      * run_server.py
         * has step to run the quiz app. i.e. it does nothing more than importing the flask app at quiz folder and run command *app.run* [a flask run command]
      * requirements.txt
         * as a common practice to store all the required python packages and its versions.
      * quiz folder (app)
         * has smaller apps constructing quiz app  
