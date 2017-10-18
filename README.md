# SFMC API Tutorial

> Guide to using SFMC API

## Table of Contents

* [Preamble](#preamble)
* [Prerequisites](#prerequisites)
* [How to make a SOAP API request using username and password as user credential](#how-to-make-a-soap-api-request-using-username-and-password-as-user-credential)
* [How to get a SFMC API key](#how-to-get-a-sfmc-api-key)
* [How to make a SOAP API request using API key as user credential](#how-to-make-a-soap-api-request-using-api-key-as-user-credential)
* [How to make a REST API request](#how-to-make-a-rest-api-request)
* [How to contribute](#how-to-contribute)

## Preamble

This guide is intended to introduce to a developer with no prior knowledge how to use postman and SFMC APIs. Content was used in the hands-on tutorial about SFMC APIs first at presented at the [Salesforce Marketing Cloud developer group October 2017 meeting](https://www.meetup.com/Salesforce-Marketing-Cloud-Developers-Group/events/237067605/). 

To keep things simple API request in tutorial will read and insert an email address to a data extension called contacts.

For more information and as a next step recommend reading developer documentation at [Get Started, Marketing Cloud Developers](https://developer.salesforce.com/docs/atlas.en-us.mc-getting-started.meta/mc-getting-started/index.htm).

## Prerequisites

* [Google Chrome](http://www.google.com/chrome/) is a prerequisite for Postman REST Client.
* [Postman REST Client](https://www.getpostman.com/) is used to make API requests.
* Sign-up for a [GitHub account](https://github.com/) if you don't already have one so you can fork repo, make suggestions on issues and follow along with tutorials.
* Access to [Salesforce Marketing Cloud](https://www.marketingcloud.com/) so that you can see result of request.

## How to make a SOAP API request using username and password as user credential

Refer to [Get Started with the SOAP Web Services API](https://developer.salesforce.com/docs/atlas.en-us.noversion.mc-apis.meta/mc-apis/getting_started_developers_and_the_exacttarget_api.htm) for list of endpoint links. In our case we will be using the `S7 Instance`. 

Use https://trust.marketingcloud.com/ to determine your instance.

Following is SOAP API request using username and password as user credential 

```
URL: https://webservice.s7.exacttarget.com/Service.asmx
REQUEST: POST
HEADERS:
Content-Type: text/xml
SOAPAction: Retrieve

<?xml version="1.0" encoding="UTF-8"?>
<s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope" xmlns:a="http://schemas.xmlsoap.org/ws/2004/08/addressing" xmlns:u="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">
   <s:Header>
      <a:Action s:mustUnderstand="1">Retrieve</a:Action>
      <a:MessageID>urn:uuid:7e0cca04-57bd-4481-864c-6ea8039d2ea0</a:MessageID>
      <a:ReplyTo>
         <a:Address>http://schemas.xmlsoap.org/ws/2004/08/addressing/role/anonymous</a:Address>
      </a:ReplyTo>
      <a:To s:mustUnderstand="1">{{soapEndPoint}}</a:To>
      <o:Security xmlns:o="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" s:mustUnderstand="1">
         <o:UsernameToken>
            <o:Username>{{soapUsername}}</o:Username>
            <o:Password>{{soapPassword}}</o:Password>
         </o:UsernameToken>
      </o:Security>
   </s:Header>
  <s:Body xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
      <RetrieveRequestMsg xmlns="http://exacttarget.com/wsdl/partnerAPI">
         <RetrieveRequest>
            <ObjectType>DataExtensionObject[Data Extension Name]</ObjectType>
            <Properties>email</Properties>
         </RetrieveRequest>
      </RetrieveRequestMsg>
  </s:Body>
</s:Envelope>

```

Notes:

* **{{soapEndPoint}}** replace with SOAP EndPoint https://webservice.s7.exacttarget.com/Service.asmx
* **{{soapUsername}}** replace with username, note that user must have API priority selected in user account
* **{{soapPassword}}** replace with password
* **[Data Extension Name]** replace with data extension name, in our case the data extension is called `contacts`

On request following is the expected body of response.

```

    <soap:Body>
        <RetrieveResponseMsg xmlns="http://exacttarget.com/wsdl/partnerAPI">
            <OverallStatus>OK</OverallStatus>
            <RequestID>5d3901bd-d7ca-4fde-9973-3d24ca5b87b4</RequestID>
            <Results xsi:type="DataExtensionObject">
                <PartnerKey xsi:nil="true" />
                <ObjectID xsi:nil="true" />
                <Type>DataExtensionObject</Type>
                <Properties>
                    <Property>
                        <Name>email</Name>
                        <Value>matt@mattcameron.me</Value>
                    </Property>
                </Properties>
            </Results>
        </RetrieveResponseMsg>
    </soap:Body>

```

Notes:

* list of emails will depend on contents of data extension.

## How to get a SFMC API Key

To get a SFMC API Key refer to [Get an API Key walk through](https://developer.salesforce.com/docs/atlas.en-us.mc-getting-started.meta/mc-getting-started/get-api-key.htm)

The URL for the Marketing Cloud App Center is https://appcenter-auth.s1.marketingcloudapps.com/

For reading and writing to a data extension an API intergration will be required with data extensions read write privileges.

`{{clientId}}` and `{{clientSecret}}` will be advised when API intergration is setup.

Following is REST API request to get API key

```
URL: https://auth.exacttargetapis.com/v1/requestToken
REQUEST: POST
HEADERS:
Content-Type: application/json

{
    "clientId":"{{clientId}}",
    "clientSecret":"{{clientSecret}}"
}

```

Notes:

* **{{clientId}}** replace with Client ID from API intergration
* **{{clientSecret}}** replace with Client Secret from API intergration

On request following is the expected body of response.

```

{
    "accessToken": "{{accessToken}}",
    "expiresIn": 3478
}

```

Notes:

* **{{accessToken}}** contains access token that is used with requests using API key
* AccessToken expires is valid for 60 minutes

For more information refer to https://developer.salesforce.com/docs/atlas.en-us.mc-getting-started.meta/mc-getting-started/get-access-token.htm


## How to make a SOAP API request using API key as user credential

## How to make a REST API request

## How to contribute

Make a pull request or suggestion on [issues](https://github.com/sfmcdg/sfmc-api-tutorial/issues)
