# AzureKeyVaultSpring


Skip to end of metadata
Created by Dey, Victor, last modified on Mar 01, 2019 Go to start of metadata
Create spring Project 

       Go to https://start.spring.io/  and  in search dependencies search “Azure Key Vault”   and add it  as dependency .

       Click on generate project and download it locally
       Go to eclipse and import the project .
       If you go to the POM ,you can see the following dependencies has been downloaded in the sample project.
       

   Register your application

   Open AZURE CLI  and execute the following commands to Register your application with Azure Active directory ( you can do from UI  also)

   az login -u id  -p password
   az account list
output

[

  {

    "cloudName": "AzureCloud",

    "id": "ssssssssc09d",

    "isDefault": true,

    "name": "JB-SelfService",

    "state": "Enabled",

    "tenantId": "xxxxxxxx",

    "user": {

      "name": "admVD08888@jetblue.com",

      "type": "user"

    }

  }

]

 az account set -s ssssssssc09d
 az group  show --name JB-RG-DEV-ITF
output

{

  "id": "/subscriptions/subscriptionsid/resourceGroups/JB-R

G-DEV-ITF",

  "location": "eastus",

  "managedBy": null,

  "name": "JB-RG-DEV-ITF",

  "properties": {

    "provisioningState": "Succeeded"

  },

  "tags": {}

}

az ad sp create-for-rbac -n "<your project name>>" --role contributor  --scopes /subscriptions/subscriptionsid/resourceGroups/JB-RG-DEV-ITF ( note if you don’t have permission please raise one ticket and ask itdevops to do it – I have given “NotificationManager” as project name)
 output

{

  "appId": "vvvvvvvv",  // it is  the client-id

  "displayName": "NotificationManager",

  "name": "http://NotificationManager",

  "password": "vvvvvvf", // it is the client-key

  "tenant": "vvvvvv"

}

 Configure  the key vault

Create key vault 

Add Access Policy -select  your project name ( my notificationmanager)in principal . you can modify your access ,I kept only the following access 


Create a secret -  I am doing manually but we can import from file also.




Configure Spring Project



Configure application.properties - Now Go back to the Spring project and add the following properties in Applcation.properties file
azure.keyvault.uri=  https://notificationmanager.vault.azure.net/  // take the DNS name from azue key vault overview  screen



azure.keyvault.client-id=already mentioned in the document previous  section



azure.keyvault.client-key= already mentioned in the document previous   section

Code Snippet 





How to use it in Application,properties file

 We define one key azureservicebusconnection in key vault  and referred it in properties file  .  It worked perfectly .

azure.servicebus.connection-string=${azureservicebusconnection}

azure.servicebus.topic-name=b6.topic.fsn.test.flight

azure.servicebus.subscription-name=airbus

azure.servicebus.subscription-receive-mode=peeklock
