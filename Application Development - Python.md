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
         * each app folder has **__init__.py** file in it; including the parent app *quiz*.
         * quiz app has 3 smaller apps constructing quiz app  
         * api app (folder)
            * focusses on creating the the end function which does the datastore fetch and also the evaluation based on user answers.
            * api.py
               * defines the functions i.e. to fetch and evaluate the quiz
            * router.py
               * defines the Blueprint for the api app which accepts the urls : *'/quizzes/<quiz_name>' && '/quizzes/feedback/<quiz_name>'*
               * it uses api.py functions to provide service to these urls requests.
            * __init__.py : dummy file for this sub app (under quiz app)
         * gcp app (folder)
            * __init__.py : dummy file for this sub app (under quiz app)
            * datastore.py : defines the functions for *list_entities and save_question* 
            * here there are no blue print because they are addressed in api. This folder acts as logical entity for all the datastore 
            * ../api/api.py files uses these functions in its fetch and evaluate definitions
         * webapp app (folder)
            * encompasses the templates and java scripts
            * router.py: does the handle the important url / core urls of the quiz app
            
               * / : root
               * '/client/<path:path>' : static path for angular(old version) client app
               * '/questions/add' : to add questions.
               * these are the 3 main tasks that user would do once he logs into the system.

# App Dev: Storing Image and Video Files in Cloud Storage - Python
* In this lab an image is taken in the input quiz
   * it is sent as a post request, so the input request.form['image'] has the image as the binary(perhaps)
* the image is saved in the cloud storage
* **public** url of which is saved in the database
* in the respose for quiz question, this public url is shared to front end which loads the image from the cloud storage image location directly (through the link)
      * *<img>* tag in the html page does hold the **ng-src** directive will have the image url. This img tag does the load of the image in the page.
* in *add.html* (under templates directory), *<input>* tag has *type=file* in it which indicates that the web form element would accept file as input. In our context, image file is what it is going to accept (it does not matter type of the image or theoritically, it does not matter if it is not even an image)
* google.cloud package usage in python :
   * storage is the method that we import for the server program
   * first create a storage client
   * get the handle for bucket ( by passing the bucket name)
   * create a blob for the new file
   * use the blob to upload the file
   * optionally make the blob public
   * get the blob url for storing in db for later access
* flask routing study:
   * add.html is called when *create question* is clicked
   * now this call raises the http.get request which routes or rather renders the html page (add.html)
   * when the page gets loaded and question , answers are entered and finally when submit is clicked
   * again the same service is called */questions/add/* but last time it was http.get but this time it is http.post
   * when post method request comes to flask rest service it does the save the q&a and routes to '/' directory and thats how the home screen is shown.

# App Dev: Adding User Authentication to your Application - Python
* firebase authentication
   * is a third party tool which takes the user credentials and contacts the respective email host for authentication.
   * the tool also gives a *.js* to be included in the web page to avail the firebase app and auth services
   * while registering the app to firebase, a unique url with respect the app is configured in firebase.
* How does it work in frontend:
   * *qiq-login.js* is included in the index.html
   * which enables us to include the angular component: <qiq-login> along with other hyperlinks.
   * file *qiq-login-template.html* has 
      * use id input text box
      * password input text box
      * login button
      * logout button
   * auth-factory.js has the *firebaseAuth* injected to it.
      * this makes the connection to firebase service method like registering, login logout vaidation
      * these methods are invoked when login screen buttons are pressed.
      * note that if you have multiple gcp project then you can enable it on all or or one. For all project the same steps of registration has to be done.
      * within a same project if multiple app requires the authentication then also same registration step has to happen. Here each url can have different method of authentication

# App Dev: Deploying the Application into App Engine Flexible Environment - Python
* lab focuses on creating a yaml file with environment configuration
* suitable for single app deployment
* first deployment is damn slow
* second deployment is faster
* canary deployment or blue green deployment are easy; rolling update is by default
   * canary deployment is using *split traffic* option.
# App Dev: Deploying the Application into Kubernetes Engine - Python
* splits the previous app into front end and backend
* front end:
   * split the *webclient* directory as the seperate app. Now this directory cannot be on its own, it requires other dependencies and those are moved along with this directory.
   * logically, the front end app collects the user inputs
      * anwers to the quests and feedback
   * it then publishes the same to pubsub
* backend:
   * subscribes to the pubsub topic 
   * on topic message, calls the evalute quiz and sentimental analysis of the feedback string
   * saves the score of quiz, rating and sentimental analysis score in the spanner db.
      
