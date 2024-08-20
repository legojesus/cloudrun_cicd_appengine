# Cloud Run CICD Pipeline Deployment Into App Engine Example
A simple demonstration of a CI/CD pipeline using GCP's Cloud Run that triggers from GitHub and deploys a NodeJS "Hello World" app into App Engine.


## How to run/deploy: 

### Fork this repository
1. Fork this repo into your own controlled repository.
2. Clone it into your computer. 


### Create a service account
1. Navigate your way to IAM in GCP’s console either by typing “IAM” at the top search bar or by using the navigation menu on the left.
2. On the left sidebar, select “Service Accounts”.
3. Click on “Create Service Account” at the top of the page. Give the new service account a name, for example “cloud-build-test”, and click on “Continue”. 
4. Provide the following roles to the service account (simply type these in the search box of the role field): 
 - Cloud Build Service Account
 - App Engine Service Admin
 - App Engine Deployer
 - Service Account User
5. Click on “Continue” and skip the last step by clicking on “Done” at the bottom. You now have a service account ready to be used by Cloud Build. 
 

 ### Enable and setup App Engine
1. Navigate your way to App Engine in GCP’s console either by typing “app engine” at the top search bar or by using the navigation menu on the left. 
2. Enable the App Engine API if asked, click on “Create Application”, then choose a region where your app will run (This cannot be changed later! You’ll need to delete the project and create a new one if you want a different region).
3. Select the service account we just created, and finish the process by clicking on “Next”. 
4. In the following page, skip the suggested steps and instead go and enable the App Engine Admin API. If you do not enable this API, Cloud Build will fail to deploy your code to App Engine so make sure this API is enabled: https://console.cloud.google.com/apis/library/appengine.googleapis.com


### Connect Cloud Build To GitHub And Set Up A Trigger
1. Navigate to Cloud Build in the GCP Console and enable the Cloud Build API if asked. 
2. Click on “Repositories” on the left sidebar, make sure you are on “2nd Generation” at the top of the page, then click on “Create host connection”. 
3. Select GitHub (or whichever git provider you use) and proceed with the instructions on screen to allow Cloud Build to access your git provider. Please note the info message GCP provides regarding how/where it stores authentication tokens and what it uses them for. You might also be asked to enable the Secrets Manager API in the process, so confirm and enable it (that is where Cloud Build will store your git repo’s access tokens).
4. After successfully allowing Cloud Build to access your git provider, go back to the “Repositories” screen in Cloud Build and click on “Link Repository”. Select the git connection you just created in step 3, then select the relevant repository. Click on “Link” at the bottom to complete the process. 
5. Next, go to the “Triggers” section on the left sidebar, then click on “Create Trigger” at the top of the page.
6. Give the trigger a name such as “cloud-build-trigger-test”. Under “Event”, select “Push New Tag” since we want this trigger to work only when we push a new tag to our repository. Under “Source” select “2nd Gen” and select the repository you just added. For tags, you can specify the naming convention of specific tags, or you can just go with any tag by writing “.*” (dot-asterisk). 
7. Leave the rest as-is, scroll to the bottom and change the service account in the provided dropdown to the service account that we have created previously in the IAM section. 
Click on “Create” to finish this trigger’s setup. 


### Trigger Cloud Build To Deploy The Website
1. Navigate into the repo's cloned folder on your machine.
2. Create a new tag and push it: git tag v.0.0.1-test && git push –tags
3. Check Cloud Build's history in GCP's Console UI. 

If the build was successful, go to Appe Engine and open your website (the link on the top right corner of App Engine's dashboard), see if the website that opens says "Hello World". 
You can change the website's "Hello World" text in the <app.js> file, then deploy again by creating and pushing a new tag. 
