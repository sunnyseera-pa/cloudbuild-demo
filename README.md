# Continuous Deployment with Cloud Run and GitHub

## To test this run the following this is a local test 
    * node server.js
    * Go to http://localhost:8080

## To build the docker container 
    * docker build -t cloudrunapp . 

## To run the docker container 
    * docker run -it -p 8080:8080 cloudrunapp

## Go to GitHbub and create repo
    * https://github.com/sunnyseera-pa/cloudbuild-demo.git
    * copy the Dockerfile + server.js and push to github

