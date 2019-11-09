##### Ingesting Data into Cloud using App Engine
* .yaml file is used for deployment of an app into app engine( PaaS)
* scripting/automating obtain google cloud instance requires you to have knowledge of command driven google cloud meta data extractor:
    * ```PROJECT_ID=$(gcloud info --format='value(config.project)') ``` : to get the project name.
