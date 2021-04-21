# azure-ci-cd-pipeline
# Overview
This project demponstrates the steps on how to build a CI/CD pipeline using Azure Devops and Github to deploy a Pyhon Machine Learning Application to Azure App Services
Technolog



## Project Plan
Click on the below links to view the Trello Board and Project Plan

* [Trello board](https://trello.com/b/DKIrIpDZ/deploy-cicd-pipeline-in-azure)
* [Project Plan](https://docs.google.com/spreadsheets/d/1X-_tgCsTntOpF15eZHiBT_9kV8qnpq180sBZ4QrkGVM/edit#gid=1348135932)

## Instructions


### Architectural Diagram
  
![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/architecture.png "Architecture Diagram")

This project uses a number of technologies as well as the following platforms to implement a CI/CD Pipeline [Azure](https://portal.azure.com), [Azure DevOps](https://dev.azure.com) and [GitHub](https://github.com), therefore the relevant accounts are required. The below steps detail the actions to implementing a CD/CD Pipeline, Configuring our Python Machine Leaarning Application and Deployment to Azure App Services:

## Development Environment
For our development environment we will use Azure Shell and push our code to a GitHub repository. To implement this the following steps need to be performed:

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/cloud-dev.png "Development Environment")


1. Log into the Azure Portal and access the cloud shell as per the below image


   ![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/cloud-shell.png "Cloud Shell")


2. Generate an SSH Key by running the following command from Azure Cloud Shell : ``` ssh-keygen -t rsa ```. Leave the passphrase blank. More information on SSH Keys can be found on [GitHub](https://docs.github.com/en/github/authenticating-to-github/about-ssh)


   ![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/ssh-key-gen.png "Generate SSH Key")


3. Once the Key has been generated run the following command from the Cloud Shell: ``` cat ~/.ssh/id_rsa.pub ``` , and copy the SSH Key
4. Navigate to [SSH and GPG key](https://github.com/settings/keys) under setting in your GitHub Account.
5. Click on New SSH key
   1. Add a title, eg Azure Cloud Shell.
   2. Paste the copied key under "Key".
   3. Click on Add SSH Key.
   
   All saved SSH Keys will be displayed as per the below image:

   ![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/all-keys.png "SSH Keys") 

6. Once the SSH Key has been successfuly setup, you can clone the Git Repository by running ``` git clone git@github.com:wallandall/azure-ci-cd-pipeline.git ``` from the root of the Azure Cloud Shell. This will copy all project files and if successful, the output will be similar to the below image:
  ![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/cloned.png "Cloned Git Repository")  

7. Setup a Python Virtual Environment by running ``` python3 -m venv ~/.azure-ci-cd-pipeline ``` from the root of the Azure Cloud Shell
8. Activate the Python Environment by running ``` source ~/.azure-ci-cd-pipeline/bin/activate ``` 
9. To install dependencies and ensure the project is properly configured, navigate to the project directory and run the following commands from Azure Cloud Shell:
   1.  ``` make all ``` , this perform the following actions:
       1.   Installs project requirements from the requirements.txt file
       2.   Runs pytest on test_hello.py
       3.   Runs pylint
         If all succesful, the below results will be displayed

         ![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/make-all.png "Make All")  
   2. To create an Azure App Service and initial deployment of your application run ```az webapp up -n <your-appservice-name> -l your-location ``` 
       1.  -n attribute is the name of your application and needs to be unique. Replace <your-appservice-name> with the name of your application, this will be included in the application URL.
       2.  -l attribute defines the location your application will be deployed to eg: germanywestcentral . More information on az webapp commands can be found [here](https://docs.microsoft.com/en-us/cli/azure/webapp?view=azure-cli-latest)
    After successfully running the above commands, you will see the below output: 

    ![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/az-webapp-up.png "Azure App Services") 

    The URL of your application will be displayed in the output and can be used to access the application, for example:

    ![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/app-service-url.png "Running application") 

## GitHub Actions
An inportant compenent of Continuous Integration is testing all code, to achieve this we will use GitHub Actions. 
![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/github-actions.png "GitHub Actions")

To enable GitHub Actions, navigate to your code repository and perform the below steps:
1. Click on Actions
2. Under "Get Started with GtHub Actions", click "setup a new workflow yourself" and replace the content with the 
content from the following [URL](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/.github/workflows/
main.yml)
3. Commit your changes.
4. Make sure to update your project by running ```git pull``` from the project directory in Azure Code Shell
If everythign is setup correctly and all tests pass you should see results similar to the below image, with a green 
tick

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/github-actions-results.png "GitHub 
Actions")

The Status Badge in the ReadME File should also indicate if your tests are passing succeffuly 

