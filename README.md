# azure-ci-cd-pipeline
# Overview
This project demponstrates the steps on how to build a CI/CD pipeline using Azure Devops and Github to deploy a Pyhon Machine Learning Application to Azure App Services

[![Python application test with Github Actions](https://github.com/webe-solutions/azure-ci-cd-pipeline/actions/workflows/main.yml/badge.svg)](https://github.com/webe-solutions/azure-ci-cd-pipeline/actions/workflows/main.yml)

## Project Plan
Click on the below links to view the Trello Board and Project Plan

* [Trello board](https://trello.com/b/DKIrIpDZ/deploy-cicd-pipeline-in-azure)
* [Project Plan](https://docs.google.com/spreadsheets/d/1X-_tgCsTntOpF15eZHiBT_9kV8qnpq180sBZ4QrkGVM/edit?usp=sharing)

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
9. To install dependencies and ensure the project is properly configured, navigate to the project directory and run ``` make all ``` , this will perform the following actions:
   1.  Installs project requirements from the requirements.txt file
   2.  Runs pytest on test_hello.py.
   3.  Runs pylint to lint all code.

   If all above steps are successful, the below results will be displayed

   ![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/make-all.png "Make All")  

 

## GitHub Actions
An inportant compenent of Continuous Integration is testing all code, to achieve this we will automate the process GitHub Actions. 
![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/github-actions.png "GitHub Actions")

To enable GitHub Actions, navigate to your code repository and perform the below steps:
1. Click on Actions
2. Under "Get Started with GtHub Actions", click "setup a new workflow yourself" and replace the content with the 
content from the following [URL](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/.github/workflows/main.yml)
3. Commit your changes.
4. Make sure to update your project by running ```git pull``` from the project directory in Azure Code Shell
If everythign is setup correctly and all tests pass you should see results similar to the below image, with a green 
tick

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/github-actions-results.png "GitHub 
Actions")

The Status Badge in the README.md File should also indicate if your tests are passing succeffuly 

## Azure App Service
For this project we will be deploying our application to Azure App Services. For initial setup and deployment follow the below steps, more infromation can be found [here](https://docs.microsoft.com/en-us/cli/azure/webapp?view=azure-cli-latest)

1. Replace the name of the app in make_predict_azure_app.sh with your-appservice-name (this is a unique name and will be used in the URL)
2. Save and commit your code.
3. From the project directory in Azure Cloud Shell run ``` az webapp up --name <your-appservice-name> --location "<your-location>" --sku B1 ```
   1. --name <your-appservice-ame> is the name of your application. This needs to be a unique value as it will be used used in your application url.
   2. --location "<your-location>" is the location you would like to deploy your application to, eg: germanywestcentral.
   3. --sku B1 refers to the size of the image created.

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/az-webapp-up.png "Azure App Services") 

In the Azure Portal you will be able to see an overview of Azure Aup Services as per the below image.

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/app-service.png "Azure App Services in Azure Portal") 

The URL of your application will be displayed in the output and can be used to access the application, for example:

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/app-service-url.png "Running application") 

1. Test application by running ```./make_predict_azure_app.sh ``` from the project directory in Azure Cloud Shell
   1. if you get a permission denied run ``` chmod +x make_predict_azure_app.sh ``` from the project directory in Azure Cloud Shell
The output should be as per the below image:
![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/make-predict.png "Make Prections") 


## Continuous Delivery with Azure DevOps
In this step we will implement Azure Pipelines to achieve Continuous Delivery for all changes to our application refer to the official docuentation for more [details](https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/python-webapp?view=azure-devops)
1. Create a new project in [Azure Devops](https://dev.azure.com/)
2. Setup a new service connection via Azure Resource Manager and Pipeline
3. Create a new pipeline and integrate it with your project

After successfuly adding the pipeline you will have similar results to the below images:

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/azure-pipelines1.png "Azure Pipelines") 


![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/azure-pipelines2.png "Successful Job") 


## Log Files
The log files for the running applrication can be accessed by navigating ``` https://<app-name>.scm.azurewebsites.net/api/logs/docker  ``` .

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/log-file.png "Log File") 

## Load Test with Locust

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/locust1.png "Log File")

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/locust2.png "Log File")

![alt text](https://github.com/webe-solutions/azure-ci-cd-pipeline/blob/main/images/locust3.png "Log File")

## Enhancements
1. Instead of using Azure Pipelines, GitHub Actions could be used to deploy the application. This would improve the proces as all workflows would be maintained by one system therfore removing complexity. Additionaly Microsoft has limited the use of the free tier of Azure DevOps and new projects will not be able to use the Azure Piplines. Microsoft have recommended the use of GitHub Actions, more information can be found in this [document](https://devblogs.microsoft.com/devops/change-in-azure-pipelines-grant-for-public-projects/)
2. Implement different environments for Test, QA and Production.
3. Adding predicitions for other cities
4. Implement Docker containers which would simplify application deployment
5. Use AzureDevops Boards instead of Trello Boards
   
## Demo
A video demonstration of the process can be found [here](https://youtu.be/FhjhtxDQIcs)
