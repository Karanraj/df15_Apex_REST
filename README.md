# Apex REST Class

In this tutorial you will learn how to exposes your apex class to the external application through REST architecture with the following operation GET, POST, DELETE. Below are the steps involved in this tutorial

  1. Create a REST based apex class with HTTP get, post and delete
  2. Use the Workbench to verify the  REST class.

### REST Apex class 

Create an Apex REST class that will allow the external application to GET, POST and DELETE account records in salesforce

#### Step 1: Create an Apex REST class 

  1. Login into your developer org
  2. In Salesforce, click **your name** in the upper right corner of the screen. In the dropdown menu, click **Developer Console**
  ![alt tag](https://raw.github.com/Karanraj/df15_Apex_REST/master/Images/DeveloperConsole.png)
  3. In the Developer Console, click **File > New > Apex class**. Specify *‘MyRestResource’* as a class name and click **ok**
  4. Copy and paste the below code
  
  ```java
  @RestResource(urlMapping='/Account/*')
  global with sharing class MyRestResource {
    @HttpGet
    global static Account doGet() {
        RestRequest req = RestContext.request;
        RestResponse res = RestContext.response;
        String AcctNumber = req.requestURI.substring(req.requestURI.lastIndexOf('/')+1);
        Account result = [SELECT Id, Name, Phone, Website FROM Account WHERE AccountNumber = :AcctNumber];
        return result;
    }
  }
  ```
  5. Click **File > Save**
  
Now we have built a custom Apex REST class which will allow the external application to fetch the Account details by passing the ‘Account Number’. 

#### Step 2: Verify Apex REST class in workbench

In the step we will verify our apex REST class in the workbench tool

  1. Open https://workbench.developerforce.com/
  2. Select  Environment as ‘Production’ and select the checkbox to agree the terms and condition 
  ![alt tag](https://raw.github.com/Karanraj/df15_Apex_REST/master/Images/Workbench.png)
  3. Click Login with Salesforce button and enter your developer edition account credentials 
  4. In the Workbench tool select Utilities > REST Explorer
  ![alt tag](https://raw.github.com/Karanraj/df15_Apex_REST/master/Images/RestExplorer.png)
  5. In the REST Explorer window paste the following url in the box
  

  ```
  /services/apexrest/Account/CD439877
  ```

  ![alt tag](https://raw.github.com/Karanraj/df15_Apex_REST/master/Images/HttpGet.png)

>Note: CD439877 is the Account Number field value of an Account record, you can also pass any valid Account number from your org to get the details. To Get Account Number, Open any account record in your dev org and then copy the ‘Account Number’ field value

  ![alt tag](https://raw.github.com/Karanraj/df15_Apex_REST/master/Images/AccountNumber.png)

#### Step 3: Add Post and Delete service in Apex class

In this step we will add the two more methods in our apex class to support POST and DELETE service

1. Open Developer console
2. Click File > Open > Apex Class  and open ‘MyRestResource’ class
3. Copy and paste the below methods in the apex class

  ```java
   @HttpPost
    global static String doPost(String name,
        String phone, String website) {
        Account account = new Account();
        account.Name = name;
        account.phone = phone;
        account.website = website;
        insert account;
        return account.Id;
    }
   
   @HttpDelete
    global static void doDelete() {
        RestRequest req = RestContext.request;
        RestResponse res = RestContext.response;
        String accountId = req.requestURI.substring(req.requestURI.lastIndexOf('/')+1);
        Account account = [SELECT Id FROM Account WHERE Id = :accountId];
        delete account;
    }
    
  ```
Your final apex REST class should look like this 
  ![alt tag](https://raw.github.com/Karanraj/df15_Apex_REST/master/Images/FinalApexClass.png)

#### Step 4: Verify Apex REST services in Workbench 

##### POST REST Services:

  1. Open workbench tool and login with your developer account
  2. Go to Utilities > REST Explorer
  3. Select POST and in the URL box paste the following URL
  

  ```
  /services/apexrest/Account
  ``` 
  4. Paste the below json string in the request body
  

  ```json
  {
    "name" : "Trailhead",
    "phone" : "123456789",
    "website" : "developer.salesforce.com/trailhead"
  }
  ```
  ![alt tag](https://raw.github.com/Karanraj/df15_Apex_REST/master/Images/HttpPost.png)
  5. Click ‘Execute’
  
Salesforce returns a response with the Id of record. To view the record in your org, copy the Id by clicking the Show RAW response and follow the below steps

  6. Open your developer org
  7. Paste the Id of the record as following screenshot 
  ![alt tag](https://raw.github.com/Karanraj/df15_Apex_REST/master/Images/RecordID.png)

##### DELETE  REST Service

To delete account record with our Apex REST service, you have to pass the ID of the account record

  1. Open workbench tool and login with your developer account
  2. Go to Utilities > REST Explorer
  3. Select ‘Delete’ Option and  in the URL box paste the following URL
  

  ```
  /services/apexrest/Account/00128000005dOI1AAM
  ```
  ![alt tag](https://raw.github.com/Karanraj/df15_Apex_REST/master/Images/HttpDelete.png)

  4. Click ‘Execute’
	
Now the record is deleted successfully from your org, you can verify in your org.

>Note: 00128000005dOI1AAM - It is the ID of the Account record. You have to pass the ID of the Account record from your developer org.

## Summary:

**Congratulations!** You have created Apex REST services which allows external application to get ,create and delete account records. By utilizing custom Apex REST endpoints, developers can tailor the REST API to suit the business needs of their application. 

## Related Resource

* [Apex REST Documentation](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_rest_methods.htm) <br/>
* [Force.com REST API](https://developer.salesforce.com/page/REST_API) <br/>
* [Getting Started with the Force.com REST API](https://developer.salesforce.com/page/Getting_Started_with_the_Force.com_REST_API)
