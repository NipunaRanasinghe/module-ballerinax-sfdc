# Salesforce Connector

## Salesforce
Salesforce is the world’s #1 CRM platform that employees can access entirely over the Internet (https://www.salesforce.com)

The Salesforce connector which is implemented in ballerina allows you to access the Salesforce REST API. SalesforceConnector covers the basic functionalities as well as the high level functionalities of the REST API. (https://developer.salesforce.com/page/REST_API)

Ballerina is a strong and flexible language. Also it is JSON friendly. It provides an integration tool which can be used to integrate the Salesforce API with other endpoints.  It is easy to write programs for the Salesforce API by having a connector for Salesforce. Therefor the Salesforce connector allows you to access the Salesforce REST API through Ballerina easily. 

Salesforce connector actions are being invoked by a ballerina main function. The following section provides you the details on how to use Ballerina Salesforce connector.


![alt text](https://github.com/erandiganepola/package-salesforce/blob/master/salesforce/salesforce.png)


## Compatibility

| Ballerina Version         | Connector Version         | API Version |
| ------------------------- | ------------------------- | ------------|
|  0.970.0-alpha1-SNAPSHOT  | 0.970.0-alpha1-SNAPSHOT   |   v37.0     |


## Getting started

1. Download the Ballerina tools distribution by navigating to https://ballerinalang.org/downloads/ and setup the SDK
2. Clone the repository by running the following command,
  `git clone https://github.com/wso2-ballerina/package-salesforce.git` and
   Import the package to your ballerina project.

### Prerequisites
Create a Salesforce developer account and create a connected app by visiting Salesforce (https://www.salesforce.com) and obtain the following parameters:
* Base URl (Endpoint)
* Client Id
* Client Secret
* Access Token
* Refresh Token
* Refresh Token Endpoint
* Refresh Token Path

IMPORTANT: This access token and refresh token can be used to make API requests on your own account's behalf. Do not share your access token, client secret with anyone.

Visit [here](https://help.salesforce.com/articleView?id=remoteaccess_authenticate_overview.htm) for more information on obtaining OAuth2 credentials.

### Working with Salesforce REST connector.

In order to use the Salesforce connector, first you need to create a SalesforceConnector endpoint by passing above mentioned parameters.

Visit `test.bal` file to find the way of creating Salesforce endpoint.
#### Salesforce struct
```ballerina
public struct SalesforceConnector {
    oauth2:OAuth2Endpoint oauth2EP;
}
```
#### Salesforce Endpoint
```ballerina
public struct SalesforceEndpoint {
    SalesforceConfiguration salesforceConfig;
    SalesforceConnector salesforceConnector;
}
```

#### init() function
```ballerina
public function <SalesforceEndpoint ep> init (SalesforceConfiguration salesforceConfig) {
    endpoint oauth2:OAuth2Endpoint oauth2Endpoint {
        baseUrl:salesforceConfig.oauth2Config.baseUrl,
        accessToken:salesforceConfig.oauth2Config.accessToken,
        clientConfig:{},
        refreshToken:salesforceConfig.oauth2Config.refreshToken,
        clientId:salesforceConfig.oauth2Config.clientId,
        clientSecret:salesforceConfig.oauth2Config.clientSecret,
        refreshTokenEP:salesforceConfig.oauth2Config.refreshTokenEP,
        refreshTokenPath:salesforceConfig.oauth2Config.refreshTokenPath,
        useUriParams:true
    };

    ep.salesforceConnector = {
                                 oauth2EP:oauth2Endpoint
                             };
}
```
#### Running sfdc tests
Create `ballerina.conf` file in `package-sfdc`, with following keys:
* ENDPOINT
* ACCESS_TOKEN
* CLIENT_ID
* CLIENT_SECRET
* REFRESH_TOKEN
* REFRESH_TOKEN_ENDPOINT
* REFRESH_TOKEN_PATH

Assign relevant string values generated for Salesforce app. 

Go inside `package-sfdc` using terminal and run test.bal file using following command `ballerina test sfdc`.


##### Example
 * Request

 ```ballerina
 import wso2/salesforce as sf;
 import ballerina/io;
 
    public function main (string[] args) {
        endpoint sf:SalesforceEndpoint salesforceEP {
            oauth2Config:{
                             accessToken:accessToken,
                             baseUrl:url,
                             clientId:clientId,
                             clientSecret:clientSecret,
                             refreshToken:refreshToken,
                             refreshTokenEP:refreshTokenEndpoint,
                             refreshTokenPath:refreshTokenPath,
                             clientConfig:{}
                         }
        };
    
        json|sf:SalesforceConnectorError response = salesforceEP -> getAvailableApiVersions();
            match response {
                json jsonRes => {
                    io:println(jsonRes);
                }
        
                sf:SalesforceConnectorError err => {
                    io:println(err);
                }
            }
```
* Response

JSON response or SalesforceConnectorError struct