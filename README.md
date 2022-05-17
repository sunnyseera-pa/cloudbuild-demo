# Continuous Deployment with Cloud Run and Git Hub

    * https://www.youtube.com/watch?v=GhSAQ19f4HA
    * https://www.kevinsimper.dk/posts/continuous-deployment-with-cloud-run-and-cloud-build

## To test this run the following this is a local test 
    * node server.js
    * Go to http://localhost:8080

## To build the docker container 
    * docker build -t cloudrunapp . 

## To run the docker container 
    * docker run -it -p 8080:8080 cloudrunapp

## Go to GitHbub and create repo
    * https://github.com/sunnyseera/docker.git
    * copy the Dockerfile + server.js and push to github

## Setting up Cloud Build to our GitHub
    * Go to Cloud Build on GCP
        * Make sure you enable Cloud Build API
    * Click Setup Trigger
        * Name: cloudrun
        * Event: Push to a branch
        * Repository: Connect New Repository - Connect your https://github.com/sunnyseera/docker.git
        * Build Configuration: Dockerfile (leave the rest now)
        * Create 
    * Go back to Cloud Build and click Triggers
        * You will see your trigger and click RUN
            * This will run a build of our container off the master branch
            * Go to history - you will see it building and you can click into it to view logs
    * Go to Cloud Build in the GCP console
        * Go to settings 
        * Change Build Functions to ENABLED
        * Change Service Accounts to ENABLED

## Setting up Cloud Run
    * Go to Cloud Run on GCP
        * Make sure the APIs are enbaled
        * Click CREATE SERVICE
            * On the Container Image URL section - Click SELECT
                * You can click through and select our recently deployed container
            * Select Region (choose one)
            * Select Allow Unauthenticated Invocations
            * Select CREATE
        * Once created click into your service and you should see a URL at the top
            * That should take you to your expected hello world page

## Combine Cloud Run and Cloud Build to automatically deploy on a commit
    * Go back to cloud build and click into your trigger
        * Change the Build Configuration from Dockerfile to Cloud Build YAML
        * Save
    * In your GitHub Repo
        * Add a new file called cloudbuild.yaml 
            * An example is here https://cloud.google.com/build/docs/build-config-file-schema
        * Because we have pushed Cloud Build will automatically run a build for us
    * Back in Cloud Build
        * You will have a new build and you can see we have 2 build steps that have come from our YAML
        * Take note of the push image command from step 2 
            * E.g. gcr.io/devops-ss-sandbox/github.com/sunnyseera/docker:d13d352
    * On the command line take note of your Cloud Run service name *
        * gcloud run services list 
            * E.g. docker
        * gcloud run deploy docker --region=us-central1 --platform=managed --image=gcr.io/devops-ss-sandbox/github.com/sunnyseera/docker:d13d352 
            * gcloud run deploy docker --region=<REGION> --platform=managed --image=<IMAGE_FROM_ABOVE>
                * TO DO CI/CD - We need this command and give it to cloud build for us
        * Ammend your CloudBuild.yml to have a new step
            * Because we have pushed Cloud Build will automatically run a build for us
            * Now everytime we push to our Repo cloud build will run and deploy to cloud run ;)

    * We can now automatically test dockerfiles in Github and see them running 
        * In Cloud Run you can create a custom domain for your URL from Cloud Build