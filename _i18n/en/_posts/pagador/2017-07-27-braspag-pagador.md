---
layout: manual
title: API Rest Integration Guide
description: Gateway Braspag Technical Integration
search: true
translated: true
categories: manual
sort_order: 1
tags:
    - 1. Pagador
language_tabs:
  json: JSON
  shell: cURL
  
---

# Introduction to the Pagador API

The purpose of this documentation is to guide the developer on how to integrate with the **Pagador API**, Braspag's payment gateway, describing all available services using request and response examples.

Below is the representation of a standard transactional flow, followed by a short description of the main parties involved:

![Fluxo Transacional](https://braspag.github.io/images/fluxo-transacional-en.png)

* **E-commerce platform:** Provides technical solution for merchants to build all the infrastructure and processes needed for their e-commerce operation.
* **Gateway:** Connects e-commerces with payment services (acquirer, *boleto* bill, issuer), making it easier for the merchants to manage payment providers.
* **Acquirer:** Connects the transaction with the payment networks (brands) and settles the transaction for the merchants.
* **Card network (or brand):** Communicates with the issuer of the transaction card and settles the transaction for the acquirers.
* **Issuer:** Gives credit and stores the buyer's money. In the transaction, either approves or denies it for reasons of balance, card validity or fraud. Settles the transaction for the brand.

## Attributes of the Solution

The Pagador API solution was developed with the market reference REST technology, which works regardless of the technology used by our customers. Therefore, it is possible to integrate using various programming languages, such as: *ASP*, *ASP.Net*, *Java*, *PHP*, *Ruby* and *Python*.

Here are some of the main benefits of using Braspag's e-commerce platform:

* **No proprietary apps**: no applications need to be installed in the online store environment.
* **Simplicity**: the protocol used is purely HTTPS.
* **Easy testing**: Braspag's platform offers a publicly accessible Sandbox environment that allows the developer to create a test account without the need for accreditation, making it easier and faster to start integrating.
* **Credentials**: the client's credentials (affiliation number and access key) are transmitted in the header of the HTTP request.
* **Security**: the information is always exchanged between the store server and Braspag, without the customer's browser.
* **Multiplatform integration**: the integration is performed through REST APIs, which allows the use of different applications.

## Integration Architecture

The model used in the integration of the APIs is simple. It is based in the use of two URLs:
* URL for transactions - specific for operations such as authorization, capture and cancellation of transactions.
* URL for queries - specific for consultation operations, such as a transaction search.

<br/>To perform an operation, combine the environment base URL with the desired operation endpoint (as in https://api.braspag.com.br/*v2/sales/*) and then submit a request to this URL using the correct HTTP method.

|HTTP Method| Description|
|---|---|
|**GET**|Retrieves existing resources, e.g.: a transaction query.|
|**POST**|Creates new resources, e.g.: creating a transaction.|
|**PUT**|Updates existing resources, e.g.: capturing or canceling a previously authorized transaction.|

All operations require the access credentials **"Merchant ID"** and **"Merchant Key"** to be sent through the request header.<br>
Each request operation will return an [HTTP Status](https://braspag.github.io//en/manual/braspag-pagador#list-of-http-status-code) code, indicating whether it was successfully completed or not.

## Test and Production Environments

Use our **Sandbox** environment to test our products and services before bringing your solution into the **Production** environment.

<aside class="notice">Access credentials must be used to authenticate all requests made to the API endpoints.</aside>
<aside class="warning">For safety reasons, these credentials must not be unduly shared or exposed.</aside> 

### Sandbox Environment

Create an account in our sandbox and try out our APIs during your testing phase, with no commitment.

|Information|Description|
|----|----|
|Access credentials|`MerchantId` and `MerchantKey` received after your test account creation in [Sandbox Registration](https://cadastrosandbox.braspag.com.br/).|
|Base URL for transactions|https://apisandbox.braspag.com.br/|
|Base URL for query services|https://apiquerysandbox.braspag.com.br/|

### Production Environment

Once you are done running your tests and ready for *go-live*, you can implement your solution in the production environment.

|Information|Description|
|---|---|
|Access credentials|`MerchantId` and `MerchantKey` provided by Braspag. Send us an email (*comercial@braspag.com.br*) for more information.|
|Base URL for transactions|https://api.braspag.com.br/|
|Base URL for query services|https://apiquery.braspag.com.br/|

## Transactional Terms

In order for you to better enjoy the features available in our API, it is important to first understand some of the concepts involved in the process of a credit card transaction:

|Step|Description|
|---|---|
|**Authorization**| Makes the process of a credit card sale possible. The authorization (also called pre-authorization) only earmarks the customer's fund, not yet releasing it from their account.|
|**Capture**| Moves the transaction out of the pending state, so that the charging can take effect. The time limit for capturing a pre-authorized transaction varies between acquirers, reaching up to 5 days after the pre-authorization date.
|**Automatic Capture**| **Authorizes** and **captures** the transaction at the same time, exempting the merchant from sending a confirmation.
|**Cancellation**| Cancels the sale on the same day of its authorization/capture. In case of an **authorized** transaction, the cancellation will release the limit of the card that has been earmarked. If the transaction has already been **captured**, the cancellation will undo the sale, but only if executed on the same day.
|**Refund**| Cancels de sale on the day after its capture. The transaction will be submitted to the chargeback process by the acquirer.

<aside class="warning">Reminder: An authorized transaction only generates credit to the merchant after being captured.</aside>

Some of the important features that we offer for your transactions are listed below:

|Term|Description|
|---|---|
|**Antifraude**| A feature which consists in a fraud prevention platform, that provides a detailed risk analysis of online purchases. The process is fully transparent to the cardholder. According to a pre-established criteria, the request can be automatically accepted, rejected or redirected for manual review. For further information, refer to the [Antifraud](https://braspag.github.io//en/manual/antifraude) integration guide.
|**Autenticação**| A process which allows the transaction to pass through the card issuing bank authentication process, bringing more security to the sale as it transfers the risk of fraud to the issuer. For further information, refer to the [3DS 2.0 Authentication](https://braspag.github.io//en/manualp/emv3ds) documentation. 
|**Cartão Protegido**| A platform that offers secure storage of sensitive credit card data. This data is transformed into an encrypted code called "token", which can be stored in a database. With this platform, the store is able to offer features such as "*1-Click Purchase*" and "*Transaction Submission Retry*", while preserving the integrity and confidentiality of that data. For further information, refer to the [Cartão Protegido](https://braspag.github.io//en/manual/cartao-protegido-api-rest) integration guide.

## Braspag Support Service

<aside class="notice">Braspag offers high availability support. You can contact us from 9AM to 7PM weekdays, through our 24 hour emergency call service or through our web-based tool. Our support team speaks Portuguese, English and Spanish.</aside>

You can access our web support tool here: [Zendesk](https://suporte.braspag.com.br/hc/en-us) and check our [Atendimento Braspag](https://suporte.braspag.com.br/hc/pt-br/articles/360006721672-Atendimento-Braspag) article, in Portuguese, for further information about our support service.

# Payment Methods

The Pagador API works with transactions made with the following payment methods: credit card, debit card, *boleto* bill, electronic transfer, e-wallet and voucher.

To prevent duplicate orders from occurring during a transaction, Pagador has the option of blocking duplicate orders which, when enabled, returns the "302" error code, informing that the `MerchantOrderId` sent is duplicated. For more details on the feature, you can refer to [this article](https://suporte.braspag.com.br/hc/pt-br/articles/360030183991), in Portuguese.

## Credit and Debit Cards

### Creating a Credit Transaction

When requesting authorization for a credit transaction, it is necessary to follow the contract below. The details related to your affiliation are sent within the `Payment.Credentials` node, and must be sent whenever a new authorization request is submitted for approval.

If your store uses *Retry* or *Loadbalance* services, affiliations must be registered by the customer support team. To request the registration of affiliations, [click here](https://suporte.braspag.com.br/hc/en-us/requests/new) and send your request.

<aside class="warning">Important: The order identification number (MerchantOrderId) does not change, remaining the same throughout the transactional flow. However, an additional number (SentOrderId) can be generated for the order and used during the transaction. This number (SentOrderId) will only be different in case of adaptation to the acquirer's rules or in case there are repeated order identification numbers (MerchantOrderId).</aside>

The parameters contained within the `Address` and `DeliveryAddress` nodes are **mandatory** when the transaction is submitted to [Antifraud](https://braspag.github.io//en/manual/antifraude) or to the **Velocity** analysis. These parameters are marked with an * in the mandatory column of the table below.

Here are request and answer examples of how to create a credit transaction:

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{
   "MerchantOrderId":"2017051002",
   "Customer":{
      "Name":"Shopper Name",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email": "shopper@braspag.com.br",
      "Birthdate":"1991-01-02",
      "Address":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment":{
      "Provider":"Simulado",
      "Type":"CreditCard",
      "Amount":10000,
      "Currency":"BRL",
      "Country":"BRA",
      "Installments":1,
      "Interest":"ByMerchant",
      "Capture":true,
      "Authenticate":false,
      "Recurrent":false,
      "SoftDescriptor":"Message",
      "DoSplit":false,
      "CreditCard":{
         "CardNumber":"4551870000000181",
         "Holder": "Cardholder Name",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa",
         "SaveCard":"false",
         "Alias":"",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
      },
      "Credentials":{
         "code":"9999999",
         "key":"D8888888",
         "password":"LOJA9999999",
         "username":"#Braspag2018@NOMEDALOJA#",
         "signature":"001"
      },
      "ExtraDataCollection":[
         {
            "Name":"FieldName",
            "Value":"FieldValue"
         }
      ]
   }
}
```

```shell
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   "MerchantOrderId":"2017051002",
   "Customer":{
      "Name": "shopper Name",
      "Identity": "12345678909",
      "IdentityType": "CPF",
      "Email": "shopper@braspag.com.br",
      "Birthdate":"1991-01-02",
      "Address":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment":{
      "Provider":"Simulado",
      "Type":"CreditCard",
      "Amount":10000,
      "Currency":"BRL",
      "Country":"BRA",
      "Installments":1,
      "Interest":"ByMerchant",
      "Capture":true,
      "Authenticate":false,
      "Recurrent":false,
      "SoftDescriptor":"Message",
      "DoSplit":false,
      "CreditCard":{
         "CardNumber":"455187******0181",
         "Holder": "Cardholder Name",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa",
         "SaveCard":"false",
         "Alias":"",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
      },
      "Credentials":{
         "Code":"9999999",
         "Key":"D8888888",
         "Password":"LOJA9999999",
         "Username":"#Braspag2018@NOMEDALOJA#",
         "Signature":"001"
      },
      "ExtraDataCollection":[
         {
            "Name":"FieldName",
            "Value":"FieldValue"
         }
      ]
   }
}
--verbose
```

|Property|Description|Type|Size|Mandatory|
|-----------|----|-------|-----------|---------|
|`MerchantId`|Store identifier at Braspag.|GUID|36|Yes (through header)|
|`MerchantKey`|Public key for dual authentication at Braspag.|Text|40|Yes (through header)|
|`RequestId`|Store-defined request identifier used when the merchant uses different servers for each GET/POST/PUT.|GUID|36|No (through header)|
|`MerchantOrderId`|Order ID number.|Text|50|Yes|
|`Customer.Name`|Customer's name.|Text|255|Yes|
|`Customer.Identity`|Customer's ID number.|Text|14|No|
|`Customer.IdentityType`|Customer's ID document type (CPF or CNPJ).|Text|255|No|
|`Customer.Email`|Customer's email address.|Text|255|No|
|`Customer.Birthdate`|Customer's date of birth in the YYYY-MM-DD format.|Date|10|No|
|`Customer.IpAddress`|Customer's IP address. IPv4 and IPv6 support.|Text|45|No|
|`Customer.Address.Street`|Customer's address street.|Text|255|No*|
|`Customer.Address.Number`|Customer's contact address number.|Text|15|No*|
|`Customer.Address.Complement`|Customer's contact address additional information.|Text|50|No*|
|`Customer.Address.ZipCode`|Customer's contact address zip code.|Text|9|No*|
|`Customer.Address.City`|Customer's contact address city.|Text|50|No*|
|`Customer.Address.State`|Customer's contact address state.|Text|2|No*|
|`Customer.Address.Country`|Customer's contact address country.|Text|35|No*|
|`Customer.Address.District`|Customer's neighborhood.|Text|50|No*|
|`Customer.DeliveryAddress.Street`|Delivery address street.|Text|255|No*|
|`Customer.DeliveryAddress.Number`|Delivery address number.|Text|15|No*|
|`Customer.DeliveryAddress.Complement`|Delivery address additional information.|Text|50|No*|
|`Customer.DeliveryAddress.ZipCode`|Delivery address zip code.|Text|9|No*|
|`Customer.DeliveryAddress.City`|Delivery address city.|Text|50|No*|
|`Customer.DeliveryAddress.State`|Delivery address state.|Text|2|No*|
|`Customer.DeliveryAddress.Country`|Delivery address country.|Text|35|No*|
|`Customer.DeliveryAddress.District`|Delivery address neighborhood.|Text|50|No*|
|`Payment.Provider`|Name of payment method provider.|Text|15|Yes|
|`Payment.Type`|Payment method type.|Text|100|Yes|
|`Payment.Amount`|Order amount in cents.|Number|15|Yes|
|`Payment.ServiceTaxAmount`|Applicable for airlines only. Value of the amount of the authorization to be allocated to the service charge. Note: This value is not added to the authorization value.|Number|15|No|
|`Payment.Currency`|Currency in which the payment will be made (BRL / USD / MXN / COP / CLP / ARS / PEN / EUR / PYN / UYU / VEB / VEF / GBP).|Text|3|No|
|`Payment.Country`|Country in which the payment will be made.|Text|3|No|
|`Payment.Installments`|Number of installments.|Number|2|Yes|
|`Payment.Interest`|Installment type - Store ("ByMerchant") or Issuer ("ByIssuer").|Text|10|No|
|`Payment.Capture`|Indicates whether the authorization should use automatic capture ("true") or not ("false"). Please check with acquirer about the availability of this feature.|Boolean|---|No (default "false")|
|`Payment.Authenticate`|Indicates whether the transaction should be authenticated ("true") or not ("false"). Please check with acquirer about the availability of this feature.|Boolean|---|No (default "false")|
|`Payment.Recurrent`|Indicates whether the transaction is of recurring type ("true") or not ("false"). The "true" value will not set a new recurrence, it will only allow the execution of a transaction without the need to send CVV. `Authenticate` must be "false" when `Recurrent` is "true". **For Cielo transactions only.** |Boolean|---|No (default "false")|
|`Payment.SoftDescriptor`|Text to be printed on bearer's invoice.|Text|13|No|
|`Payment.DoSplit`|Indicates whether the transaction will be split between multiple accounts ("true") or not ("false").|Boolean|---|No (default "false")|
|`Payment.ExtraDataCollection.Name`|Name of the extra data field.|Text|50|No|
|`Payment.ExtraDataCollection.Value`|Value of the extra data field.|Text|1024|No|
|`Payment.Credentials.Code`|Affiliation generated by acquirer.|Text|100|Yes|
|`Payment.Credentials.Key`|Affiliation key/token generated by acquirer.|Text|100|Yes|
|`Payment.Credentials.Username`|Username generated on credential process with the **Getnet** acquirer (field must be submitted if transaction is directed to Getnet).|Text|50|No|
|`Payment.Credentials.Password`|Password generated on credential process with the **Getnet** acquirer (field must be submitted if transaction is directed to Getnet).|Text|50|No|
|`Payment.Credentials.Signature`|Submission of the *TerminalID* for Global Payments (applicable to merchants affiliated with this acquirer). E.g.: "001". For **Safra**, send establishment name, city and state concatenated with a semicolon (;), e.g.: “EstablishmentName;SaoPaulo;SP”.|Text|3|No|
|`Payment.PaymentFacilitator.EstablishmentCode`|Facilitator establishment code. “Facilitator ID” (Facilitator register with card network).<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Number|11|Yes for facilitators|
|`Payment.PaymentFacilitator.SubEstablishment.EstablishmentCode`|Sub-merchant's establishment code. “Sub-Merchant ID” (Sub-accredited register with facilitator).<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Number|15|Yes for facilitators|
|`Payment.PaymentFacilitator.SubEstablishment.Mcc`|Sub-merchant's MCC.<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Number|15|Yes for facilitators|
|`Payment.PaymentFacilitator.SubEstablishment.Address`|Sub-merchant's address.<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Text|15|Yes for facilitators|
|`Payment.PaymentFacilitator.SubEstablishment.City`|Sub-merchant's city.<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Text|15|Yes for facilitators|
|`Payment.PaymentFacilitator.SubEstablishment.State`|Sub-merchant's state.<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Text|15|Yes for facilitators|
|`Payment.PaymentFacilitator.SubEstablishment.PostalCode`|Sub-merchant's postal code.<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Number|15|Yes for facilitators|
|`Payment.PaymentFacilitator.SubEstablishment.PhoneNumber`|Sub-merchant's telephone number.<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Number|15|Yes for facilitators|
|`Payment.PaymentFacilitator.SubEstablishment.Identity`|Sub-merchant's CNPJ or CPF.<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Number|15|Yes for facilitators|
|`Payment.PaymentFacilitator.SubEstablishment.CountryCode`|Sub-merchant's country code based on ISO 3166.<br><br>**Applicable to `Provider` Cielo30 or Rede2.**|Number|15|Yes for facilitators|
|`CreditCard.CardNumber`|Customer's card number.|Text|16|Yes|
|`CreditCard.Holder`|Name of cardholder printed on the card.|Text|25|Yes|
|`CreditCard.ExpirationDate`|Expiration date printed on the card.|Text|7|Yes|
|`CreditCard.SecurityCode`|Security code printed on the back of the card.|Text|4|Yes|
|`CreditCard.Brand`|Card brand.|Text|10|Yes|
|`CreditCard.SaveCard`|Indicates whether the card will be saved to generate the token (*CardToken*).|Boolean|---|No (default "false")|
|`CreditCard.Alias`|Name given by merchant to card saved as *CardToken*.|Text|64|No|
|`CreditCard.CardOnFile.Usage`|"First" if the card has been stored and it is your first use.<br>"Used" if the card has been stored and it has been used previously in another transaction.<br><br>**Applicable to `Provider` Cielo30 only.**|Text|-|No|
|`CreditCard.CardOnFile.Reason`|Indicates the purpose of the card storage, in case the `Usage` field is "Used".<br><br>"Recurring" - Scheduled recurring purchase (e.g.: subscription services).<br>"Unscheduled" - Unscheduled recurring purchase (e.g.: services apps).<br>"Installments" - Installment through recurrence. <br><br> **Applicable to `Provider` Cielo30 only.**|Text|-|Conditional|

#### Response

```json
{
"MerchantOrderId":"2017051002",
    "Customer":{
        "Name":"Shopper Name",
        "Identity":"12345678909",
        "IdentityType":"CPF",
        "Email": "shopper@braspag.com.br",
        "Birthdate":"1991-01-02",
        "Address":{
            "Street":"Alameda Xingu",
            "Number":"512",
            "Complement":"27th floor",
            "ZipCode":"12345987",
            "City":"São Paulo",
            "State":"SP",
            "Country":"BRA",
            "District":"Alphaville"
        },
        "DeliveryAddress":{
            "Street":"Alameda Xingu",
            "Number":"512",
            "Complement":"27th floor",
            "ZipCode":"12345987",
            "City":"São Paulo",
            "State":"SP",
            "Country":"BRA",
            "District":"Alphaville"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "DoSplit":false,
        "CreditCard":{
            "CardNumber": "455187******0181",
            "Holder": "Cardholder Name",
            "ExpirationDate":"12/2021",
            "SaveCard":"false",
            "Brand":"Visa",
            "Alias": "",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
        },
        "Credentials":{
            "Code":"9999999",
            "Key":"D8888888",
            "Password":"LOJA9999999",
            "Username":"#Braspag2018@NOMEDALOJA#"
        },
        "ProofOfSale": "20170510053219433",
        "AcquirerTransactionId": "0510053219433",
        "AuthorizationCode": "936403",
        "SoftDescriptor":"Message",
        "VelocityAnalysis": {
            "Id": "c374099e-c474-4916-9f5c-f2598fec2925",
            "ResultMessage": "Accept",
            "Score": 0
        },
        "PaymentId": "c374099e-c474-4916-9f5c-f2598fec2925",
        "Type":"CreditCard",
        "Amount":10000,
        "ReceivedDate": "2017-05-10 17:32:19",
        "CapturedAmount": 10000,
        "CapturedDate": "2017-05-10 17:32:19",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider":"Simulado",
        "ExtraDataCollection": [{
            "Name":"FieldName",
            "Value":"FieldValue"
        }],
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 2,
        "ProviderReturnCode": "6",
        "ProviderReturnMessage": "Operation Successful",
        "Links": [{
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/c374099e-c474-4916-9f5c-f2598fec2925"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.braspag.com.br/v2/sales/c374099e-c474-4916-9f5c-f2598fec2925/void"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
"MerchantOrderId":"2017051002",
    "Customer":{
        "Name":"Shopper Name",
        "Identity":"12345678909",
        "IdentityType":"CPF",
        "Email": "shopper@braspag.com.br",
        "Birthdate":"1991-01-02",
        "Address":{
            "Street":"Alameda Xingu",
            "Number":"512",
            "Complement":"27th floor",
            "ZipCode":"12345987",
            "City":"São Paulo",
            "State":"SP",
            "Country":"BRA",
            "District":"Alphaville"
        },
        "DeliveryAddress":{
            "Street":"Alameda Xingu",
            "Number":"512",
            "Complement":"27th floor",
            "ZipCode":"12345987",
            "City":"São Paulo",
            "State":"SP",
            "Country":"BRA",
            "District":"Alphaville"
    },
    "Payment":{
        "ServiceTaxAmount": 0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "DoSplit":false,
        "CreditCard":{
            "CardNumber": "455187******0181",
            "Holder": "Cardholder Name",
            "ExpirationDate":"12/2021",
            "SaveCard":"false",
            "Brand":"Visa",
            "Alias": "",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
        },
        "Credentials":{
            "code":"9999999",
            "key":"D8888888",
            "password":"LOJA9999999",
            "username":"#Braspag2018@NOMEDALOJA#",
        },
        "ProofOfSale": "20170510053219433",
        "AcquirerTransactionId": "0510053219433",
        "AuthorizationCode": "936403",
        "SoftDescriptor":"Message",
        "VelocityAnalysis": {
            "Id": "c374099e-c474-4916-9f5c-f2598fec2925",
            "ResultMessage": "Accept",
            "Score": 0
        },
        "PaymentId": "c374099e-c474-4916-9f5c-f2598fec2925",
        "Type":"CreditCard",
        "Amount":10000,
        "ReceivedDate": "2017-05-10 17:32:19",
        "CapturedAmount": 10000,
        "CapturedDate": "2017-05-10 17:32:19",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider":"Simulado",
        "ExtraDataCollection": [{
            "Name":"FieldName",
            "Value":"FieldValue"
        }],
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 2,
        "ProviderReturnCode": "6",
        "ProviderReturnMessage": "Operation Successful",
        "Links": [{
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/c374099e-c474-4916-9f5c-f2598fec2925"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.braspag.com.br/v2/sales/c374099e-c474-4916-9f5c-f2598fec2925/void"
            }
        ]
    }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`AcquirerTransactionId`|Transaction ID at payment provider.|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of sale reference.|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code from the acquirer.|Text|300|Alphanumeric text|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date the transaction was received by Braspag.|Text|19|YYYY-MM-DD HH:mm:SS|
|`CapturedDate`|Date the transaction was captured. |Text|19|YYYY-MM-DD HH:mm:SS|
|`CapturedAmount`|Captured amount in cents.|Number|15|100 equivalent to R$ 1,00|
|`ECI`|Electronic Commerce Indicator. Represents the authentication result.|Text|2|E.g.: 5|
|`ReasonCode`|Return code from operation.|Text|32|Alphanumeric|
|`ReasonMessage`|Return message from operation.|Text|512|Alphanumeric|
|`Status`|Transaction status.|Byte|2|E.g.: 1|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and issuer).|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and issuer).|Text|512|Transaction Approved|

### Creating a Debit Transaction

A debit card transaction creation is similar to that of a credit card, except for the fact that it is mandatory to submit it to the authentication process.

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment": {
      "Provider":"Simulado",
      "Type":"DebitCard",
      "Amount": 10000,
      "Currency":"BRL",
      "Country":"BRA",
      "Installments": 1,
      "Interest":"ByMerchant",
      "Capture": true,
      "Authenticate": true,
      "Recurrent": false,
      "SoftDescriptor":"Mensagem",
      "DebitCard":{
         "CardNumber":"4551870000000181",
         "Holder":"Nome do Portador",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa"
      },
      [...]
   }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment": {
      "Provider": "Simulado",
      "Type": "DebitCard",
      "Amount": 10000,
      "Currency": "BRL",
      "Country": "BRA",
      "Installments": 1,
      "Interest": "ByMerchant",
      "Capture": true,
      "Authenticate": true,
      "Recurrent": false,
      "SoftDescriptor": "Mensagem",
      "DebitCard":{
         "CardNumber":"4551870000000181",
         "Holder":"Nome do Portador",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa"
      },
      [...]
   }
}
```

|Property|Description|Type|Size|Mandatory|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Name of payment method provider.|Text|15|Yes|
|`Payment.Type`|Payment method type. In this case, "DebitCard".|Text|100|Yes|
|`Payment.Amount`|Order amount in cents.|Number|15|Yes|
|`Payment.Installments`|Number of installments.|Number|2|Yes|
|`Payment.ReturnUrl`|URL to which the user will be redirected at the end of the payment.|Text|1024|Yes|
|`DeditCard.CardNumber`|Customer's card number.|Text|16|Yes|
|`DeditCard.Holder`|Name of the cardholder printed on the card.|Text|25|Yes|
|`DebitCard.ExpirationDate`|Expiration date printed on the card, in the MM/YYYY format.|Text|7|Yes|
|`DebitCard.SecurityCode`|Security code printed on the back of the card.|Text|4|Yes|
|`DebitCard.Brand`|Card brand.|Text|10|Yes|

#### Response

```json
{
 [...]
  "Payment":{
    "DebitCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard":"false",
      "Brand":"Visa",
    },
    "AuthenticationUrl": "https://qasecommerce.cielo.com.br/web/index.cbmp?id=13fda1da8e3d90d3d0c9df8820b96a7f",
    "AcquirerTransactionId": "10069930690009D366FA",
    "PaymentId": "21423fa4-6bcf-448a-97e0-e683fa2581ba",
    "Type": "DebitCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 15:19:58",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider": "Cielo",
    "ReturnUrl": "http://www.braspag.com.br",
    "ReasonCode": 9,
    "ReasonMessage": "Waiting",
    "Status": 0,
    "ProviderReturnCode": "0",
    [...]
  }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
 [...]
  "Payment":{
    "DebitCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard":"false",
      "Brand":"Visa",
    },
    "AuthenticationUrl": "https://qasecommerce.cielo.com.br/web/index.cbmp?id=13fda1da8e3d90d3d0c9df8820b96a7f",
    "AcquirerTransactionId": "10069930690009D366FA",
    "PaymentId": "21423fa4-6bcf-448a-97e0-e683fa2581ba",
    "Type": "DebitCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 15:19:58",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider": "Cielo",
    "ReturnUrl": "http://www.braspag.com.br",
    "ReasonCode": 9,
    "ReasonMessage": "Waiting",
    "Status": 0,
    "ProviderReturnCode": "0",
    [...]
  }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`AcquirerTransactionId`|Transaction ID of the payment method provider.|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of sale reference.|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code from the acquirer.|Text|300|Alphanumeric text|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date the transaction was received by Braspag.|Text|19|YYYY-MM-DD HH:mm:SS|
|`ReasonCode`|Operation return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Operation return message.|Text|512|Alphanumeric|
|`Status`|Transaction Status.|Byte|2|E.g.: 1|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and issuer).|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and issuer).|Text|512|Transaction Approved|
|`AuthenticationUrl`|URL to which the holder will be redirected for authentication.|Text|56|https://qasecommerce.cielo.com.br/web/index.cbmp?id=13fda1da8e3d90d3d0c9df8820b96a7f|

### Creating a Debit Transaction with no Authentication

It is possible to process a debit card without having to submit your customer to the authentication process. You will find more details in the [Débito sem Senha](https://suporte.braspag.com.br/hc/pt-br/articles/360013285531) article, written in Portuguese.<br>That is the case with the "Coronavoucher" emergency aid, provided by the government, which can be consumed through the *Caixa Econômica Federal* virtual debit card. In such case, the request must follow the Debit Card pattern, only with **no authentication**, according to the example below.

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
      "Payment": {
         "Provider": "Cielo30",
         "Type": "DebitCard",
         "Amount": 10000,
         "Currency": "BRL",
         "Country": "BRA",
         "Installments": 1,
         "Capture": true,
         "Authenticate": false,
         "DebitCard":{
            "CardNumber":"5067220000000001",
            "Holder":"Nome do Portador",
            "ExpirationDate":"12/2021",
            "SecurityCode":"123",
            "Brand":"Elo"
        },
        [...]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment": {
      "Provider": "Cielo30",
      "Type": "DebitCard",
      "Amount": 10000,
      "Currency": "BRL",
      "Country": "BRA",
      "Installments": 1,
      "Capture": true,
      "Authenticate": false,
      "DebitCard":{
         "CardNumber":"5067220000000001",
         "Holder":"Nome do Portador",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Elo"
      },
      [...]
   }
}
```

|Property|Description|Type|Size|Mandatory|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Name of payment method provider. **Applicable to "Cielo30" only.**|Text|15|Yes|
|`Payment.Type`|Payment method type. In this case, "DebitCard".|Text|100|Yes|
|`Payment.Amount`|Order amount in cents.|Number|15|Yes|
|`Payment.Installments`|Number of installments. For this type, always use "1".|Number|2|Yes|
|`DeditCard.CardNumber`|Customer's card number.|Text|16|Yes|
|`DeditCard.Holder`|Name of cardholder printed on the card.|Text|25|Yes|
|`DebitCard.ExpirationDate`|Expiration date printed on the card, in the MM/YYYY format.|Text|7|Yes|
|`DebitCard.SecurityCode`|Security code printed on the back of the card.|Text|4|Yes|
|`DebitCard.Brand`|Card brand. For this type, always use "Elo".|Text|10|Yes|

#### Response

```json
{
 [...]
  "Payment": {
    "DebitCard": {
      "CardNumber": "506722******0001",
      "Holder": "Nome do Portador",
      "ExpirationDate": "12/2021",
      "SaveCard": false,
      "Brand": "Elo"
    },
    "AcquirerTransactionId": "10069930690009D366FA",
    "PaymentId": "21423fa4-6bcf-448a-97e0-e683fa2581ba",
    "Type": "DebitCard",
    "Amount": 10000,
    "ReceivedDate": "2017-05-11 15:19:58",
    "Currency": "BRL",
    "Country": "BRA",
    "Provider": "Cielo30",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 2,
    "ProviderReturnCode": "6",
    "ProviderReturnMessage": "Operation Successful",
    [...]
  }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
 [...]
  "Payment": {
    "DebitCard": {
      "CardNumber": "506722******0001",
      "Holder": "Nome do Portador",
      "ExpirationDate": "12/2021",
      "SaveCard": false,
      "Brand": "Elo"
    },
    "AcquirerTransactionId": "10069930690009D366FA",
    "PaymentId": "21423fa4-6bcf-448a-97e0-e683fa2581ba",
    "Type": "DebitCard",
    "Amount": 10000,
    "ReceivedDate": "2017-05-11 15:19:58",
    "Currency": "BRL",
    "Country": "BRA",
    "Provider": "Cielo30",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 2,
    "ProviderReturnCode": "6",
    "ProviderReturnMessage": "Operation Successful",
    [...]
  }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`AcquirerTransactionId`|Transaction ID of the payment method provider.|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of sale reference.|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code from the acquirer.|Text|300|Alphanumeric text|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date the transaction was received by Braspag.|Text|19|YYYY-MM-DD HH:mm:SS|
|`ReasonCode`|Operation return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Operation return message.|Text|512|Alphanumeric|
|`Status`|Transaction status.|Byte|2|E.g.: 1|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and issuer).|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and issuer).|Text|512|Transaction Approved|

### Capturing a Transaction

When a transaction is submitted with the `Payment.Capture` parameter as "false", there is the need of a later request for capturing the transaction, in order for it to be confirmed.

An authorization that is not captured by the deadline is automatically released by the acquirer. Merchants may have specific negotiations with the acquirers to change their capturing deadline. For further information about capture deadlines and refund, you can refer to [this article](https://suporte.braspag.com.br/hc/pt-br/articles/360028661812), in Portuguese.

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/sales/{PaymentId}/capture</span></aside>

```shell
--request PUT "https://apisandbox.braspag.com.br/v2/sales/{PaymentId}/capture?amount=xxx&serviceTaxAmount=xxx"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--verbose
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|Store identifier in the API.|GUID|36|Yes (through header)|
|`MerchantKey`|Public key for dual authentication in the API.|Text 40|Yes (through header)|
|`RequestId`|Store-defined request identifier used when the merchant uses different servers for each GET/POST/PUT.|GUID|36|No (through header)|
|`PaymentId`|Order identifier field.|GUID|36|Yes (through endpoint)|
|`Amount`|Amount to be captured, in cents. The support for partial capture must be verified with the acquirer.|Number|15|No|
|`ServiceTaxAmount`|Applicable to airlines. Amount of the authorization to be allocated to the service charge. Note: This value is not added to the authorization value.|Number|15|No|

#### Response

```json
{
    "Status": 2,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "6",
    "ProviderReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/{PaymentId}"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.braspag.com.br/v2/sales/{PaymentId}/void"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Status": 2,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "6",
    "ProviderReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/{PaymentId}"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.braspag.com.br/v2/sales/{PaymentId}/void"
        }
    ]
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`Status`|Transaction status.|Byte|2|E.g.: 1|
|`ReasonCode`|Acquirer return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Acquirer return message.|Text|512|Alphanumeric|

### Authenticating a Transaction 

With the authentication process, a risk analysis can be carried out taking more of user's and buyer's data into consideration, which helps the validation process of an online purchase.

Through Pagador, when a transaction is submitted to the authentication process, the bearer is redirected to the issuer's environment to confirm their information. If correctly validated, the risk of chargeback (dispute of a purchase made by credit or debit card) shifts to the issuer, which means the merchant is not held accountable for it.

In the mobile environment, we recommend using the [3DS 2.0](https://braspag.github.io//en/manualp/emv3ds) version for authentication.

<aside class="warning"> Important: The 3DS 1.0 version is not compatible in the mobile environment.</aside>

There are two ways of authenticating transactions with Braspag:

* **Standard** - when the merchant does not have a direct connection to an authenticator (MPI)
* **External** - when the merchant has their own authenticator (MPI)

#### Standard Authentication

In a standard authentication, as the merchant does not have a direct connection to an authenticator (MPI), the payment method redirects the customer to the authentication environment. The `Payment.Authenticate` parameter should be sent as "true", as shown below:

##### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment": {
      "Provider": "Simulado",
      "Type": "CreditCard",
      "Amount": 10000,
      "Currency": "BRL",
      "Country": "BRA",
      "Installments": 1,
      "Interest": "ByMerchant",
      "Capture": true,
      "Authenticate": true,
      "Recurrent": false,
      "SoftDescriptor": "Mensagem",
      "CreditCard": {
         "CardNumber": "4551870000000181",
         "Holder": "Nome do Portador",
         "ExpirationDate": "12/2021",
         "SecurityCode": "123",
         "Brand": "Visa",
         "SaveCard": "false",
         "Alias": ""
      },
        [...]
   }
}
```

```shell
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment": {
      "Provider": "Simulado",
      "Type": "CreditCard",
      "Amount": 10000,
      "Currency": "BRL",
      "Country": "BRA",
      "Installments": 1,
      "Interest": "ByMerchant",
      "Capture": true,
      "Authenticate": true,
      "Recurrent": false,
      "SoftDescriptor": "Mensagem",
      "CreditCard": {
         "CardNumber": "4551870000000181",
         "Holder": "Nome do Portador",
         "ExpirationDate": "12/2021",
         "SecurityCode": "123",
         "Brand": "Visa",
         "SaveCard": "false",
         "Alias": ""
      },
      [...]
   }
}
--verbose
```

|Property|Description|Type|Size|Mandatory|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Name of payment method provider.|Text|15|Yes|
|`Payment.Type`|Payment method type.|Text|100|Yes|
|`Payment.Amount`|Order amount in cents.|Number|15|Yes|
|`Payment.Installments`|Number of installments.|Number|2|Yes|
|`Payment.Authenticate`|Defines whether the customer will be directed to the issuer for card authentication. For authenticated transactions, the value must be "true". Please check with the acquirer about the availability of this feature.|Boolean|---|No (default "false")|
|`Payment.ReturnUrl`|URL to which the user will be redirected after authentication ends.|Text|1024|Yes (when `Autenticate` is "true")|
|`CreditCard.CardNumber`|Customer's card number.|Text|16|Yes|
|`CreditCard.Holder`|Name of cardholder printed on the card.|Text|25|Yes|
|`CreditCard.ExpirationDate`|Expiration date printed on the card, in the MM/YYYY format.|Text|7|Yes|
|`CreditCard.SecurityCode`|Security code printed on the back of the card.|Text|4|Yes|
|`CreditCard.Brand`|Card brand.|Text|10|Yes|

##### Response

With the default authentication, a transaction will receive the `Payment.AuthenticationUrl` parameter in addition to the default return from the authorization transaction.

```json
{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate": true,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "CardNumber":"455187******0181",
            "Holder": "Cardholder Name",
            "ExpirationDate":"12/2021",
            "SecurityCode":"123",
            "Brand":"Visa",
            "SaveCard":"false",
            "Alias":""
        [...]
    },
    "AuthenticationUrl": "https://qasecommerce.cielo.com.br/web/index.cbmp?id=9e61c78c0b0ca3e5db41fa7e31585eab",
    "AcquirerTransactionId": "10069930690009D2A47A",
    "ReturnUrl": "http://www.braspag.com.br",
    "PaymentId": "b125109f-681b-4338-8450-f3e38bc71b32",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 11:09:49",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider": "Cielo",
    "ReasonCode": 9,
    "ReasonMessage": "Waiting",
    "Status": 0,
    "ProviderReturnCode": "0",
   [...]
  }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate": true,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "CardNumber":"455187******0181",
            "Holder": "Cardholder Name",
            "ExpirationDate":"12/2021",
            "SecurityCode":"123",
            "Brand":"Visa",
            "SaveCard":"false",
            "Alias":""
        [...]
    },
    "AuthenticationUrl": "https://qasecommerce.cielo.com.br/web/index.cbmp?id=9e61c78c0b0ca3e5db41fa7e31585eab",
    "AcquirerTransactionId": "10069930690009D2A47A",
    "ReturnUrl": "http://www.braspag.com.br",
    "PaymentId": "b125109f-681b-4338-8450-f3e38bc71b32",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 11:09:49",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider": "Cielo",
    "ReasonCode": 9,
    "ReasonMessage": "Waiting",
    "Status": 0,
    "ProviderReturnCode": "0",
   [...]
  }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`AcquirerTransactionId`|Transaction ID of the payment method provider.|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of sale reference.|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code from the acquirer.|Text|300|Alphanumeric text|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date the transaction was received by Braspag.|Text|19|YYYY-MM-DD HH:mm:SS|
|`ReasonCode`|Operation return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Operation return message.|Text|512|Alphanumeric|
|`Status`|Transaction status.|Byte|2|E.g.: 1|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and issuer).|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and issuer).|Text|512|Transaction Approved|
|`AuthenticationUrl`|URL to which the holder will be redirected for authentication.|Texto|256|https://qasecommerce.cielo.com.br/web/index.cbmp?id=5f177203bf524c78982ad28f7ece5f08|

#### External Authentication

In an external authentication, the merchant who has their own authenticator does not need the payment method to redirect their customer to the authentication environment.

Add the `Payment.ExternalAuthentication` node to the default contract, as shown. This flow is supported by the **Cielo**, **Global Payments** and **Banorte** acquirers.

##### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment": {
      "Provider": "Simulado",
      "Type": "CreditCard",
      "Amount": 10000,
      "Currency": "BRL",
      "Country": "BRA",
      "Installments": 1,
      "Interest": "ByMerchant",
      "Capture": true,
      "Authenticate": true,
      "Recurrent": false,
      "SoftDescriptor": "Mensagem",
      "CreditCard": {
         "CardNumber": "4551870000000181",
         "Holder": "Nome do Portador",
         "ExpirationDate": "12/2021",
         "SecurityCode": "123",
         "Brand": "Visa",
         "SaveCard": "false",
         "Alias": ""
      },
      "ExternalAuthentication":{
      "Cavv":"AAABB2gHA1B5EFNjWQcDAAAAAAB=",
      "Xid":"Uk5ZanBHcWw2RjRCbEN5dGtiMTB=",
      "Eci":"5"
      },
      [...]
   }
}
```

```shell
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment": {
      "Provider": "Simulado",
      "Type": "CreditCard",
      "Amount": 10000,
      "Currency": "BRL",
      "Country": "BRA",
      "Installments": 1,
      "Interest": "ByMerchant",
      "Capture": true,
      "Authenticate": true,
      "Recurrent": false,
      "SoftDescriptor": "Mensagem",
      "CreditCard": {
         "CardNumber": "4551870000000181",
         "Holder": "Nome do Portador",
         "ExpirationDate": "12/2021",
         "SecurityCode": "123",
         "Brand": "Visa",
         "SaveCard": "false",
         "Alias": ""
      },
      "ExternalAuthentication":{
      "Cavv":"AAABB2gHA1B5EFNjWQcDAAAAAAB=",
      "Xid":"Uk5ZanBHcWw2RjRCbEN5dGtiMTB=",
      "Eci":"5"
      },
      [...]
   }
}
--verbose
```

|Property|Description|Type|Size|Mandatory|
|-----------|----|-------|-----------|---------|
|`Payment.ExternalAuthentication.Cavv`|Cavv value returned by external authentication mechanism.|Text|28|Yes|
|`Payment.ExternalAuthentication.Xid`|Xid value returned by the external authentication mechanism.|Text|28|Yes|
|`Payment.ExternalAuthentication.Eci`|Eci value returned by the external authentication mechanism.|Number|1|Yes|

##### Response

With the external authentication, a transaction will receive the `Payment.ExternalAuthentication` node in addition to the standard return from the authorization transaction. The node will contain the same information sent in the request.

```json
{
  [...]
  },
  "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard":"false",
      "Brand":"Visa",
    },
    "ProofOfSale": "20170510053219433",
    "AcquirerTransactionId": "0510053219433",
    "AuthorizationCode": "936403",
    "SoftDescriptor":"Message",
    "VelocityAnalysis": {
      "Id": "c374099e-c474-4916-9f5c-f2598fec2925",
      "ResultMessage": "Accept",
      "Score": 0
    },
    "PaymentId": "c374099e-c474-4916-9f5c-f2598fec2925",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-10 17:32:19",
    "CapturedAmount": 10000,
    "CapturedDate": "2017-05-10 17:32:19",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ExternalAuthentication":{
       "Cavv":"AAABB2gHA1B5EFNjWQcDAAAAAAB=",
       "Xid":"Uk5ZanBHcWw2RjRCbEN5dGtiMTB=",
       "Eci":"5",
     },
     [...]
  }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  [...]
  },
  "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard":"false",
      "Brand":"Visa",
    },
    "ProofOfSale": "20170510053219433",
    "AcquirerTransactionId": "0510053219433",
    "AuthorizationCode": "936403",
    "SoftDescriptor":"Message",
    "VelocityAnalysis": {
      "Id": "c374099e-c474-4916-9f5c-f2598fec2925",
      "ResultMessage": "Accept",
      "Score": 0
    },
    "PaymentId": "c374099e-c474-4916-9f5c-f2598fec2925",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-10 17:32:19",
    "CapturedAmount": 10000,
    "CapturedDate": "2017-05-10 17:32:19",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ExternalAuthentication":{
       "Cavv":"AAABB2gHA1B5EFNjWQcDAAAAAAB=",
       "Xid":"Uk5ZanBHcWw2RjRCbEN5dGtiMTB=",
       "Eci":"5",
     },
     [...]
  }
}
```

|Property|Type|Size|Description|
|-----------|----|-------|-----------|---------|
|`Payment.ExternalAuthentication.Cavv`|Text|28|Cavv value submitted in authorization request|
|`Payment.ExternalAuthentication.Xid`|Text|28|Xid value submitted in authorization request|
|`Payment.ExternalAuthentication.Eci`|Number|1|ECI value submitted in authorization request|

### Canceling/Refunding a Transaction

The availability of the refunding service varies depending on the acquirer. Each acquirer has its own deadlines to allow the refund of a transaction. In [this article](https://suporte.braspag.com.br/hc/pt-br/articles/360028661812), written in Portuguese, you can check each of them.

To cancel a credit card transaction, you must send an HTTP message through the PUT method to the *Payment* resource, as in the example:

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/sales/{PaymentId}/void?amount=xxx</span></aside>

```shell
--request PUT "https://apisandbox.braspag.com.br/v2/sales/{PaymentId}/void?amount=xxx"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--verbose
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|Store identifier in the API.|GUID|36|Yes (through header)|
|`MerchantKey`|Public key for dual authentication in the API.|Text|40|Sim (through header)|
|`RequestId`|Store-defined request identifier used when the merchant uses different servers for each GET/POST/PUT.|GUID|36|No (through header)|
|`PaymentId`|Order identifier field.|GUID|36|Yes (through endpoint)|
|`Amount`|Amount to be canceled/refunded, in cents. Check if your acquirer supports the canceling or refunding operations.|Number|15|No (through endpoint)|

#### Response

```json
{
    "Status": 10,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "9",
    "ProviderReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/{PaymentId}"
        }
    ]
}
```

```shell
{
    "Status": 10,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "9",
    "ProviderReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/{PaymentId}"
        }
    ]
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`Status`|Transaction status.|Byte|2|E.g.: 1|
|`ReturnCode`|Acquirer's return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Acquirer's return message.|Text|512|Alphanumeric|

### Analyzing with Velocity Check

*Velocity Check* is a fraud-fighting tool that prevents massive bursts of transactions with repeated payment data. It analyzes the frequency of traceability elements such as Card Number, Social Security Number, Zip Code, among others, and blocks suspicious transactions.

The functionality must be contracted separately and then enabled in your store via dashboard. When Velocity is active, the transaction response brings in the `Velocity` node, with details of the analysis.

In case of a Velocity rule rejection, *ProviderReasonCode* will be "BP 171 - Rejected by fraud risk" (Velocity, with "ReasonCode 16 - AbortedByFraud").

#### Response

```json
{
    [...]
    "VelocityAnalysis": {
        "Id": "2d5e0463-47be-4964-b8ac-622a16a2b6c4",
        "ResultMessage": "Reject",
        "Score": 100,
        "RejectReasons": [
        {
            "RuleId": 49,
            "Message": "Blocked by the CardNumber rule. Name: Maximum 3 Card Hits in 1 day. HitsQuantity: 3. HitsTimeRangeInSeconds: 1440. ExpirationBlockTimeInSeconds: 1440"
        }]
    [...]
  }
}
```

```shell
{
    [...]
    "VelocityAnalysis": {
        "Id": "2d5e0463-47be-4964-b8ac-622a16a2b6c4",
        "ResultMessage": "Reject",
        "Score": 100,
        "RejectReasons": [
        {
            "RuleId": 49,
            "Message": "Blocked by the CardNumber rule. Name: Maximum 3 Card Hits in 1 day. HitsQuantity: 3. HitsTimeRangeInSeconds: 1440. ExpirationBlockTimeInSeconds: 1440"
        }]
    [...]
  }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`VelocityAnalysis.Id`|Identification of the analysis performed.|GUID|36|
|`VelocityAnalysis.ResultMessage`|Analysis result ("Accept" / "Reject").|Text|25|
|`VelocityAnalysis.Score`|Number of points given to the operation. E.g.: 100.|Number|10|
|`VelocityAnalysis.RejectReasons.RuleId`|Rejection rule code.|Number|10|
|`VelocityAnalysis.RejectReasons.Message`|Description of the rejection rule.|Text|512|

### Using DCC (Currency Converter)

The Dynamic Currency Conversion (DCC) is a currency converter from the **Global Payments** acquirer that allows a foreign cardholder to choose between paying in reais or in their local currency, converting the order amount at the time of purchase with full transparency for the customer.
The solution is suitable for establishments such as hotels, inns, shopping centers and tourist shops, that receive payments with cards issued abroad.

<aside class="notice">To use this feature using the standard authentication functionality, the merchant must contact the Global Payments acquirer and request a DCC activation at their store.</aside>

<aside class="warning">This feature is not compatible with external MPI transactions.</aside>

When the establishment has DCC product enabled, the authorization process is performed in the 3 steps described below:

#### STEP 1 - Authorization

In the first step, when applying for an authorization with an international card, Global Payments identifies the card's country and applies the currency conversion following the brand-specific calculations, and then returns the conversion information.

##### Request

There is no difference between a standard authorization request and a DCC authorization request.

##### Response

```json
{
    [...]
    },
    "Payment":{
        "ServiceTaxAmount": 0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "CreditCard":{
            "CardNumber": "123412******1234",
            "Holder": "Shopper Test",
            "ExpirationDate": "12/2022",
            "SaveCard":"false",
            "Brand":"Visa",
        },
        "ReturnUrl": "http://www.braspag.com.br/",
        "PaymentId": "fa0c3119-c730-433a-123a-a3b6dfaaad67",
        "Type":"CreditCard",
        "Amount": 100,
        "ReceivedDate": "2018-08-23 10:46:25",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider": "GlobalPayments",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 12,
        "ProviderReturnCode": "0",
        "ProviderReturnMessage": "Authorized Transaction",
        "CurrencyExchangeData": {
            "Id": "fab6f3a752d700af1d50fdd19987b95df497652b",
            "CurrencyExchanges": [{
                    "Currency": "EUR",
                    "ConvertedAmount": 31,
                    "ConversionRate": 3.218626,
                    "ClosingDate": "2017-03-09T00:00:00"
                },
                {
                    "Currency":"BRL",
                    "ConvertedAmount": 100
                }
            ]
        },
        [...]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    [...]
    },
    "Payment":{
        "ServiceTaxAmount":0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "CreditCard":{
            "CardNumber": "123412******1234",
            "Holder": "Shopper Test",
            "ExpirationDate": "12/2022",
            "SaveCard":"false",
            "Brand":"Visa",
        },
        "ReturnUrl": "http://www.braspag.com.br/",
        "PaymentId": "fa0c3119-c730-433a-123a-a3b6dfaaad67",
        "Type":"CreditCard",
        "Amount": 100,
        "ReceivedDate": "2018-08-23 10:46:25",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider": "GlobalPayments",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 12,
        "ProviderReturnCode": "0",
        "ProviderReturnMessage": "Authorized Transaction",
        "CurrencyExchangeData": {
            "Id": "fab6f3a752d700af1d50fdd19987b95df497652b",
            "CurrencyExchanges": [{
                    "Currency": "EUR",
                    "ConvertedAmount": 31,
                    "ConversionRate": 3.218626,
                    "ClosingDate": "2017-03-09T00:00:00"
                },
                {
                    "Currency":"BRL",
                    "ConvertedAmount": 100
                }
            ]
        }
        [...]
}
```

|Property|Description|Type|Size|Format|
|-------------------------|-----------------------------------------------------------------------------|-------|---------|--------------------------------------|
|`AcquirerTransactionId`|Transaction ID of the payment method provider.|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of sale number.|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code.|Text|300|Alphanumeric|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date the transaction was received by Braspag.|Text|19|YYYY-MM-DD HH:mm:SS|
|`ReasonCode`|Operation return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Operation return message.|Text|512|Alphanumeric|
|`Status`|Transaction status.|Byte|2|E.g.: 12|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and issuer).|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and issuer).|Text|512|Transaction Approved|
|`CurrencyExchangeData.Id`|ID of the currency exchange action.|Text|50|1b05456446c116374005602dcbaf8db8879515a0|
|`CurrencyExchangeData.CurrencyExchanges.Currency`|Customer's local currency/credit card.|Numeric|4|EUR|
|`CurrencyExchangeData.CurrencyExchanges.ConvertedAmount`|Converted value.|Numeric|12|23|
|`CurrencyExchangeData.CurrencyExchanges.ConversionRate`|Conversion rate.|Numeric|9|3.218626|
|`CurrencyExchangeData.CurrencyExchanges.ClosingDate`|Transaction end date.|Texto|19|AAAA-MM-DD HH:mm:SS|
|`CurrencyExchangeData.CurrencyExchanges.Currency`|Real currency code.|Text|3|BRA|
|`CurrencyExchangeData.CurrencyExchanges.ConvertedAmount`|Order value in reais.|Numeric|12|100|

#### STEP 2 - Payment Options

In the second step, the store system should present the customer with the options of paying in reais or in their country's currency (credit card currency), following the best practices requested by the brand. The text is presented in English and the website layout does not need to be changed, as long as the currency selection options follow the same font, color and dimension characteristics.

In Global Payments, the payment options (in reais or in the card currency) are displayed on the screen, right beside a summary of the purchase.

#### STEP 3 - Confirmation

In the third step, the store system sends the transaction confirmation with the information of the currency chosen by the customer. At this point, the authorization response is returned.

##### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/{PaymentId}/confirm</span></aside>

```json
{
  "Id": "1b05456446c116374005602dcbaf8db8879515a0",
  "Currency": "EUR",
  "Amount": 23
}
```

```shell
--request POST " https://apisandbox.braspag.com.br/v2/sales/{PaymentId}/confirm"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  "Id": "1b05456446c116374005602dcbaf8db8879515a0",
  "Currency": "EUR",
  "Amount": 23
}
--verbose
```

|Property|Description|Type|Size|Mandatory|
|-----------|----|-------|-----------|---------|
|`Id`|ID of the currency exchange action.|Text|50|Yes|
|`Currency`|Customer's selected currency.|Numeric|4|Yes|
|`Amount`|Converted value.|Numeric|12|Yes|

##### Response

```json
{
   [...]
   "Payment":{
        "ServiceTaxAmount": 0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture": false,
        "Authenticate":false,
        "Recurrent":false,
        "CreditCard":{
            "CardNumber": "123412******1234",
            "Holder": "TestDcc",
            "ExpirationDate": "12/2022",
            "SecurityCode": "***",
            "Brand":"Visa",
        },
        "ProofOfSale": "20170510053219433",
        "AcquirerTransactionId": "0510053219433",
        "AuthorizationCode": "936403",
        "SoftDescriptor":"Message",
        "PaymentId": "fa0c3119-c730-433a-123a-a3b6dfaaad67",
        "Type":"CreditCard",
        "Amount": 23,
        "ReceivedDate": "2017-05-10 17:32:19",
        "CapturedAmount": 23,
        "CapturedDate": "2017-05-10 17:32:19",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider": "GlobalPayments",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 2,
        "ProviderReturnCode": "6",
        "ProviderReturnMessage": "Operation Successful",
        [...]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   [...]
   "Payment":{
        "ServiceTaxAmount": 0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture": false,
        "Authenticate":false,
        "Recurrent":false,
        "CreditCard":{
            "CardNumber": "123412******1234",
            "Holder": "TestDcc",
            "ExpirationDate": "12/2022",
            "SecurityCode": "***",
            "Brand":"Visa",
        },
        "ProofOfSale": "20170510053219433",
        "AcquirerTransactionId": "0510053219433",
        "AuthorizationCode": "936403",
        "SoftDescriptor":"Message",
        "PaymentId": "fa0c3119-c730-433a-123a-a3b6dfaaad67",
        "Type":"CreditCard",
        "Amount": 23,
        "ReceivedDate": "2017-05-10 17:32:19",
        "CapturedAmount": 23,
        "CapturedDate": "2017-05-10 17:32:19",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider": "GlobalPayments",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 2,
        "ProviderReturnCode": "6",
        "ProviderReturnMessage": "Operation Successful",
        [...]
    }
}
```

|Property|Description|Type|Size|Format|
|-------------------------|-----------------------------------------------------------------------------|-------|---------|--------------------------------------|
|`AcquirerTransactionId`|Transaction ID of the payment method provider.|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of sale number.|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code.|Text|300|Alphanumeric|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date the transaction was received by Braspag.|Text|19|YYYY-MM-DD HH:mm:SS|
|`ReasonCode`|Operation return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Operation return message.|Text|512|Alphanumeric|
|`Status`|Transaction status.|Byte|2|E.g.: 12|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and issuer).|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and issuer).|Text|512|Transaction Approved|

## QR Code Transaction

To create a QR code transaction you must submit a request using the POST method as shown below. This request will create the transaction, which will receive the *Pending* status in Braspag, and generate the QR code for the payment. The customer makes the payment through one of the supported applications and the transaction changes status (e.g.: to *Pago* when paid, *Não pago* when not paid, or *Não autorizado* for unauthorized transactions).
The example below covers the minimum required fields to be submitted for authorization:

### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{
 "MerchantOrderId": "20191123",
 "Customer":{
  "Name":"QRCode Test"
  },
 "Payment":{
   "Provider":"Cielo30",
   "Type":"qrcode",
   "Amount":100,
   "Installments":1,
   "Capture": false
   }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
 "MerchantOrderId": "20191023",
 "Customer":{
  "Name":"QRCode Test"
  },
 "Payment":{
   "Provider":"Cielo30",
   "Type":"qrcode",
   "Amount": 500,
   "Installments":1,
   "Capture": false
   }
}
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantOrderId`|Order ID number.|Text|50|Yes|
|`Customer.Name`|Customer's name.|Text|255|No|
|`Payment.Provider`|Name of payment method provider. Currently only available for "Cielo".|Text|15|Yes|
|`Payment.Type`|Payment method type. In this case, "qrcode".|Text|100|Yes|
|`Payment.Amount`|Order amount (greater than 0) in cents.|Number|15|Yes|
|`Payment.Installments`|Number of installments.|Number|2|Yes|
|`Payment.Capture`|Submit "true" for a transaction with auto capture.|Boolean|-|No|

### Response

```json
{
 "MerchantOrderId": "20191023",
 "Customer":{
  "Name": "QRCode Test"
 },
 "Payment": {
  "Installments":1,
  "Capture": false,
  "AcquirerTransactionId": "52d641fb-2880-4024-89f4-7b452dc5d9cd",
  "QrCodeBase64Image": "iVBORw0KGgoAAAA(...)",
  "PaymentId": "403dba6-23e3-468b-92f8-f9af56d3b9d7",
  "Type": "QrCode",
  "Amount": 100,
  "ReceivedDate": "2019-10-23 21:30:00",
  "Currency":"BRL",
  "Country":"BRA",
  "Provider": "Cielo30",
  "ReasonCode": 0,
  "ReasonMessage": "Successful",
  "Status": 12,
  "ProviderReturnCode": "0",
  "ProviderReturnMessage": "Successfully generated QRCode",
  "Links": [
   {
    "Method": "GET",
    "Rel": "self",
    "Href": "http://apiquerysandbox.braspag.com.br/v2/sales/4031dba6-23e3-468b-92f8-f9af56d3b9d7"
   }
  ]
 }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
 "MerchantOrderId": "20191023",
 "Customer":{
  "Name": "QRCode Test"
 },
 "Payment":{
  "Installments":1,
  "Capture": false,
  "AcquirerTransactionId": "52d641fb-2880-4024-89f4-7b452dc5d9cd",
  "QrCodeBase64Image": "iVBORw0KGgoAAAA(...)",
  "PaymentId": "403dba6-23e3-468b-92f8-f9af56d3b9d7",
  "Type": "QrCode",
  "Amount": 100,
  "ReceivedDate": "2019-10-23 21:30:00",
  "Currency":"BRL",
  "Country":"BRA",
  "Provider": "Cielo30",
  "ReasonCode": 0,
  "ReasonMessage": "Successful",
  "Status": 12,
  "ProviderReturnCode": "0",
  "ProviderReturnMessage": "Successfully generated QRCode",
  "Links": [
   {
    "Method": "GET",
    "Rel": "self",
    "Href": "http://apiquerysandbox.braspag.com.br/v2/sales/4031dba6-23e3-468b-92f8-f9af56d3b9d7"
   }
  ]
 }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`QrCodeBase64Image`|Base64 encoded QR code. The QR code image can be displayed on the page through an HTML code as follows:<br><br> &lt;img src="data:image/png;base64,{código da imagem em base 64}"&gt;.|Text|Variable|Alphanumeric text|
|`PaymentId`|Order identifier field, required for operations such as query, capture and cancellation.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`Status`|Transaction status. For transactions with QR Code, the initial status is "12" (*Pending*).|Byte|-|2|
|`ReturnCode`|Acquirer's return code.|Text|32|Alphanumeric|
|`ReturnMessage`|Acquirer's return message.|Text|512|Alphanumeric|

## Boleto

### Registered Boleto

FEBRABAN, the Brazilian Federation of Banks, together with the banking network, launched the **New Payment Slip-Registered Collections Platform**. This new system was created in order to promote greater control and security to the boleto transactional in e-commerce, and to ensure more reliability and convenience to users.

Since July 21, 2018, all boletos issued in e-commerce must be registered. [Click here](https://portal.febraban.org.br/pagina/3150/1094/en-us/new-payment-platform) to access the full announcement (available only in Portuguese).

Here is a list of the migration/membership procedures (in Portuguese) for each bank:

* [Bradesco](https://gallery.mailchimp.com/365fc3ca5e4f598460f07ecaa/files/24157160-4da2-46d4-a119-60d8f614a842/Procedimento_de_Migra%C3%A7%C3%A3o_Boleto_Registrado_Bradesco.pdf)
* [Banco do Brasil](https://gallery.mailchimp.com/365fc3ca5e4f598460f07ecaa/files/0f4644c6-da10-42ab-b647-09786d5db5cb/Procedimento_de_Migra%C3%A7%C3%A3o_Boleto_Registrado_Banco_do_Brasil.pdf)
* [Itaú](https://gallery.mailchimp.com/365fc3ca5e4f598460f07ecaa/files/de2e95e8-441a-4fa2-be01-9b89463477d0/Procedimento_de_Migra%C3%A7%C3%A3o_Boleto_Registrado_Ita%C3%BA_v1.1.pdf)
* [Santander](https://gallery.mailchimp.com/365fc3ca5e4f598460f07ecaa/files/a8661c34-6341-466a-86cf-078fb5e19626/Procedimento_de_Migra%C3%A7%C3%A3o_Boleto_Registrado_Santander.pdf)
* [Caixa Econômica](https://gallery.mailchimp.com/365fc3ca5e4f598460f07ecaa/files/fee80b87-2b37-4f19-b293-bb43389025de/Procedimento_de_Migra%C3%A7%C3%A3o_Boleto_Registrado_Caixa_v1.1.pdf)

### Creating a Boleto Transaction

To generate boletos, customer's data such as ID number (CPF or CNPJ) and address must be provided. Below is an example of how to create a boleto as the payment method.

The `Payment.FineRate` and `Payment.FineAmount` parameters must not be used together. The same rule applies to the `Payment.InterestRate` and `Payment.InterestAmount` parameters. They are all marked with an "\*" in the "MANDATORY" column,

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{
    "MerchantOrderId":"2017091101",
    "Customer":
    {
        "Name": "shopper Name",
        "Identity": "12345678909",
        "IdentityType": "CPF",
        "Address":{
            "Street":"Alameda Xingu",
            "Number":"512",
            "Complement":"27th floor",
            "ZipCode":"12345987",
            "City":"São Paulo",
            "State":"SP",
            "Country":"BRA",
            "District":"Alphaville"
        }
    },
    "Payment":
    {
     "Provider":"Simulado",
     "Type": "Boleto",
     "Amount":10000,
     "BoletoNumber":"2017091101",
     "Assignor": "Test Company",
     "Demonstrative": "Desmonstrative Test",
     "ExpirationDate": "2017-12-31",
     "Identification": "12346578909",
     "Instructions": "Accept only until the due date.",
     "DaysToFine": 1,
     "FineRate": 10.00000,
     "FineAmount": 1000,
     "DaysToInterest": 1,
     "InterestRate": 5.00000,
     "InterestAmount": 500
    }
}
```

```shell
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId":"2017091101",
    "Customer":
    {
        "Name": "shopper Name",
        "Identity": "12345678909",
        "IdentityType": "CPF",
        "Address":{
            "Street":"Alameda Xingu",
            "Number":"512",
            "Complement":"27th floor",
            "ZipCode":"12345987",
            "City":"São Paulo",
            "State":"SP",
            "Country":"BRA",
            "District":"Alphaville"
        }
    },
    "Payment":
    {
     "Provider":"Simulado",
     "Type": "Boleto",
     "Amount":10000,
     "BoletoNumber":"2017091101",
     "Assignor": "Test Company",
     "Demonstrative": "Desmonstrative Test",
     "ExpirationDate": "2017-12-31",
     "Identification": "12346578909",
     "Instructions": "Accept only until the due date.",
     "DaysToFine": 1,
     "FineRate": 10.00000,
     "FineAmount": 1000,
     "DaysToInterest": 1,
     "InterestRate": 5.00000,
     "InterestAmount": 500
    }
}
--verbose
```

|Property|Description|Type|Size|Mandatory|
|-----------|----|-------|-----------|---------|
|`MerchantId`|Store identifier at Braspag.|GUID|36|Yes (through header)|
|`MerchantKey`|Public key for dual authentication at Braspag.|Text|40|Yes (through header)|
|`RequestId`|Store-defined request identifier used when the merchant uses different servers for each GET/POST/PUT.|GUID|36|No (through header)|
|`MerchantOrderId`|Order ID number. Rule varies according to the provider used (see table annexed).|Text|see table annexed|Yes|
|`Customer.Name`|Customer's name. The rule varies according to the provider used (see table annexed).|Text|see table annexed|Yes|
|`Customer.Identity`|Customer ID like RG, CPF or CNPJ number.|Text|14|No|
|`Customer.IdentityType`|Customer's ID document Type (CPF or CNPJ).|Text|255|No|
|`Customer.Address.Street`|Customer's contact address. The rule varies according to the provider used (see table annexed).|Text|see table annexed|Yes|
|`Customer.Address.Number`|Customer's contact address number. The rule varies according to the provider used (see table annexed).|Text|see table annexed|Yes|
|`Customer.Address.Complement`|Customer's contact address complement. The rule varies according to the provider used (see table annexed).|Text|see table annexed|No|
|`Customer.Address.ZipCode`|Customer's contact address zip code.|Text|8|No|
|`Customer.Address.District`|Customer's contact address district. The rule varies according to the provider used (see table annexed).|Text|see table annexed|Yes|
|`Customer.Address.City`|Customer's contact address city. The rule varies according to the provider used (see table annexed).|Text|see table annexed|Yes|
|`Customer.Address.State`|Customer's contact address state.|Text|2|No|
|`Customer.Address.Country`|Customer's contact address country.|Text|35|No|
|`Payment.Provider`|Name of payment method provider. See table annexed to access the list of providers.|Text|15|Yes|
|`Payment.Type`|Payment method type. In this case, "Boleto".|Text|100|Yes|
|`Payment.Amount`|Order amount in cents.|Number|15|Yes|
|`Payment.BoletoNumber`|Boleto number ("Nosso Número"). If filled, overrides the value set on the payment method. The rule varies according to the provider used (see table annexed).|Text|see table annexed|No|
|`Payment.Assignor`|Name of the assignor. If filled, overrides the value set on the payment method.|Text|200|No|
|`Payment.Demonstrative`|Statement text. If filled, overrides the value set on the payment method. The rule varies according to the provider used (see table annexed).|Text|see table annexed|No|
|`Payment.ExpirationDate`|Boleto's expiration date. If you are not previously registered with the payment method, this field is required. If sent on request, overrides the value set on the payment method.|Date|YYYY-MM-DD|No|
|`Payment.Identification`|Assignor's CNPJ. If filled, overrides the value set on the payment method.|Text|14|No|
|`Payment.Instructions`|Boleto's instructions. If filled, overrides the value set on the payment method. The rule varies according to the provider used (see table annexed). To break lines in this text always use the `<br>` HTML tag.|Text|see table annexed|No|
|`Payment.NullifyDays`|Deadline for automatic annulment of the boleto. After the number of days set in this field from the due date, the boleto will be automatically canceled. E.g.: a boleto due on Dec 15th which has 5 "nullify days" can be paid until Dec 20th; after this date the title is canceled. Note: Feature valid only for Banco Santander registered boletos.|Number|2|No|
|`Payment.DaysToFine`|Optional and only applicable for Bradesco2 provider. Number of days (an integer) after the due date to charge a fine amount. E.g.: 3.|Number|15|No|
|`Payment.FineRate`|Optional and only applicable for Bradesco2 provider. Amount of fine (in percentage) after due date, based on the boleto total amount (%). Permits decimal with up to 5 decimal places. Do not submit if using `FineAmount`. E.g.: 1012345 = 10.12345%.|Number|15|No*|
|`Payment.FineAmount`|Optional and only applicable for Bradesco2 provider. Amount of fine (in cents) after due date in absolute value. Do not submit if using `FineRate`. E.g.: 1000 = R$ 10.00.|Number|15|No*|
|`Payment.DaysToInterest`|Optional and only applicable for Bradesco2 provider. Number of days (an integer) after due date to start charging daily interest based on the boleto total amount. E.g.: 3.|Number|15|No|
|`Payment.InterestRate`|Optional and only applicable for Bradesco2 provider. Monthly interest amount (in percentage) after due date , based on the boleto total amount (%). Interest is charged pro rata per day (monthly divided by 30). Permits decimal with up to 5 decimal places. Do not submit if using `InterestAmount`. E.g.: 10.12345.|Number|15|No*|
|`Payment.InterestAmount`|Optional and only applicable for Bradesco2 provider. Absolute value (in cents) for daily interest after due date. Do not submit if using `InterestRate`. E.g.: 1000 = R$ 10.00.|Number|15|No*|

#### Response

```json
{
  "MerchantOrderId": "2017091101",
  "Customer":{
    "Name":"Shopper Name",
    "Identity":"12345678909",
    "IdentityType":"CPF",
    "Address":{
      "Street":"Alameda Xingu",
      "Number":"512",
      "Complement":"27th floor",
      "ZipCode":"12345987",
      "City":"São Paulo",
      "State":"SP",
      "Country": "BRA"
    }
  },
  "Payment":{
    "Instructions": "Accept only until the due date.",
    "ExpirationDate": "2017-12-31",
    "Demonstrative": "Desmonstrative Test",
    "Url": "https://homologacao.pagador.com.br/post/pagador/reenvia.asp/d24b0aa4-21c9-449d-b85c-6279333f070f",
    "BoletoNumber": "2017091101",
    "BarCodeNumber": "00091739000000100000494250000000263400656560",
    "DigitableLine": "00090.49420 50000.000260 34006.565609 1 73900000010000",
    "Assignor": "Test Company",
    "Address": "Av. Brigadeiro Faria Lima, 160",
    "Identification": "12346578909",
    "PaymentId": "d24b0aa4-21c9-449d-b85c-6279333f070f",
    "Type": "Boleto",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 16:42:55",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "InterestAmount": 1,
    "FineAmount": 5,
    "DaysToFine": 1,
    "DaysToInterest": 1,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/d24b0aa4-21c9-449d-b85c-6279333f070f"
      }
    ]
  }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  "MerchantOrderId": "2017091101",
  "Customer":{
    "Name":"Shopper Name",
    "Identity":"12345678909",
    "IdentityType":"CPF",
    "Address":{
      "Street":"Alameda Xingu",
      "Number":"512",
      "Complement":"27th floor",
      "ZipCode":"12345987",
      "City":"São Paulo",
      "State":"SP",
      "Country":"BRA",
     "District":"Alphaville"
    }
  },
  "Payment":{
    "Instructions": "Accept only until the due date.",
    "ExpirationDate": "2017-12-31",
    "Demonstrative": "Desmonstrative Test",
    "Url": "https://homologacao.pagador.com.br/post/pagador/reenvia.asp/d24b0aa4-21c9-449d-b85c-6279333f070f",
    "BoletoNumber": "2017091101",
    "BarCodeNumber": "00091739000000100000494250000000263400656560",
    "DigitableLine": "00090.49420 50000.000260 34006.565609 1 73900000010000",
    "Assignor": "Test Company",
    "Address": "Av. Brigadeiro Faria Lima, 160",
    "Identification": "12346578909",
    "PaymentId": "d24b0aa4-21c9-449d-b85c-6279333f070f",
    "Type": "Boleto",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 16:42:55",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "InterestAmount": 1,
    "FineAmount": 5,
    "DaysToFine": 1,
    "DaysToInterest": 1,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/d24b0aa4-21c9-449d-b85c-6279333f070f"
      }
    ]
  }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ExpirationDate`|Expiration date.|Text|10|2014-12-25|
|`Url`|Boleto URL generated.|string|256|https://.../pagador/reenvia.asp/8464a692-b4bd-41e7-8003-1611a2b8ef2d|
|`BoletoNumber`|"NossoNumero" generated.|Text|50|2017091101|
|`BarCodeNumber`|Numerical representation of the barcode.|Text|44|00091628800000157000494250100000001200656560|
|`DigitableLine`|Digitable line.|Texto|256|00090.49420 50100.000004 12006.565605 1 62880000015700|
|`Address`|Store address registered in the issuer.|Text|256|E.g.: Av. Teste, 160|
|`Status`|Transaction status. See status list annexed.|Byte|2|E.g.: 1|

### Boleto Conciliation

In order to update the status of a Boleto to *Pago* (meaning paid), the Pagador must receive CNAB files with the related settlements from the banks. To enable your store to receive bank files, simply follow the procedure described on [this article](https://suporte.braspag.com.br/hc/pt-br/articles/360007068352-Como-funciona-a-Concilia%C3%A7%C3%A3o-via-Nexxera-) (written in Portuguese).

### Specific Rules by Issuer 

The following is a list of properties and their size specifications, related to different rules for each issuer and their respective providers:

|Property|Bradesco|Banco do Brasil|Itaú Shopline|Santander|Caixa Econômica|Citibank|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|`Provider`|Bradesco2|BancoDoBrasil2|ItauShopline|Santander2|Caixa2|Citibank2|
|`MerchantOrderId`|27 (`*1`)|50|8|50|11 (`*2`)|10 (`*2`)|
|`Payment.BoletoNumber`|11 (`*3`)|9 (`*4`)|8 (`*5`)|13 (`*3`)|12 (`*6`)|11 (`*7`)|
|`Customer.Name`|34|60 (`*8`)|30|40|40|50 (`*9`)|
| `Customer.Address.Street`; `Customer.Address.Number`; `Customer.Address.Complement`; `Customer.Address.District` | Street: 70<br><br>Number: 10<br><br>Complement: 20<br><br>District: 50 | Total up to 60 characters (`*8`) | Street, Number and Complement must total up to 40 characters<br><br>District: 15 | Street, Number and Complement must total up to 40 characters<br><br>District: 15 | Street, Number e Complement must total up to 40 characters<br><br>District: 15 | Street, Number e Complement must total up to 40 characters<br><br>District: 50 (`*9`) |
|`Customer.Address.City`|50|18 (`*8`)|15|30|15|50 (`*9`)|
|`Payment.Instructions`|450|450|N/A|450|450|450|
|`Payment.Demonstrative`|255|N/A|N/A|255|255|255|

|Remarks|Details|
|---|---|
|`*1`|Only letters, numbers, and characters like "\_" and "$".|
|`*2`|If it exceeds 11 digits, the API will generate an incremental number from the defined configuration.|
|`*3`|The value must be unique, i.e., the bank does not allow repeating previously used values.|
|`*4`|When submitted with over 9 positions, the API considers the last 9 digits.|
|`*5`|Must always be the same as the order number (`MerchantOrderId`).|
|`*6`|The API automatically concatenates the value “14” + 12 free digits + check digit, before sending it to the bank. If the total exceeds 14 digits, the API considers the last 14 digits.|
|`*7`|When the number sent exceeds the maximum number, the API generates a random number.|
|`*8`|The following are accepted as valid characters: numbers, letters A to Z (UPPERCASE) and conjunction special characters (hyphen "-" and apostrophe "'". When used, there must be no space between letters. Correct examples: D'EL-REI / D'ALCORTIVO / SANT'ANA. Incorrect examples: D'EL - REI / a space between words.|
|`*9`|Special characters and diacritics will be removed automatically.|

## Electronic Transfer

Similar to a debit card payment, an electronic transfer connects the customer to their issuer in order to authenticate a debit sale. The difference between them is that transfers are neither submitted to the acquirer nor depend on card data.

### Creating a Transaction

To create a sale, you must send an HTTP message through the POST method to the *Payment* resource, according to the example:

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{
    "MerchantOrderId": "2017051109",
    "Customer":
    {
        "Name": "Shopper Name",
        "Identity":"12345678909",
        "IdentityType":"CPF",
        "Email": "shopper@braspag.com.br",
        "Address":
        {
             "Street":"Alameda Xingu",
             "Number":"512",
             "Complement":"27th floor",
             "ZipCode":"12345987",
             "City":"São Paulo",
             "State":"SP",
             "Country":"BRA",
             "District":"Alphaville"
         }
  },
    "Payment":
    {
        "Provider":"Bradesco",
        "Type":"EletronicTransfer",
        "Amount":10000,
        "ReturnUrl":"http://www.braspag.com.br",
        "Beneficiary":
            {
            "Bank":"Bradesco"
        },
        "Shopper":{
            "Branch":"1669",
            "Account":"19887-5"
        }
    }
}
```

```shell
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2017051109",
    "Customer":
    {
        "Name": "shopper Name",
        "Identity":"12345678909",
        "IdentityType":"CPF",
        "Email": "shopper@braspag.com.br",
        "Address":
        {
             "Street":"Alameda Xingu",
             "Number":"512",
             "Complement":"27th floor",
             "ZipCode":"12345987",
             "City":"São Paulo",
             "State":"SP",
             "Country":"BRA",
             "District":"Alphaville"
        }
    },
    "Payment":
    {
        "Provider":"Bradesco",
        "Type":"EletronicTransfer",
        "Amount":10000,
        "ReturnUrl":"http://www.braspag.com.br",
        "Beneficiary":
            {
            "Bank":"Bradesco"
        },
        "Shopper":{
            "Branch":"1669",
            "Account":"19887-5"
        }
    }
}
--verbose
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|Store identifier in the API.|GUID|36|Yes (through header)|
|`MerchantKey`|Public key for dual authentication in the API.|Text|40|Sim (through header)|
|`RequestId`|Store-defined request identifier used when the merchant uses different servers for each GET/POST/PUT.|GUID|36|No (through header)|
|`MerchantOrderId`|Order ID number.|Text|50|Yes|
|`Customer.Name`|Customer's name.|Text|255|Yes|
|`Customer.Identity`|Customer's ID, CPF or CNPJ number.|Text|14|Yes|
|`Customer.IdentityType`|Customer's ID document type (CPF or CNPJ).|Text|255|No|
|`Customer.Email`|Customer's email address.|Text|255|No|
|`Customer.Address.Street`|Customer's contact address.|Text|255|Yes|
|`Customer.Address.Number`|Customer's contact address number.|Text|15|No|
|`Customer.Address.Complement`|Customer's contact address additional information.|Text|50|No|
|`Customer.Address.ZipCode`|Customer's contact address zip code.|Text|8|No|
|`Customer.Address.City`|Customer's contact address city.|Text|50|Yes|
|`Customer.Address.State`|Customer's contact address state.|Text|2|Yes|
|`Customer.Address.Country`|Customer's contact address country.|Text|35|Yes|
|`Customer.Address.District`|Customer's contact address neighborhood.|Text|35|Yes|
|`Payment.Type`|Payment method type.|Text|100|Yes|
|`Payment.Amount`|Order amount in cents.|Number|15|Yes|
|`Payment.Provider`|Name of payment method provider.|Text|15|Yes|
|`Payment.Beneficiary.Bank`|Payer's bank (required only for electronic transfers with PayMeeSemiTransparent).|Text|100|Conditional|
|`Payment.Shopper.Branch`|Payer's agency (required only for electronic transfers with PayMeeSemiTransparent). Note: Suppress this node for *Depósito Identificado*.|Text|100|Conditional|
|`Payment.Shopper.Account`|Payer's account (required only for electronic transfers with PayMeeSemiTransparent). Note: Suppress this node for *Depósito Identificado*.|Text|100|Conditional|

#### Response

```json
{
    "MerchantOrderId": "2017051109",
    "Customer":{
        "Name": "Shopper Name",
        "Identity":"12345678909",
        "IdentityType":"CPF",
        "Email": "shopper@braspag.com.br",
        "Address":
        {"Street":"Alameda Xingu",
        "Number":"512",
        "Complement":"27th floor",
        "ZipCode":"12345987",
        "City":"São Paulo",
        "State":"SP",
             "Country":"BRA",
             "District":"Alphaville"
            }
    },
    "Payment":{
        "Url": "https://xxx.xxxxxxx.xxx.xx/post/EletronicTransfer/Redirect/{PaymentId}",
        "PaymentId": "765548b6-c4b8-4e2c-b9b9-6458dbd5da0a",
        "Type": "EletronicTransfer",
        "Amount":10000,
        "ReceivedDate": "2015-06-25 09:37:55",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider": "Bradesco",
        "ReturnUrl": "http://www.braspag.com.br",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 0,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/{PaymentId}"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2017051109",
    "Customer":{
        "Name": "Shopper name",
    },
    "Payment":{
        "Url": "https://xxx.xxxxxxx.xxx.xx/post/EletronicTransfer/Redirect/{PaymentId}",
        "PaymentId": "765548b6-c4b8-4e2c-b9b9-6458dbd5da0a",
        "Type": "EletronicTransfer",
        "Amount":10000,
        "ReceivedDate": "2015-06-25 09:37:55",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider": "Bradesco",
        "ReturnUrl": "http://www.braspag.com.br",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 0,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/{PaymentId}"
            }
        ]
    }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`Url`|URL to which the buyer will be redirected for electronic transfer authentication.|Text|256|Authentication URL|
|`Status`|Transaction status.|Byte|2|E.g.: 1|

## E-Wallet (Digital Wallet)

E-wallets are electronic safes (repositories) of cards and payment data for the physical and e-commerce customers. Digital wallets allow a customer to register their payment details, making the purchase process more convenient and secure.

<aside class="warning">The merchant needs the wallets to be integrated in their checkout in order to use them in Pagador.</aside>

Contact the provider of your choice for further information on how to contract the service.

### Available E-Wallets

Pagador currently supports the following digital wallets:

* [*Apple Pay*](https://www.apple.com/br/apple-pay/)
* [*Samsung Pay*](https://www.samsung.com.br/samsungpay/)
* [*Google Pay*](https://pay.google.com/intl/pt-BR_br/about/)
* [*Visa Checkout*](https://vaidevisa.visa.com.br/site/visa-checkout)
* [*Masterpass*](https://masterpass.com/)

<aside class="warning">When the “Wallet” node is sent in the request, the “CreditCard” node becomes optional.</aside>

<aside class="warning">When the “Wallet” node is sent in the request, the “DebitCard” node containing the “ReturnUrl” will be required for debit cards.</aside>

### E-Wallet Integration

Here is an example of a standard request for the e-wallet integration:

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{
  "MerchantOrderId": "2014111708",
  "Customer":{
    "Name": "Example Standard Wallet",
    "Identity": "11225468954",
    "IdentityType": "CPF"
  },
  "Payment":{
    "Type":"CreditCard",
    "Amount": 100,
    "Provider": "Cielo",
    "Installments":1,
    "Currency":"BRL",
    "Wallet": {
      "Type": "WALLET TYPE",
      "WalletKey": "WALLET STORE ID",
      "AdditionalData": {
        "EphemeralPublicKey": "TOKEN INFORMED BY WALLET"
      }
    }
  }
}
```

|Property|Description|Type|Size|Mandatory|
|----------------------------|--------|---------|-------------|--------------------------------------------------------|
|`MerchantId`|Store identifier at Braspag. (through header)|GUID|36|Yes|
|`MerchantKey`|Public key for dual authentication at Braspag. (through header)|Text|40|Yes|
|`RequestId`|Request identifier, used when the merchant uses different servers for each GET/POST/PUT. (through header)|GUID|36|No|
|`MerchantOrderId`|Order ID number.|Text|50|Yes|
|`Customer.Name`|Customer's name.|Text|255|Yes|
|`Customer.Status`|Customer's registration status ("NEW" / "EXISTING").|Text|255|No|
|`Payment.Type`|Payment method type.|Text|100|Yes|
|`Payment.Amount`|Order amount in cents.|Number|15|Yes|
|`Payment.Provider`|Cielo providers only ("Cielo" / "Cielo30").|Text|15|Yes|
|`Payment.Installments`|Number of installments.|Number|2|Yes|
|`Wallet.Type`|Wallet type: "ApplePay" / "SamsungPay" / "GooglePay" / "VisaCheckout" / "Masterpass".|Text|--|Yes|
|`Wallet.WalletKey`|Cryptographic key that identifies stores in wallets. Refer to the WalletKey table in the ANNEXES for more information.|Text|--|Yes|
|`Wallet.AdditionalData.EphemeralPublicKey`|Token returned by wallet. Must be submitted in **ApplePay** integrations.|Text|--|Yes|
|`Wallet.AdditionalData.CaptureCode`|Code informed by **Masterpass** to the merchant.|Text|--|Yes|
|`Wallet.AdditionalData.Signature`|Token returned by wallet. Must be submitted in **GooglePay** integrations.|Text|--|Yes|

##### WalletKey

WalletKey is the identifier used by Braspag to decrypt the payloads returned by an e-wallet.

Here is a list of the `WalletKey` formats to be passed to the Pagador API:

|Wallet|Example|
|----------------|----------------|
|*Apple Pay*|9zcCAciwoTS+qBx8jWb++64eHT2QZTWBs6qMVJ0GO+AqpcDVkxGPNpOR/D1bv5AZ62+5lKvucati0+eu7hdilwUYT3n5swkHuIzX2KO80Apx/SkhoVM5dqgyKrak5VD2/drcGh9xqEanWkyd7wl200sYj4QUMbeLhyaY7bCdnnpKDJgpOY6J883fX3TiHoZorb/QlEEOpvYcbcFYs3ELZ7QVtjxyrO2LmPsIkz2BgNm5f+JaJUSAOectahgLZnZR+easdhghrsa/E9A6DwjMd0fDYnxjj0bQDfaZpBPeGGPFLu5YYn1IDc|.|
|*Samsung Pay*|eyJhbGciOiJSU0ExXzUiLCJraWQiOiIvam1iMU9PL2hHdFRVSWxHNFpxY2VYclVEbmFOUFV1ZUR5M2FWeHBzYXVRPSIsInR5cCI6IkpPU0UiLCJjaGFubmVsU2VjdXJpdHlDb250ZXh0IjoiUlNBX1BLSSIsImVuYyI6IkExMjhHQ00ifQ.cCsGbqgFdzVb1jhXNR--gApzoXH-fdafddfa-Bo_utsmDN_DuGm69Kk2_nh6txa7ML9PCI59LFfOMniAf7ZwoZUBDCY7Oh8kx3wsZ0kxNBwfyLBCMEYzET0qcIYxePezQpkNcaZ4oogmdNSpYY-KbZGMcWpo1DKhWphDVp0lZcLxA6Q25K78e5AtarR5whN4HUAkurQ.CFjWpHkAVoLCG8q0.NcsTuauebemJXmos_mLMTyLhEHL-p5Wv6J88WkgzyjAt_DW7laiPMYw2sqRXkOiMJLwhifRzbSp8ZgJBM25IX05dKKSS4XfFjJQQjOBHw6PYtEF5pUDMLHML3jcddCrX07abfef_DuP41PqOQYsjwesLZ8XsRj-R0TH4diOZ_GQop8_oawjRIo9eJr9Wbtho0h8kAzHYpfuhamOPT718EaGAY6SSrR7t6nBkzGNkrKAmHkC7aRwe.AbZG53wRqgF0XRG3wUK_UQ|.|
|*Google Pay*|{\"encryptedMessage\":\"0mXBb94Cy9JZhMuwtrBhMjXb8pDslrNsN5KhcEqnowOINqJgjXHD36KcCuzpQQ4cDAe64ZLmk2N3UBGXsN9hMMyeMakXlidVmteE+QMaNZIor048oJqlUIFPD54B/ic8zCdqq3xnefUmyKQe0I03x57TcEA9xAT/E4x3rYfyqLFUAEtu2lT0GwTdwgrsT8pKoTldHIgP+wVNTjrKvJrB4xM/Bhn6JfcSmOzFyI6w37mBU71/TK761nYOSxt7z1bNWSLZ4b8xBu1dlRgen2BSlqdafuQjV3UZjr6ubSvaJ8NiCh5FD/X013kAwLuLALMS2uAFS9j8cZ6R6zNIi13fK6Fe4ACbFTHwLzSNZjQiaRDb6MlMnY8/amncPIOXzpirb5ScIz8EZUL05xd+3YWVTVfpqgFo1eaaS+wZdUyRG0QEgOsr6eLBoH8d5lfV9Rx6XdioorUuT7s1Yqc0OJZO+fhBt6X0izE9hBGTexdZyg\\u003d\\u003d\",\"ephemeralPublicKey\":\"BMdwrkJeEgCOtLevYsN3MbdP8xbOItXiTejoB6vXy0Kn0ZM10jy4Aasd6jTSxtoxoTpFydLhj5kzoOhbw2OzZu0\\u003d\",\"tag\":\"yAQIjWZ0VuCC7SWyYwc4eXOzpSUKhZduF9ip0Ji+Gj8\\u003d\"}|.|
|*Visa Checkout*|1140812334225873901|.|
|*Masterpass*|a561da1c18a89cfdafas875f9d43fc46cd9bf3e1|.|

##### EphemeralPublicKey

This is the `EphemeralPublicKey` format to be passed to the Pagador API:

|Wallet|Example|
|----------------|----------------------------------------------------------------------------------------------------------------------------------|
|*Apple Pay*|`MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEoedz1NqI6hs9hEO6dBsnn0X0xp5/DKj3gXirjEqxNIJ8JyhGxVB3ITd0E+6uG4W6Evt+kugG8gOhCBrdUU6JwQ==`|

##### Signature

This is the `Signature` format to be passed to the Pagador API:

|Wallet|Example|
|----------------|----------------------------------------------------------------------------------------------------------------------------------|
|*Google Pay*|`MEUCIQCGQLOmwxe5eFMSuTcr4EcwSZu35fB0KlCWcVop6ZxxhgIgbdtNHThSlynOopfxMIxkDs0cLh2NFh5es+J5uDmaViA=`|

#### Responses

```json
{
    "MerchantOrderId": "2014111703",
    "Customer":{
        "Name": "[Guest]"
    },
    "Payment":{
        "ServiceTaxAmount": 0,
        "Installments":1,
        "Interest": 0,
        "Capture": false,
        "Authenticate":false,
        "Recurrent":false,
        "CreditCard":{
            "CardNumber": "453211* *** **1521",
            "Holder": "BJORN IRONSIDE",
            "ExpirationDate": "08/2020",
            "SaveCard":"false",
            "Brand":"Visa",
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "WALLET TYPE",
            "WalletKey": "WALLET STORE ID",
            "Eci": 0,
            "AdditionalData": {
                "EphemeralPublicKey": "TOKEN INFORMED BY WALLET"
            },
        },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency":"BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.braspag.com.br/v2/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.braspag.com.br/v2/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

|Property|Description|Type|Size|Format|
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
|`ProofOfSale`|Authorization number, identical to NSU.|Text|6|Alphanumeric
|`Tid`|Transaction ID in the acquirer.|Text|20|Alphanumeric
|`AuthorizationCode`|Authorization code.|Text|6|Alphanumeric
|`SoftDescriptor`|Text to be printed on the bearer bank statement. Available for VISA/MASTER only - no special characters allowed.|Text|13|Alphanumeric|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|*Electronic Commerce Indicator*. Represents how secure a transaction is.|Text|2|E.g.: 7|
|`Status`|Transaction status.|Byte|2|E.g.: 1|
|`ReturnCode`|Return code from the acquirer.|Text|32|Alphanumeric|
|`ReturnMessage`|Return message from the acquirer.|Text|--|Alphanumeric|
|`Type`|Wallet type: "ApplePay" / "SamsungPay" / "GooglePay" / "VisaCheckout" / "Masterpass".|Text|--|Alphanumeric|
|`WalletKey`|Cryptographic key that identifies stores in wallets. Check the WalletKey table in the ANNEXES for more information.|Text|--|See "WalletKey" table annexed|
|`AdditionalData.EphemeralPublicKey`|Token returned by wallet. Must be submitted in **ApplePay** integrations.|Text|--|See "EphemeralPublicKey" table annexed|
|`AdditionalData.CaptureCode`|Code informed by **Masterpass** to the merchant.|Text|--|3|
|`AdditionalData.Signature`|Token returned by wallet. Must be submitted in **GooglePay** integrations.|Text|--|See "Signature" table annexed|

### Examples of Integration

Refer to our [E-Wallets](https://braspag.github.io//en/manual/ewallets) guide for a few examples of integration with the main e-wallets available in the market.

## Voucher

### Creating a Voucher Transaction

A transaction with a voucher card is similar to a debit card transaction; only without the authentication process. Currently, only the *Alelo* and *Ticket* providers are supported in this modality.

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment": {
      "Provider": "Alelo",
      "Type": "DebitCard",
      "Amount": 10,
      "Installments": 1,
      "DebitCard": {
         "CardNumber": "****4903",
         "Holder": "TesteBraspag",
         "ExpirationDate": "02/2019",
         "SecurityCode": "***",
         "Brand": "Elo"
      },
      [...]
   }
}
```

```shell
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2017051001",
   "Customer":{  
      "Name":"Nome do Comprador",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"comprador@braspag.com.br",
      "Birthdate":"1991-01-02",
      "IpAddress":"127.0.0.1",
      "Address":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27 andar",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country":"BRA",
         "District":"Alphaville"
      }
   },
   "Payment": {
      "Provider": "Alelo",
      "Type": "DebitCard",
      "Amount": 10,
      "Installments": 1,
      "DebitCard": {
         "CardNumber": "****4903",
         "Holder": "TesteBraspag",
         "ExpirationDate": "02/2019",
         "SecurityCode": "***",
         "Brand": "Elo"
      },
      [...]
   }
}
```

|Property|Description|Type|Size|Mandatory|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Name of the payment method provider. Currently only "Cielo" supports this form of payment via Pagador.|Text|15|Yes|
|`Payment.Type`|Payment method type. In this case, "DebitCard".|Text|100|Yes|
|`Payment.Amount`|Order amount in cents.|Number|15|Yes|
|`Payment.Installments`|Number of installments.|Number|2|Yes|
|`Payment.ReturnUrl`|URL to which the user will be redirected after the end of the payment.|Text|1024|Yes|
|`DeditCard.CardNumber`|Customer's card number.|Text|16|Yes|
|`DeditCard.Holder`|Name of the cardholder printed on the card.|Text|25|Yes|
|`DebitCard.ExpirationDate`|Expiration date printed on the card, in the MM/YYYY format. Note: "Ticket" vouchers do not have an expiration date printed on the card. Send a date previous to the current date to process the transaction.|Text|7|Yes|
|`DebitCard.SecurityCode`|Security code printed on back of card.|Text|4|Yes|
|`DebitCard.Brand`|Card brand.|Text|10|Yes|

#### Response

```json
{
    [...]
    "Payment":{
        "DebitCard":{
            "CardNumber": "527637******4903",
            "Holder": "TestBraspag",
            "ExpirationDate": "02/2019",
            "SaveCard":"false",
            "Brand": "Elo"
        },
        "ProofOfSale": "004045",
        "AcquirerTransactionId": "c63fb9f7-02ad-42b3-a182-c56e26238a00",
        "AuthorizationCode": "128752",
        "Eci": "0",
        "PaymentId": "562a8563-9181-4f12-bee8-0ccc89c8f931",
        "Type": "DebitCard",
        "Amount": 10,
        "ReceivedDate": "2018-02-21 10:59:57",
        "CapturedAmount": 10,
        "CapturedDate": "2018-02-21 11:00:48",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider": "Alelo",
        "ReturnUrl": "http://www.braspag.com.br",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 2,
        "ProviderReturnCode": "00",
        "ProviderReturnMessage": "Transaction successfully captured",
        [...]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    [...]
    "Payment":{
        "DebitCard":{
            "CardNumber": "527637******4903",
            "Holder": "TestBraspag",
            "ExpirationDate": "02/2019",
            "SaveCard":"false",
            "Brand": "Elo"
        },
        "ProofOfSale": "004045",
        "AcquirerTransactionId": "c63fb9f7-02ad-42b3-a182-c56e26238a00",
        "AuthorizationCode": "128752",
        "Eci": "0",
        "PaymentId": "562a8563-9181-4f12-bee8-0ccc89c8f931",
        "Type": "DebitCard",
        "Amount": 10,
        "ReceivedDate": "2018-02-21 10:59:57",
        "CapturedAmount": 10,
        "CapturedDate": "2018-02-21 11:00:48",
        "Currency":"BRL",
        "Country":"BRA",
        "Provider": "Alelo",
        "ReturnUrl": "http://www.braspag.com.br",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 2,
        "ProviderReturnCode": "00",
        "ProviderReturnMessage": "Transaction successfully captured",
        [...]
    }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`AcquirerTransactionId`|Transaction ID of the payment method provider.|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of sale reference.|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code from the acquirer.|Text|300|Alphanumeric|
|`PaymentId`|Order identifier field.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date that the transaction was received by Braspag.|Text|19|YYYY-MM-DDHH:mm:SS|
|`ReasonCode`|Operation return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Operation return message.|Text|512|Alphanumeric|
|`Status`|Transaction status.|Byte|2|E.g.: 1|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and issuer).|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and issuer).|Text|512|Transaction Approved|
|`AuthenticationUrl`|URL to which the holder will be redirected for authentication.|Text|56|https://qasecommerce.cielo.com.br/web/index.cbmp?id=13fda1da8e3d90d3d0c9df8820b96a7f|

# Recurrence

Unlike traditional credit card or boleto payments, recurring payments are automatically repeated for periods and at specified intervals, always charging the same amount from the same card or account.

It is widely used for magazine subscriptions, monthly fees, software licenses, among others. In addition to the technical integration, it is necessary for the customer's merchant to be enabled with the acquirer to receive recurring payments.

The merchant counts with features that can be used to shape their charging system in accordance with their business. Some of these features are: parameterization and change of periodicity, start and end date, number of attempts and interval between attempts. To learn more details about Recurrence, refer to our [article](https://suporte.braspag.com.br/hc/pt-br/articles/360013311991) in Portuguese.

<aside class="notice">Recurring credit card sales do not require CVV.</aside>
<aside class="warning">Recurrence is not available for e-wallet transactions due to the use of ephemeral keys which are necessary in credit operations.</aside>

## Recurrence Authorization

### Authorizing a Credit Card Recurrence

Add the `RecurrentPayment` node to the `Payment` node to schedule future recurrences when authorizing a transaction for the first time in the recurrence series.

The `Payment.RecurrentPayment.Interval` and `Payment.RecurrentPayment.DailyInterval` parameters, marked with an "\*" in the "MANDATORY" column, must not be used together. 

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{
    [...]
    "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Installments":1,
        "CreditCard":{
            "CardNumber":"455187******0181",
            "Holder": "Cardholder Name",
            "ExpirationDate":"12/2021",
            "SecurityCode":"123",
            "Brand":"Visa"
        },
        "RecurrentPayment": {
            "AuthorizeNow":"true",
            "EndDate":"2019-12-31",
            "Interval": "Monthly"
        }
    }
}

```

```shell

curl
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    [...]
    "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Installments":1,
        "CreditCard":{
            "CardNumber":"455187******0181",
            "Holder": "Cardholder Name",
            "ExpirationDate":"12/2021",
            "SecurityCode":"123",
            "Brand":"Visa"
        },
        "RecurrentPayment": {
            "AuthorizeNow":"true",
            "EndDate":"2019-12-31",
            "Interval": "Monthly"
        }
    }
}
--verbose

```

|Property|Type|Size|Mandatory|Description|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Text|15|Yes|Name of Payment Method Provider|
|`Payment.Type`|Text|100|Yes|Payment Method Type|
|`Payment.Amount`|Number|15|Yes|Order Amount (in cents)|
|`Payment.Installments`|Number|2|Yes|Number of Installments|
|`Payment.RecurrentPayment.EndDate`|Text|10|No|Recurring End Date|
|`Payment.RecurrentPayment.Interval`|Text|10|No*|Recurrence interval. Must not be used together with `DailyInterval`.<br>Monthly (default) / Bimonthly / Quarterly / SemiAnnual / Annual|
|`Payment.RecurrentPayment.DailyInterval`|Number|2|No*|Pattern of recurrence in days. Must not be used together with `Interval`.|
|`Payment.RecurrentPayment.AuthorizeNow`|Boolean|---|Yes|If true, authorizes at moment of request. False for future scheduling|
|`CreditCard.CardNumber`|Text|16|Yes|Shopper's card number|
|`CreditCard.Holder`|Text|25|Yes|Name of cardholder printed on card|
|`CreditCard.ExpirationDate`|Text|7|Yes|Expiration date printed on the card, in the MM/YYYY format|
|`CreditCard.SecurityCode`|Text|4|Yes|Security code printed on back of card|
|`CreditCard.Brand`|Text|10|Yes|Card brand|

#### Response

```json

{
  [...]
  "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard":"false",
      "Brand":"Visa",
    },
    "ProofOfSale": "5646418",
    "AcquirerTransactionId": "0511045646418",
    "AuthorizationCode": "100024",
    "PaymentId": "067f73ce-62fb-4d76-871d-0bcbb88fbd22",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 16:56:46",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "ProviderReturnCode": "4",
    "ProviderReturnMessage": "Operation Successful",
    "RecurrentPayment": {
      "RecurrentPaymentId": "808d3631-47ca-43b4-97f5-bd29ab06c271",
      "ReasonCode": 0,
      "ReasonMessage": "Successful",
      "NextRecurrency": "2017-06-11",
      "EndDate":"2019-12-31",
      "Interval": "Monthly",
      [...]
    }
  }
}

```

```shell

--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  [...]
  "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard":"false",
      "Brand":"Visa",
    },
    "ProofOfSale": "5646418",
    "AcquirerTransactionId": "0511045646418",
    "AuthorizationCode": "100024",
    "PaymentId": "067f73ce-62fb-4d76-871d-0bcbb88fbd22",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 16:56:46",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "ProviderReturnCode": "4",
    "ProviderReturnMessage": "Operation Successful",
    "RecurrentPayment": {
      "RecurrentPaymentId": "808d3631-47ca-43b4-97f5-bd29ab06c271",
      "ReasonCode": 0,
      "ReasonMessage": "Successful",
      "NextRecurrency": "2017-06-11",
      "EndDate":"2019-12-31",
      "Interval": "Monthly",
      [...]
    }
  }
}

```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`RecurrentPaymentId`|ID that represents the recurrence, used for future queries and changes|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`NextRecurrency`|Date when the next recurrence will happen|Text|7|05/2019 (MM/YYYY)|
|`EndDate`|End of recurrence date|Text|7|05/2019 (MM/YYYY)|
|`Interval`|Interval between recurrences.|Text|10|<ul><li>Monthly</li><li>Bimonthly</li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul>|
|`AuthorizeNow`|Boolean to know if the first recurrence will already be Authorized or not.|Boolean|---|true or false|

### Authorizing a Boleto Recurrence

The request is the same as the creation of a traditional boleto. Add the `RecurrentPayment` node to the `Payment` node to schedule future recurrences when authorizing a transaction for the first time in the recurrence series.

The expiration date of recurring boletos will be created based on the date of the next recurring order plus whatever is in the payment method settings here at Braspag.

E.g.: Day of next charge: 01/01/2021 + 5 days = Expiration of the ticket created automatically: 06/01/2021

Contact Support to determine how many days you want your boletos generated via Recurrence to expire.

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json

{
    [...]
        "Payment": {
        "Provider": "Simulado",
        "Type": "Boleto",
        "Amount": 1000,
     
        "Instructions": "Aceitar somente até a data de vencimento.",
        "RecurrentPayment": {
            "AuthorizeNow": "true",
            "StartDate": "2020-01-01",
            "EndDate": "2020-12-31",
            "Interval": "Monthly"
        }
    }
}

```

```shell

curl
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    [...]
"Payment": {
        "Provider": "Simulado",
        "Type": "Boleto",
        "Amount": 1000,
     
        "Instructions": "Aceitar somente até a data de vencimento.",
        "RecurrentPayment": {
            "AuthorizeNow": "true",
            "StartDate": "2020-01-01",
            "EndDate": "2020-12-31",
            "Interval": "Monthly"
        }
    }
}
--verbose

```

|Property|Type|Size|Mandatory|Description|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Text|15|Yes|Name of Payment Method Provider|
|`Payment.Type`|Text|100|Yes|Type of Payment Method|
|`Payment.Amount`|Number|15|Yes|Total amount (in cents)|
|`Payment.RecurrentPayment.StartDate`|Text|10|No|Recurrent start date|
|`Payment.RecurrentPayment.EndDate`|Text|10|No|Recurrent end date|
|`Payment.RecurrentPayment.Interval`|Text|10|No|Recurrence interval.<br/><ul><li>Monthly (Default) </li><li>Bimonthly </li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul> |
|`Payment.RecurrentPayment.AuthorizeNow`|Boolean|--- |Yes|If `true`, authorize at the same moment of the request. `false` to just make a schedulement|

#### Response

```json

{
    "MerchantOrderId": "teste001",
    "Customer": {
        "Name": "Nome do Comprador",
        "Identity": "12345678909",
        "IdentityType": "CPF",
        "Address": {
            "Street": "Alameda Xingu",
            "Number": "512",
            "Complement": "27 andar",
            "ZipCode": "06455914",
            "City": "São Paulo",
            "State": "SP",
            "Country": "BRA",
            "District": "Alphaville"
        }
    },
    "Payment": {
        "Instructions": "Aceitar somente até a data de vencimento.",
        "ExpirationDate": "2020-08-15",
        "Url": "https://transactionsandbox.pagador.com.br/post/pagador/reenvia.asp/58e4bde3-1abc-4aef-a58a-741f4c53940d",
        "BoletoNumber": "100031-0",
        "BarCodeNumber": "00096834800000129001234270000010003105678900",
        "DigitableLine": "00091.23423 40000.010004 31056.789008 6 83480000012900",
        "Address": "N/A, 1",
        "IsRecurring": false,
        "PaymentId": "58e4bde3-1abc-4aef-a58a-741f4c53940d",
        "Type": "Boleto",
        "Amount": 1000,
        "ReceivedDate": "2020-01-01 00:00:01",
        "Currency": "BRL",
        "Country": "BRA",
        "Provider": "Simulado",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 1,
        "RecurrentPayment": {
            "RecurrentPaymentId": "a08a622b-71f2-4553-9345-5f3c4fbbacb0",
            "ReasonCode": 0,
            "ReasonMessage": "Successful",
            "NextRecurrency": "2020-02-01",
            "StartDate": "2020-01-01",
            "EndDate": "2020-12-31",
            "Interval": "Monthly",
            "Link": {
                "Method": "GET",
                "Rel": "recurrentPayment",
                "Href": "https://apiquerysandbox.braspag.com.br/v2/RecurrentPayment/a08a622b-71f2-4553-9345-5f3c4fbbacb0"
            },
            "AuthorizeNow": true
        },
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/58e4bde3-1abc-4aef-a58a-741f4c53940d"
            }
        ]
    }
}

```

```shell

curl
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "teste001",
    "Customer": {
        "Name": "Nome do Comprador",
        "Identity": "12345678909",
        "IdentityType": "CPF",
        "Address": {
            "Street": "Alameda Xingu",
            "Number": "512",
            "Complement": "27 andar",
            "ZipCode": "06455914",
            "City": "São Paulo",
            "State": "SP",
            "Country": "BRA",
            "District": "Alphaville"
        }
    },
    "Payment": {
        "Instructions": "Aceitar somente até a data de vencimento.",
        "ExpirationDate": "2020-08-15",
        "Url": "https://transactionsandbox.pagador.com.br/post/pagador/reenvia.asp/58e4bde3-1abc-4aef-a58a-741f4c53940d",
        "BoletoNumber": "100031-0",
        "BarCodeNumber": "00096834800000129001234270000010003105678900",
        "DigitableLine": "00091.23423 40000.010004 31056.789008 6 83480000012900",
        "Address": "N/A, 1",
        "IsRecurring": false,
        "PaymentId": "58e4bde3-1abc-4aef-a58a-741f4c53940d",
        "Type": "Boleto",
        "Amount": 1000,
        "ReceivedDate": "2020-01-01 00:00:01",
        "Currency": "BRL",
        "Country": "BRA",
        "Provider": "Simulado",
        "ReasonCode": 0,
        "ReasonMessage": "Successful",
        "Status": 1,
        "RecurrentPayment": {
            "RecurrentPaymentId": "a08a622b-71f2-4553-9345-5f3c4fbbacb0",
            "ReasonCode": 0,
            "ReasonMessage": "Successful",
            "NextRecurrency": "2020-02-01",
            "StartDate": "2020-01-01",
            "EndDate": "2020-12-31",
            "Interval": "Monthly",
            "Link": {
                "Method": "GET",
                "Rel": "recurrentPayment",
                "Href": "https://apiquerysandbox.braspag.com.br/v2/RecurrentPayment/a08a622b-71f2-4553-9345-5f3c4fbbacb0"
            },
            "AuthorizeNow": true
        },
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/58e4bde3-1abc-4aef-a58a-741f4c53940d"
            }
        ]
    }
}
--verbose

```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`RecurrentPaymentId`|Identifier field of the next recurrence.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`NextRecurrency`|Date of the next recurrence.|Text|7|05/2019 (MM/YYYY)|
|`StartDate`|Start date of recurrence.|Text |7|05/2019 (MM/YYYY)|
|`EndDate`|End date of recurrence.|Text |7|05/2019 (MM/YYYY)|
|`Interval`|Recurrence interval. |Texto |10 |<ul><li>Monthly</li><li>Bimonthly </li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul> |
|`AuthorizeNow`|If `true`, authorize at the same moment of the request. `false` to just make a schedulement|Boolean|--- |true ou false |

## Recurrence Schedule 

### Scheduling an Authorization

Unlike the previous recurrence, this example does not immediately authorize, but schedules a future authorization.

To schedule the first transaction in the recurrence series, pass the `Payment.RecurrentPayment.AuthorizeNow` parameter to _"false"_ and add the `Payment.RecurrentPayment.StartDate` parameter.

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json

{
   [...]
   "Payment":{
     "Provider":"Simulado",
     "Type":"CreditCard",
     "Amount":10000,
     "Installments":1,
     "CreditCard":{
         "CardNumber":"455187******0181",
         "Holder": "Cardholder Name",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa"
     },
     "RecurrentPayment":{
       "AuthorizeNow": "false",
       "StartDate": "2017-12-31",
       "EndDate":"2019-12-31",
       "Interval": "Monthly"
     }
   }
}

```

```shell

curl
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   [...]
   "Payment":{
     "Provider":"Simulado",
     "Type":"CreditCard",
     "Amount":10000,
     "Installments":1,
     "CreditCard":{
         "CardNumber":"455187******0181",
         "Holder": "Cardholder Name",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa"
     },
     "RecurrentPayment":{
       "AuthorizeNow": "false",
       "StartDate": "2017-12-31",
       "EndDate":"2019-12-31",
       "Interval": "Monthly"
     }
   }
}
--verbose

```

|Property|Type|Size|Mandatory|Description|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Text|15|Yes|Name of Payment Method Provider|
|`Payment.Type`|Text|100|Yes|Payment Method Type|
|`Payment.Amount`|Number|15|Yes|Order Amount (in cents)|
|`Payment.Installments`|Number|2|Yes|Number of Installments|
|`Payment.RecurrentPayment.StartDate`|Text|10|No|Recurrence Start Date|
|`Payment.RecurrentPayment.EndDate`|Text|10|No|Recurring End Date|
|`Payment.RecurrentPayment.Interval`|Text|10|No|Recurrence Interval.Monthly<br/><ul><li>(Default) Bimonthly  </li><li></li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul>|
|`Payment.RecurrentPayment.AuthorizeNow`|Boolean|---|Yes|If true, authorizes at moment of request. false for future scheduling|
|`CreditCard.CardNumber`|Text|16|Yes|Shopper's card number|
|`CreditCard.Holder`|Text|25|Yes|Name of cardholder printed on card|
|`CreditCard.ExpirationDate`|Text|7|Yes|Expiration date printed on the card, in the MM/YYYY format|
|`CreditCard.SecurityCode`|Text|4|Yes|Security code printed on back of card|
|`CreditCard.Brand`|Text|10|Yes|Card brand|

#### Response

```json

{
  [...]
  "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard":"false",
      "Brand": "Undefined"
    },
    "Type":"CreditCard",
    "Amount":10000,
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    Status 20
    "RecurrentPayment": {
      "RecurrentPaymentId": "32703035-7dfb-4369-ac53-34c7ff7b84e8",
      "ReasonCode": 0,
      "ReasonMessage": "Successful",
      "NextRecurrency": "2017-12-31",
      "StartDate": "2017-12-31",
      "EndDate":"2019-12-31",
      "Interval": "Monthly",
      [...]
      "AuthorizeNow": false
    }
  }
}

```

```shell

--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  [...]
  "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard":"false",
      "Brand": "Undefined"
    },
    "Type":"CreditCard",
    "Amount":10000,
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "Status":20,
    "RecurrentPayment": {
      "RecurrentPaymentId": "32703035-7dfb-4369-ac53-34c7ff7b84e8",
      "ReasonCode": 0,
      "ReasonMessage": "Successful",
      "NextRecurrency": "2017-12-31",
      "StartDate": "2017-12-31",
      "EndDate":"2019-12-31",
      "Interval": "Monthly",
      [...]
      "AuthorizeNow": false
    }
  }
}

```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`RecurrentPaymentId`|Field Identifier of the next recurrence.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`NextRecurrency`|Date of next recurrence.|Text|7|05/2019 (MM/YYYY)|
|`StartDate`|Date of start of recurrence.|Text|7|05/2019 (MM/YYYY)|
|`EndDate`|Date of end of recurrence.|Text|7|05/2019 (MM/YYYY)|
|`Interval`|Interval between recurrences.|Text|10|<ul><li>Monthly</li><li>Bimonthly</li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul>|
|`AuthorizeNow`|Boolean to know if the first recurrence will already be Authorized or not.|Boolean|---|true or false|

## Data Alteration

### Changing Customer Data

To change customer data in an existing recurrence, simply make a PUT call as demonstrated in the example:

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}/Customer</span></aside>

```json

{
  "Name": "Another Shopper Name",
  "Email":"outrocomprador@braspag.com.br",
  "Birthdate":"1999-12-12",
  "Identity":"0987654321",
  "IdentityType": "CPF",
  "Address":{
      "Street":"Avenida Brigadeiro Faria Lima",
      "Number":"1500",
      "Complement":"AP 201",
      "ZipCode":"05426200",
      "City":"São Paulo",
      "State":"SP",
      "Country":"BRA",
      "District":"Alphaville"
      },
   "DeliveryAddress":{
      "Street":"Avenida Brigadeiro Faria Lima",
      "Number":"1500",
      "Complement":"AP 201",
      "ZipCode":"05426200",
      "City":"São Paulo",
      "State":"SP",
      "Country":"BRA",
      "District":"Alphaville"
      }
    }
}

```

```shell

curl
--request PUT "https://apisandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}/Customer"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Name": "Another Shopper Name",
    "Email":"outrocomprador@braspag.com.br",
    "Birthdate":"1999-12-12",
    "Identity":"0987654321",
    "IdentityType": "CPF",
    "Address":{
    "Street":"Avenida Brigadeiro Faria Lima",
      "Number":"1500",
      "Complement":"AP 201",
      "ZipCode":"05426200",
      "City":"São Paulo",
      "State":"SP",
      "Country":"BRA",
      "District":"Alphaville"
   },
   "DeliveryAddress":{
    "Street":"Avenida Brigadeiro Faria Lima",
      "Number":"1500",
      "Complement":"AP 201",
      "ZipCode":"05426200",
      "City":"São Paulo",
      "State":"SP",
      "Country":"BRA",
      "District":"Alphaville"
      }
}
--verbose

```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence ID number.|Text|50|Yes (through endpoint)|
|`Customer.Name`|Customer's name.|Text|255|Yes|
|`Customer.Email`|Customer's email.|Text|255|No|
|`Customer.Birthdate`|Customer's date of birth.|Date|10|No|
|`Customer.Identity`|Customer ID, CPF or CNPJ number.|Text|14|No|
|`Customer.IdentityType`|Text|255|No|Customer Identification Document Type (CPF or CNPJ)|
|`Customer.Address.Street`|Customer's contact address.|Text|255|No|
|`Customer.Address.Number`|Customer's contact address number.|Text|15|No|
|`Customer.Address.Complement`|Customer's contact address additional information.|Text|50|No|
|`Customer.Address.ZipCode`|Customer's contact address zip code.|Text|9|No|
|`Customer.Address.City`|Customer's contact address city.|Text|50|No|
|`Customer.Address.State`|Customer's contact address state.|Text|2|No|
|`Customer.Address.Country`|Customer's contact address country.|Text|35|No|
|`Customer.Address.District`|Customer's contact address neighborhood.|Text|50|No|
|`Customer.DeliveryAddress.Street`|Delivery address street.|Text|255|No|
|`Customer.DeliveryAddress.Number`|Delivery address number.|Text|15|No|
|`Customer.DeliveryAddress.Complement`|Delivery address additional information.|Text|50|No|
|`Customer.DeliveryAddress.ZipCode`|Delivery address zip code.|Text|9|No|
|`Customer.DeliveryAddress.City`|Delivery address city.|Text|50|No|
|`Customer.DeliveryAddress.State`|Delivery address state.|Text|2|No|
|`Customer.DeliveryAddress.Country`|Delivery address country.|Text|35|No|
|`Customer.DeliveryAddress.District`|Delivery address neighborhood.|Text|50|No|

#### Response

```shell

HTTP Status 200

```

See Appendix HTTP Status Code for a list of all HTTP status codes possibly returned by the API.

### Changing Recurrence End Date

To change the end date of the existing recurrence, just make a PUT as shown.

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}/EndDate</span></aside>

```json

"2021-01-09"

```

```shell

curl
--request PUT "https://apisandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}/EndDate"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
"2021-01-09"
--verbose

```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence ID number.|Text|50|Yes (through endpoint)|
|`EndDate`|Date to end recurrence|Text|10|Yes|

#### Response

```shell

HTTP Status 200

```

See Appendix HTTP Status Code for a list of all HTTP status codes possibly returned by the API.

### Changing Recurrence Interval

To change the range of an existing recurrence, just make a PUT as per the example.

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}/Interval</span></aside>

```json

"SemiAnnual"

```

```shell

curl
--request PUT "https://apisandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}/Interval"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
"SemiAnnual"
--verbose

```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence ID number.|Text|50|Yes (through endpoint)|
|`Interval`|Recurrence Interval. <ul><li>Monthly</li><li>Bimonthly</li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul>|Text|2|Yes|

#### Response

```shell

HTTP Status 200

```

See Appendix HTTP Status Code for a list of all HTTP status codes possibly returned by the API.

### Changing Recurrence Day

To modify the expiration day of an existing recurrence, simply make a PUT as per the example.

<aside class="notice"><strong>Rule:</strong> If the new day entered is after the current day, we will update the recurrence day with effect on the next recurrence. E.g.: Today is May 5, and the next recurrence is May 25. When I upgrade to the 10, the next recurrence date will be May 10. If the new day entered is before the current day, we will update the recurrence day, but this will take effect only after the next recurrence has been successfully executed. E.g.: Today is 5th, and the next recurrence is May 25. When I upgrade to day 3, the date of the next recurrence will remain on May 25, and after it runs, the next will be scheduled for June 3. If the new day entered is before the current day, but the next recurrence is in another month, we will update the recurrence day with effect on the next recurrence. E.g.: Today is 5th, and the next recurrence is September 25. When I upgrade to day 3, the next recurrence date will be September 3</aside>

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}/RecurrencyDay</span></aside>

```json

16

```

```shell

curl
--request PUT "https://apisandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}/RecurrencyDay"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
16
--verbose

```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence ID number.|Text|50|Yes (through endpoint)|
|`RecurrencyDay`|Recurrence Day|Number|2|Yes|

#### Response

```shell

HTTP Status 200

```

See Appendix HTTP Status Code for a list of all HTTP status codes possibly returned by the API.

### Changing Recurrence Transaction Amount

To modify the transaction value of an existing recurrence, simply make a PUT as per the example.

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}/Amount</span></aside>

```json

156

```

```shell

curl
--request PUT "https://apisandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}/Amount"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
156
--verbose

```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence ID number.|Text|50|Yes (through endpoint)|
|`Payment.Amount`|Order value in cents: 156 is R$ 1.56|Number|15|Yes|

<aside class="warning">This change only affects the payment date of the next recurrence.</aside>

#### Response

```shell

HTTP Status 200

```

See Appendix HTTP Status Code for a list of all HTTP status codes possibly returned by the API.

### Changing Next Payment Date

To change only the next payment date, just make a PUT as per the example.

This operation only modifies the date of the next payment, i.e. future recurrences will remain with the original characteristics.

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}/NextPaymentDate</span></aside>

```json

"2017-06-15"

```

```shell

curl
--request PUT "https://apisandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}/NextPaymentDate"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
"2016-06-15"
--verbose

```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence ID number.|Text|50|Yes (through endpoint)|
|`NextPaymentDate`|Date of next recurrence payment|Text|10|Yes|

#### Response

```shell

HTTP Status 200

```

See Appendix HTTP Status Code for a list of all HTTP status codes possibly returned by the API.

### Changing Recurrence Payment Data

During the life cycle of a recurrence, you can change:

* Acquirer (Rede to Cielo, for example)
* Card (in case of expired card)
* Payment method (from Card to Boleto and vice versa)

To change the payment details, simply make a PUT as per the example.

<aside class="notice"><strong>Attention:</strong> This change affects all Payment node data. So to keep the previous data you must enter the fields that will not change with the same values that were already saved.</aside>

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}/Payment</span></aside>

```json

{
   "Type":"CreditCard",
   "Amount": "20000",
   "Installments":3,
   "Country": "USA",
   "Currency":"USD",
   "SoftDescriptor":"Message",
   "Provider":"Simulado",
   "CreditCard":{
      "Brand":"Master",
      "Holder": "Cardholder Name",
      "CardNumber":"4111111111111111",
      "ExpirationDate":"05/2019"
      },
   "Credentials": {
      "code":"9999999",
      "key":"D8888888",
      "password":"LOJA9999999",
      "username": "#Braspag2018@NOMEDALOJA#",
      "signature": "001"
      }
}

```

```shell

curl
--request PUT "https://apisandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}/Payment"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   "Type":"CreditCard",
   "Amount": "20000",
   "Installments":3,
   "Country": "USA",
   "Currency":"USD",
   "SoftDescriptor":"Message",
   "Provider":"Simulado",
   "CreditCard":{
      "Brand":"Master",
      "Holder": "Cardholder Name",
      "CardNumber":"4111111111111111",
      "ExpirationDate":"05/2019"
      },
   "Credentials": {
      "code":"9999999",
      "key":"D8888888",
      "password":"LOJA9999999",
      "username": "#Braspag2018@NOMEDALOJA#",
      "signature": "001"
      }
}
--verbose

```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence ID number.|Text|50|Yes (through endpoint)|
|`Payment.Provider`|Name of the Payment Method Provider|Text|15|Yes|
|`Payment.Type`|Payment method type.|Text|100|Yes|
|`Payment.Amount`|Order Amount (in cents)|Number|15|Yes|
|`Payment.Installments`|Number of Installments|Number|2|Yes|
|`Payment.SoftDescriptor`|Text to be printed on the credit card invoice|Text|13|No|
|`CreditCard.CardNumber`|Card Number|Text|16|Yes|
|`CreditCard.Holder`|Card holder name|Text|25|Yes|
|`CreditCard.ExpirationDate`|Expiration date printed on the card|Text|7|Yes|
|`CreditCard.SecurityCode`|Security code printed on back of the card|Text|4|Yes|
|`CreditCard.Brand`|Card brand|Text|10|Yes|
|`Payment.Credentials.Code`|acquirer affiliation|Text|100|Yes|
|`Payment.Credentials.Key`|affiliate key/token by acquirer|Text|100|Yes|
|`Payment.Credentials.Username`|user generated in the accreditation with the acquirer (providers like Rede and Getnet use username and password in communications, so the field must be sent.)|Text|50|No|
|`Payment.Credentials.Password`|password generated in the accreditation with the acquirer (providers like Rede and Getnet use username and password in communications, so the field must be sent.)|Text|50|No|
|`Payment.Credentials.Signature`|Text|3|No|Submit TerminalID from Global Payments (applicable to merchants affiliated with this acquirer). E.g.: 001|Text|3|No|

#### Response

```shell
HTTP Status 200
```

See Appendix HTTP Status Code for a list of all HTTP status codes possibly returned by the API.

## Order Deactivation

### Deactivating a Recurring Order

To deactivate a recurring request, simply make a PUT as per the example.

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}/Payment</span></aside>

```shell
curl
--request PUT "https://apisandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}/Deactivate"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence ID number.|Text|50|Yes (through endpoint)|

#### Response

```shell
HTTP Status 200
```

See Appendix HTTP Status Code for a list of all HTTP status codes possibly returned by the API.

## Order Reactivation

### Reactivating a Recurring Order

To reactivate a recurring request, simply make a PUT as per the example.

#### Request

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}/Reactivate</span></aside>

```shell
curl
--request PUT "https://apisandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}/Reactivate"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence ID number.|Text|50|Yes (through endpoint)|

#### Response

```shell

HTTP Status 200

```

See Appendix HTTP Status Code for a list of all HTTP status codes possibly returned by the API.

## Transaction with Renova Fácil

Renova Fácil is a service developed by CIELO together with issuing banks, which aims to increase the conversion rate of recurring credit card sales.

By identifying expired cards at the time of transaction, the authorization is made with a new card and the new card is returned for storage.

To use Renova Fácil, the service must be enabled at CIELO. It is not necessary to send any extra information in the authorization request, but the response will have one more node as shown below.

Participating Banks: Bradesco, Banco do Brasil, Santander, Panamericano, Citibank

### Response

```json
{
  [...]
  "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187* *** **0183",
      "Holder": "Cardholder Name",
      "ExpirationDate": "12/2016",
      "SaveCard":"false",
      "Brand":"Visa",
    },
    "AcquirerTransactionId": "0512105630844",
    "NewCard": {
      "CardNumber": "4551870000512353",
      "Holder": "Cardholder Name",
      "ExpirationDate": "05/2020",
      "SaveCard":"false",
      "Brand": "Visa"
    },
    "PaymentId": "ca81c3c9-2dfa-4e6e-9c77-37e33a77ac84",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-12 10:56:30",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 15,
    "ReasonMessage": "CardExpired",
    "Status": 3,
    "ProviderReturnCode": "57",
    "ProviderReturnMessage": "Card Expired",
    [...]
  }
}

```

```shell

{
  [...]
  "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187* *** **0183",
      "Holder": "Cardholder Name",
      "ExpirationDate": "12/2016",
      "SaveCard":"false",
      "Brand":"Visa",
    },
    "AcquirerTransactionId": "0512105630844",
    "NewCard": {
      "CardNumber": "4551870000512353",
      "Holder": "Cardholder Name",
      "ExpirationDate": "05/2020",
      "SaveCard":"false",
      "Brand":"Visa",
    },
    "PaymentId": "ca81c3c9-2dfa-4e6e-9c77-37e33a77ac84",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-12 10:56:30",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 15,
    "ReasonMessage": "CardExpired",
    "Status": 3,
    "ProviderReturnCode": "57",
    "ProviderReturnMessage": "Card Expired",
    [...]
  }
}

```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`NewCard.CardNumber`|New Shopper's Card Number|Text|16|
|`NewCard.Holder`|Holder name printed on new card|Text|25|
|`NewCard.ExpirationDate`|Expiration date printed on the new card|Text|7|
|`CreditCard.SecurityCode`|Security code printed on back of the card|Text|4|Yes|
|`NewCard.Brand`|New Card Brand|Text|10|

# Saving and reusing cards

With the Cartão Protegido, you can save your client's credit card on Braspag and use it for future "one-click" transactions, from returning buyers, or recurrent payments. Important: for security reasons, Braspag will never store the credit card security code (CVV).

In addition to Card Token generation, you can associate a name, an identifier in text format, with the saved card. This identifier will be the Alias.

## Saving a card during an authorization

#### Request

To save a credit card used in a transaction, simply send the `Payment.SaveCard` parameter as _true_ in the standard authorization request.

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json

{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "CardNumber":"455187******0181",
            "Holder": "Cardholder Name",
            "ExpirationDate":"12/2021",
            "SecurityCode":"123",
            "Brand":"Visa",
            "SaveCard": true,
            "Alias":""
        },
        [...]
    }
}

```

```shell

curl
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "CardNumber":"455187******0181",
            "Holder": "Cardholder Name",
            "ExpirationDate":"12/2021",
            "SecurityCode":"123",
            "Brand":"Visa",
            "SaveCard": true,
            "Alias":""
        },
        [...]
    }
}
--verbose

```

|Property|Type|Size|Mandatory|Description|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Text|15|Yes|Name of Payment Method Provider|
|`Payment.Type`|Text|100|Yes|Payment Method Type|
|`Payment.Amount`|Number|15|Yes|Order Amount (in cents)|
|`Payment.Installments`|Number|2|Yes|Number of Installments|
|`CreditCard.CardNumber`|Text|16|Yes|Shopper's card number|
|`CreditCard.Holder`|Text|25|Yes|Name of cardholder printed on card|
|`CreditCard.ExpirationDate`|Text|7|Yes|Expiration date printed on the card, in the MM/YYYY format|
|`CreditCard.SecurityCode`|Text|4|Yes|Security code printed on back of card|
|`CreditCard.Brand`|Text|10|Yes|Card brand|
|`CreditCard.SaveCard`|Boolean|10|No (Default false)|true to save the card and false to not save|
|`CreditCard.Alias`|Text|64|No|Credit Card Alias|

##### Response

The `CreditCard.CardToken` parameter will return the token to be saved for future transactions with the same card.

```json

{
  [...]
  },
    "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard": true,
      "CardToken": "250e7c7c-5501-4a7c-aa42-a33d7ad61167",
      "Brand":"Visa",
      "Alias": "Customer1"
    },
    "ProofOfSale": "3519928",
    "AcquirerTransactionId": "0511023519928",
    "AuthorizationCode": "536934",
    "PaymentId": "3af00b2d-dbd0-42d6-a669-d4937f0881da",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 14:35:19",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "ProviderReturnCode": "4",
    "ProviderReturnMessage": "Operation Successful",
    [...]
  }
}

```

```shell

curl
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  [...]
  },
    "Payment":{
    "ServiceTaxAmount": 0,
    "Installments":1,
    "Interest":"ByMerchant",
    "Capture":true,
    "Authenticate":false,
    "Recurrent":false,
    "CreditCard":{
      "CardNumber": "455187******0181",
      "Holder": "Cardholder Name",
      "ExpirationDate":"12/2021",
      "SaveCard": true,
      "CardToken": "250e7c7c-5501-4a7c-aa42-a33d7ad61167",
      "Brand":"Visa",
      "Alias": "Customer1"
    },
    "ProofOfSale": "3519928",
    "AcquirerTransactionId": "0511023519928",
    "AuthorizationCode": "536934",
    "PaymentId": "3af00b2d-dbd0-42d6-a669-d4937f0881da",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 14:35:19",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "ProviderReturnCode": "4",
    "ProviderReturnMessage": "Operation Successful",
    [...]
  }
}
--verbose

```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`AcquirerTransactionId`|Transaction Id of the Payment Method Provider|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of Sale Reference|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code from the acquirer|Text|300|Alphanumeric text|
|`PaymentId`|Order Identifier field|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date that the transaction was received by Braspag|Text|19|YYYY-MM-DD HH:mm:SS|
|`ReasonCode`|Operation return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Operation return message.|Text|512|Alphanumeric|
|`Status`|Transaction status.|Byte|2|E.g.: 1|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and banks)|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and banks)|Text|512|Transaction Approved|
|`CreditCard.CardToken`|Cartão Protegido Token representing card data|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|

## Creating a Card Token Transaction

This is an example of how to use the previously saved Card Token to create a transaction. For security reasons, a Card Token has not kept the Security Code (CVV). Therefore, you must request this information from the holder for each new transaction. If your merchant location is set to Recurring, you may submit transactions without CVV.

#### Request

The `CreditCard` node within the `Payment` node will change as shown below.

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json

{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "CardToken":"250e7c7c-5501-4a7c-aa42-a33d7ad61167",
            "SecurityCode":"123",
            "Brand":"Visa"
            },
        [...]
    }
}

```

```shell

curl
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "CardToken":"250e7c7c-5501-4a7c-aa42-a33d7ad61167",
            "SecurityCode":"123",
            "Brand":"Visa"
            },
        [...]
    }
}
--verbose

```

|Property|Type|Size|Mandatory|Description|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Text|15|Yes|Name of Payment Method Provider|
|`Payment.Type`|Text|100|Yes|Payment Method Type|
|`Payment.Amount`|Number|15|Yes|Order Amount (in cents)|
|`Payment.Installments`|Number|2|Yes|Number of Installments|
|`CreditCard.CardToken`|Cartão Protegido Token representing card data|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`CreditCard.SecurityCode`|Text|4|No|Security code printed on the back of the card. To make transactions without the CVV, you must request permission with your acquirer.|
|`CreditCard.Brand`|Text|10|Yes|Card brand|

##### Response

```json

{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "CardToken":"250e7c7c-5501-4a7c-aa42-a33d7ad61167",
            "SecurityCode":"123",
            "Brand":"Visa"
            },
    "ProofOfSale": "124305",
    "AcquirerTransactionId": "0511030124305",
    "AuthorizationCode": "065964",
    "PaymentId": "23cd8bf5-2251-4991-9042-533ff5608788",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 15:01:24",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "ProviderReturnCode": "4",
    "ProviderReturnMessage": "Operation Successful",
   [...]
  }
}

```

```shell

--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "CardToken":"250e7c7c-5501-4a7c-aa42-a33d7ad61167",
            "SecurityCode":"123",
            "Brand":"Visa"
            },
    "ProofOfSale": "124305",
    "AcquirerTransactionId": "0511030124305",
    "AuthorizationCode": "065964",
    "PaymentId": "23cd8bf5-2251-4991-9042-533ff5608788",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 15:01:24",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "ProviderReturnCode": "4",
    "ProviderReturnMessage": "Operation Successful",
   [...]
  }
}

```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`AcquirerTransactionId`|Transaction Id of the Payment Method Provider|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of Sale Reference|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code from the acquirer|Text|300|Alphanumeric text|
|`PaymentId`|Order Identifier field|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date that the transaction was received by Braspag|Text|19|YYYY-MM-DD HH:mm:SS|
|`ReasonCode`|Operation return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Operation return message.|Text|512|Alphanumeric|
|`Status`|Transaction Status|Byte|2|E.g.: 1|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and banks)|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and banks)|Text|512|Transaction Approved|

## Creating a Transaction with Alias

This is an example of how to use the previously saved Alias to create a transaction. For security reasons, an Alias has not kept the Security Code. Therefore, you must request this information from the holder for each new transaction. If your merchant location is set to Recurring, you may submit transactions without CVV.

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json

{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "Alias": "Customer1",
            "SecurityCode":"123",
            "Brand":"Visa"
            },
        [...]
    }
}

```

```shell

curl
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "Alias": "Customer1",
            "SecurityCode":"123",
            "Brand":"Visa"
            },
        [...]
    }
}
--verbose

```

|Property|Type|Size|Mandatory|Description|
|-----------|----|-------|-----------|---------|
|`Payment.Provider`|Text|15|Yes|Name of Payment Method Provider|
|`Payment.Type`|Text|100|Yes|Payment Method Type|
|`Payment.Amount`|Number|15|Yes|Order Amount (in cents)|
|`Payment.Installments`|Number|2|Yes|Number of Installments|
|`CreditCard.CardToken`|Cartão Protegido Token representing card data|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`CreditCard.SecurityCode`|Text|4|No|Security code printed on the back of the card. To make transactions without the CVV, you must request permission with your acquirer.|
|`CreditCard.Brand`|Text|10|Yes|Card brand|
|`CreditCard.Alias`|Text|64|No|Credit Card Alias|

##### Response

```json

{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "Alias": "Customer1",
            "SecurityCode":"123",
            "Brand":"Visa"
            },
    "ProofOfSale": "124305",
    "AcquirerTransactionId": "0511030124305",
    "AuthorizationCode": "065964",
    "PaymentId": "23cd8bf5-2251-4991-9042-533ff5608788",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 15:01:24",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "ProviderReturnCode": "4",
    "ProviderReturnMessage": "Operation Successful",
   [...]
  }
}

```

```shell

--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   [...]
   },
     "Payment":{
        "Provider":"Simulado",
        "Type":"CreditCard",
        "Amount":10000,
        "Currency":"BRL",
        "Country":"BRA",
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":true,
        "Authenticate":false,
        "Recurrent":false,
        "SoftDescriptor":"Message",
        "CreditCard":{
            "Alias": "Customer1",
            "SecurityCode":"123",
            "Brand":"Visa"
            },
    "ProofOfSale": "124305",
    "AcquirerTransactionId": "0511030124305",
    "AuthorizationCode": "065964",
    "PaymentId": "23cd8bf5-2251-4991-9042-533ff5608788",
    "Type":"CreditCard",
    "Amount":10000,
    "ReceivedDate": "2017-05-11 15:01:24",
    "Currency":"BRL",
    "Country":"BRA",
    "Provider":"Simulado",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "Status": 1,
    "ProviderReturnCode": "4",
    "ProviderReturnMessage": "Operation Successful",
   [...]
  }
}

```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`AcquirerTransactionId`|Transaction Id of the Payment Method Provider|Text|40|Alphanumeric|
|`ProofOfSale`|Proof of Sale Reference|Text|20|Alphanumeric|
|`AuthorizationCode`|Authorization code from the acquirer|Text|300|Alphanumeric text|
|`PaymentId`|Order Identifier field|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date that the transaction was received by Braspag|Text|19|YYYY-MM-DD HH:mm:SS|
|`ReasonCode`|Operation return code.|Text|32|Alphanumeric|
|`ReasonMessage`|Operation return message.|Text|512|Alphanumeric|
|`Status`|Transaction Status|Byte|2|E.g.: 1|
|`ProviderReturnCode`|Code returned by the payment provider (acquirer and banks)|Text|32|57|
|`ProviderReturnMessage`|Message returned by the payment provider (acquirer and banks)|Text|512|Transaction Approved|

# Payments with Fraud Analysis

You can verify whether a transaction is likely to be a fraud or not during an authorization.

|Integration Type|Description|Required Parameters|
|-|-|-|
|Pre-authorization review|Before the transaction is submitted for authorization, AntiFraude assesses whether it is at high risk or not. This avoids sending risky transactions for authorization|`FraudAnalysis.Sequence` equal to _AnalyseFirst_|
|Analysis after authorization|Before the transaction is sent to Antifraud, it will be sent for authorization|`FraudAnalysis.Sequence` equal to _AuthorizeFirst_|
|Risk analysis only if transaction is authorized|AntiFraude is only triggered to analyze transactions with _authorized_ staus. This avoids the cost of unauthorized transaction analysis|`FraudAnalysis.SequenceCriteria` equal to _OnSuccess_|
|Risk analysis in any event|Regardless of transaction status after authorization, Antifraud will analyze risk|`FraudAnalysis.Sequence` equal to _AuthorizeFirst_ and` FraudAnalysis.SequenceCriteria` as _Always_|
|Authorization in any event|Regardless of the transaction fraud score, it will always be submitted for authorization|`FraudAnalysis.Sequence` as _AnalyseFirst_ and` FraudAnalysis.SequenceCriteria` as _Always_|
|Capture only if a transaction is secure|After fraud analysis, automatically captures an already authorized transaction if set low risk. This same parameter is for you that will work with manual review, which after Braspag receives notification of the new status and is equal to accepted, the transaction will be automatically captured|`FraudAnalysis.Sequence` equal to _AuthorizeFirst_,` FraudAnalysis.CaptureOnLowRisk` _true_ and `Payment.Capture` equal to _false_||
|Cancel a suspect transaction|If fraud analysis returns a high risk for an already authorized or captured transaction, it will be immediately canceled or reversed. This same parameter is for you who will work with manual review, that after Braspag receives notification of the new status and is equal to rejected, the transaction will be automatically canceled or reversed|`FraudAnalysis.Sequence` as _AuthorizeFirst_ and` FraudAnalysis.VoidOnHighRisk` equal to _true_|

If not specified otherwise during authorization, Braspag will process your transaction through the flow `FraudAnalysis.Sequence` _AuthorizeFirst_, `FraudAnalysis.SequenceCriteria` _OnSuccess_, `FraudAnalysis.VoidOnHighRisk` _false_ and `FraudAnalysis.CaptureOnLowRisk` _false_.

## Implementing Cybersource Fraud Analysis

For CyberSource fraud analysis to be performed during a credit card transaction, you must supplement the authorization agreement with the "FraudAnalysis", "Cart", "MerchantDefinedFields" and "Travel (only for airline tickets)" nodes.

#### Request

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v2/sales/</span></aside>

```json
{
   "MerchantOrderId":"2017051002",
   "Customer":{
      "Name": "shopper Name",
      "Identity": "12345678910",
      "IdentityType": "CPF",
      "Email": "shopper@braspag.com.br",
      "Birthdate":"1991-01-02",
      "Phone": "5521976781114",
      "Address":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country": "BR",
         "District":"Alphaville"
      },
      "DeliveryAddress":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country": "BR",
         "District":"Alphaville"
      }
   },
   "Payment":{
      "Provider":"Simulado",
      "Type":"CreditCard",
      "Amount":10000,
      "Currency":"BRL",
      "Country":"BRA",
      "Installments":1,
      "Interest":"ByMerchant",
      "Capture":true,
      "Authenticate":false,
      "Recurrent":false,
      "SoftDescriptor":"Message",
      "DoSplit":false,
      "CreditCard":{
         "CardNumber":"455187******0181",
         "Holder": "Cardholder Name",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa",
         "SaveCard": "false"
      },
      "Credentials":{
         "code":"9999999",
         "key":"D8888888",
         "password":"LOJA9999999",
         "username":"#Braspag2018@NOMEDALOJA#",
         "signature":"001"
      },
      "ExtraDataCollection":[
         {
            "Name":"FieldName",
            "Value":"FieldValue"
         }
      ],
      "FraudAnalysis": {
         "Sequence": "AnalyseFirst",
         "SequenceCriteria": "Always",
         "Provider": "Cybersource",
         "CaptureOnLowRisk": false,
         "VoidOnHighRisk": false,
         "TotalOrderAmount": 10000,
         "FingerPrintId":"074c1ee676ed4998ab66491013c565e2",
         "Browser":{
            "CookiesAccepted":false,
            "Email": "shopper@braspag.com.br",
            "HostName":"Test",
            "IpAddress": "127.0.0.1",
            "Type": "Chrome"
         },
         "Cart": {
            "IsGift": false,
            "ReturnsAccepted": true,
            "Items":[
               {
                  "GiftCategory": "Undefined",
                  "HostHedge": "Off",
                  "NonSensicalHedge": "Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "Name":"ItemTest1",
                  "Quantity":1,
                  "Sku":"20170511",
                  "UnitPrice":10000,
                  "Risk":"High",
                  "TimeHedge":"Normal",
                  "Type":"AdultContent",
                  "VelocityHedge":"High"
               },
               {
                  "GiftCategory": "Undefined",
                  "HostHedge": "Off",
                  "NonSensicalHedge": "Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "Name":"ItemTest2",
                  "Quantity":1,
                  "Sku":"20170512",
                  "UnitPrice":10000,
                  "Risk":"High",
                  "TimeHedge":"Normal",
                  "Type":"AdultContent",
                  "VelocityHedge":"High"
               }
            ]
         },
         "MerchantDefinedFields":[
            {
               "Id": 2,
               "Value": "100"
            },
            {
               "Id": 4,
               "Value": "Web"
            },
            {
               "Id": 9,
               "Value":"SIM"
            }
         ],
         "Shipping":{
            "Addressee":"João das Couves",
            "Method":"LowCost",
            "Phone":"551121840540"
         },
         "Travel":{
            "JourneyType":"OneWayTrip",
            "DepartureTime":"2018-01-09 18:00",
            "Passengers":[
               {
                  "Name":"Passenger Test",
                  "Identity":"212424808",
                  "Status":"Gold",
                  "Rating":"Adult",
                  "Email":"email@mail.com",
                  "Phone":"5564991681074",
                  "TravelLegs":[
                     {
                        "Origin":"AMS",
                        "Destination":"GIG"
                     }
                  ]
               }
            ]
         }
      }
   }
}

```

```shell

curl
--request POST "https://apisandbox.braspag.com.br/v2/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   "MerchantOrderId":"2017051002",
   "Customer":{
      "Name": "shopper Name",
      "Identity": "12345678910",
      "IdentityType": "CPF",
      "Email": "shopper@braspag.com.br",
      "Birthdate":"1991-01-02",
      "Phone": "5521976781114",
      "Address":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country": "BR",
         "District":"Alphaville"
      },
      "DeliveryAddress":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country": "BR",
         "District":"Alphaville"
      }
   },
   "Payment":{
      "Provider":"Simulado",
      "Type":"CreditCard",
      "Amount":10000,
      "Currency":"BRL",
      "Country":"BRA",
      "Installments":1,
      "Interest":"ByMerchant",
      "Capture":true,
      "Authenticate":false,
      "Recurrent":false,
      "SoftDescriptor":"Message",
      "DoSplit":false,
      "CreditCard":{
         "CardNumber":"455187******0181",
         "Holder": "Cardholder Name",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa",
         "SaveCard": "false"
      },
      "Credentials":{
         "code":"9999999",
         "key":"D8888888",
         "password":"LOJA9999999",
         "username":"#Braspag2018@NOMEDALOJA#",
         "signature":"001"
      },
      "ExtraDataCollection":[
         {
            "Name":"FieldName",
            "Value":"FieldValue"
         }
      ],
      "FraudAnalysis": {
         "Sequence": "AnalyseFirst",
         "SequenceCriteria": "Always",
         "Provider": "Cybersource",
         "CaptureOnLowRisk": false,
         "VoidOnHighRisk": false,
         "TotalOrderAmount": 10000,
         "FingerPrintId":"074c1ee676ed4998ab66491013c565e2",
         "Browser":{
            "CookiesAccepted":false,
            "Email": "shopper@braspag.com.br",
            "HostName":"Test",
            "IpAddress": "127.0.0.1",
            "Type": "Chrome"
         },
         "Cart": {
            "IsGift": false,
            "ReturnsAccepted": true,
            "Items":[
               {
                  "GiftCategory": "Undefined",
                  "HostHedge": "Off",
                  "NonSensicalHedge": "Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "Name":"ItemTest1",
                  "Quantity":1,
                  "Sku":"20170511",
                  "UnitPrice":10000,
                  "Risk":"High",
                  "TimeHedge":"Normal",
                  "Type":"AdultContent",
                  "VelocityHedge":"High"
               },
               {
                  "GiftCategory": "Undefined",
                  "HostHedge": "Off",
                  "NonSensicalHedge": "Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "Name":"ItemTest2",
                  "Quantity":1,
                  "Sku":"20170512",
                  "UnitPrice":10000,
                  "Risk":"High",
                  "TimeHedge":"Normal",
                  "Type":"AdultContent",
                  "VelocityHedge":"High"
               }
            ]
         },
         "MerchantDefinedFields":[
            {
               "Id": 2,
               "Value": "100"
            },
            {
               "Id": 4,
               "Value": "Web"
            },
            {
               "Id": 9,
               "Value":"SIM"
            }
         ],
         "Shipping":{
            "Addressee":"João das Couves",
            "Method":"LowCost",
            "Phone":"551121840540"
         },
         "Travel":{
            "JourneyType":"OneWayTrip",
            "DepartureTime":"2018-01-09 18:00",
            "Passengers":[
               {
                  "Name":"Passenger Test",
                  "Identity":"212424808",
                  "Status":"Gold",
                  "Rating":"Adult",
                  "Email":"email@mail.com",
                  "Phone":"5564991681074",
                  "TravelLegs":[
                     {
                        "Origin":"AMS",
                        "Destination":"GIG"
                     }
                  ]
               }
            ]
         }
      }
   }
}
--verbose

```

|Property|Type|Size|Mandatory|Description|
|-----------|----|-------|-----------|---------|
|`MerchantId`|GUID|36|Yes|Store identifier at Braspag|
|`MerchantKey`|Text|40|Yes|Public key for dual authentication with Braspag|
|`RequestId`|GUID|36|No|Store-defined request identifier|
|`MerchantOrderId`|Text|50|Yes|Order ID number.|
|`Customer.Name`|Text|120|Yes|Shopper's full name|
|`Customer.Identity`|Text|16|Yes|Shopper's ID|
|`Customer.IdentityType`|Text|255|No|Shopper's ID Type  <br/> Possible values: CPF or CNPJ|
|`Customer.Email`|Text|100|Yes|Shopper's Email|
|`Customer.Birthdate`|Date|10|Yes|Shopper's date of birth  <br/> E.g.: 1991-01-10|
|`Customer.Phone`|Text|15|Yes|Buyer Phone Number  <br/> E.g.: 5521976781114|
|`Customer.Address.Street`|Text|54|Yes|Billing Address Street|
|`Customer.Address.Number`|Text|5|Yes|Billing Address Number|
|`Customer.Address.Complement`|Text|14|No|Billing Address Supplement|
|`Customer.Address.ZipCode`|Text|9|Yes|Billing Address ZipCode|
|`Customer.Address.City`|Text|50|Yes|Billing Address City|
|`Customer.Address.State`|Text|2|Yes|Billing Address Status|
|`Customer.Address.Country`|Text|2|Yes|Billing Address Country. More information at [ISO 2-Digit Alpha Country Code](https://www.iso.org/obp/ui)|
|`Customer.Address.District`|Text|45|Yes|Billing Address Neighborhood|
|`Customer.DeliveryAddress.Street`|Text|54|No|Delivery Address Street|
|`Customer.DeliveryAddress.Number`|Text|5|No|Delivery Address Number|
|`Customer.DeliveryAddress.Complement`|Text|14|No|Delivery Address Supplement|
|`Customer.DeliveryAddress.ZipCode`|Text|9|No|Delivery Address Zipcode|
|`Customer.DeliveryAddress.City`|Text|50|No|Delivery Address City|
|`Customer.DeliveryAddress.State`|Text|2|No|Delivery Address Status|
|`Customer.DeliveryAddress.Country`|Text|2|No|Delivery address country. More information at [ISO 2-Digit Alpha Country Code](https://www.iso.org/obp/ui)|
|`Customer.DeliveryAddress.District`|Text|45|No|Delivery Address Neighborhood|
|`Payment.Provider`|Text|15|Yes|Name of authorization provider|
|`Payment.Type`|Text|100|Yes|Type of payment method. <br/> Note: Only _CreditCard_ type works with Fraud Analysis|
|`Payment.Amount`|Number|15|Yes|Financial transaction amount in cents  <br/> Ex: 150000 = $ 1,500.00|
|`Payment.ServiceTaxAmount`|Number|15|No|Applicable to airlines only. Amount of authorization amount to be allocated to service fee  <br/> Note: This amount is not added to authorization value|
|`Payment.Currency`|Text|3|No|Currency in which payment will be made <br/> possible values: BRL/USD/MXN/COP/PLC/ARS/PEN/EUR/PYN/UYU/VEB/VEF/GBP|
|`Payment.Country`|Text|3|No|Country in which payment will be made|
|`Payment.Installments`|Number|2|Yes|Number of installments|
|`Payment.Interest`|Text|10|No|Installment type  <br/> Possible values: ByMerchant/ByIssuer|
|`Payment.Capture`|Boolean|---|No|Indicates whether authorization should be auto-capture  <br/> Possible values: true/false (default)  <br/> Note: You should check with the acquirer for the availability of this feature  <br/> . Note2: This field should be completed according to fraud analysis flow|
|`Payment.Authenticate`|Boolean|---|No|Indicates whether the transaction should be authenticated  <br/> Possible values: true/false (default)  <br/> Note: You should check with the acquirer for the availability of this feature|
|`Payment.Recurrent`|Boolean|---|No|Indicates whether the transaction is of a recurring type  <br/> Possible values: true/false (default)  <br/> Note: This field equal to _true_ will not create a recurrence, it will only allow to perform of a transaction without the need to send the CVV and serving as an indication to the acquirer that is charging a transaction of a recurrence  <br/> Note2: For Cielo transactions only  <br/> Note3: The `Payment.Authenticate` field must be equal to _false_ when this equals _true_|
|`Payment.SoftDescriptor`|Text|13|No|Text that will be printed on the carrier  's invoice <br/> Note: The value of this field must be clear and easy to identify by the carrier the place of purchase as it is one of the Top Chargeback Offenders|
|`Payment.DoSplit`|Boolean|---|No|Indicates whether the transaction will be split among multiple participants  <br/> Possible values: true/false (default)  <br/> To use the split payment functionality, you must contract the solution with Braspag|
|`Payment.ExtraDataCollection.Name`|Text|50|No|Extra field identifier to be sent|
|`Payment.ExtraDataCollection.Value`|Text|1024|No|Extra field value to be sent|
|`Payment.Credentials.Code`|Text|100|Yes|Acquirer affiliation|
|`Payment.Credentials.Key`|Text|100|Yes|Affiliate/Token Key Generated by acquirer|
|`Payment.Credentials.Username`|Text|50|No|User generated by Acquirer Getnet <br/> Note: The field must be submitted if the transaction is directed to Getnet|
|`Payment.Credentials.Password`|Text|50|No|Password generated with the Acquirer Getnet <br/> Note: The field must be submitted if the transaction is directed to Getnet|
|`Payment.Credentials.Signature`|Text|3|No|Terminal ID with Acquirer Global Payments  <br/> Note: This field must be submitted if the transaction is directed to Global Payments|
|`Payment.CreditCard.CardNumber`|Text|16|Yes|Credit Card Number|
|`Payment.CreditCard.Holder`|Text|25|Yes|Holder name printed on credit card|
|`Payment.CreditCard.ExpirationDate`|Text|7|Yes|Credit Card Expiration Date|
|`Payment.CreditCard.SecurityCode`|Text|4|Yes|Security Code printed on the back of Credit Card|
|`Payment.CreditCard.Brand`|Text|10|Yes|Credit Card brand|
|`Payment.CreditCard.SaveCard`|Boolean|---|No|Indicates whether credit card data will be stored on Cartão Protegido|
|`Payment.CreditCard.Alias`|Text|64|No|Credit Card Alias (alias) saved to Cartão Protegido|
|`Payment.FraudAnalysis.Sequence`|Text|14|Yes|Fraud Analysis Flow Type  <br/> Possible Values: AnalyseFirst/AuthorizeFirst|
|`Payment.FraudAnalysis.SequenceCriteria`|Text|9|Yes|Fraud Analysis Flow Criteria  <br/> Possible Values: OnSuccess/Always|
|`Payment.FraudAnalysis.Provider`|Text|10|Yes|Anti-fraud provider  <br/> Possible values: Cybersource|
|`Payment.FraudAnalysis.CaptureOnLowRisk`|Boolean|---|No|Indicates whether transaction after fraud analysis will be captured  <br/> Possible values: true/false (default)  <br/> Note: When sent equal to _true_ and return of analysis of fraud is low risk (Accept) the previously authorized transaction will be captured  <br/> Note2: When sent equal to _true_ and the return of fraud analysis is Review the transaction will be authorized. It will be captured after Braspag receives notification of the status change and it is low risk (Accept)  <br/> Note: To use this parameter, the sequence of the risk analysis flow must be _AuthorizeFirst_|
|`Payment.FraudAnalysis.VoidOnHighRisk`|Boolean|---|No|Indicates if transaction after fraud analysis will be canceled  <br/> Possible values: true/false (default)  <br/> Note: When sent equal to _true_ and return of analysis If a fraud is high risk (Reject) the previously authorized transaction will be canceled  <br/> . Note2: When sent equal to _true_ and the return of the fraud analysis is Review the transaction will be authorized. It will be canceled after Braspag receives notification of the status change and it is high risk (Reject)  <br/> Note: To use this parameter, the sequence of the risk analysis flow must be _AuthorizeFirst_|
|`Payment.FraudAnalysis.TotalOrderAmount`|Number|15|Yes|Total order value in cents  <br/> Ex: 123456 = R $ 1,234.56|
|`Payment.FraudAnalysis.FingerPrintId`|Text|100|Yes|Identifier used to crosscheck information obtained from the shopper's device. This same identifier must be used to generate the value that will be assigned to the `session_id` field of the script that will be included in the checkout page. <br/> Note: This identifier can be any value or order number, but must be unique for 48 hours|
|`Payment.FraudAnalysis.Browser.HostName`|Text|60|No|Host name entered by the shopper's browser and identified through the HTTP header|
|`Payment.FraudAnalysis.Browser.CookiesAccepted`|Boolean|---|Yes|Identifies if the shopper's browser accepts cookies  <br/> Possible values: true/false (default)|
|`Payment.FraudAnalysis.Browser.Email`|Text|100|No|Email registered in the shopper's browser. May differ from store registration email (`Customer.Email`)|
|`Payment.FraudAnalysis.Browser.Type`|Text|40|No|Name of browser used by shopper and identified via HTTP header  <br/> E.g.: Google Chrome, Mozilla Firefox, Safari, etc|
|`Payment.FraudAnalysis.Browser.IpAddress`|Text|45|Yes|Shopper's IP Address. IPv4 or IPv6 format|
|`Payment.FraudAnalysis.Cart.IsGift`|Boolean|---|No|Indicates if the order placed by the shopper is for gift|
|`Payment.FraudAnalysis.Cart.ReturnsAccepted`|Boolean|---|No|Indicates whether the shopper's order can be returned to the store<br/> Possible values: true/false (default)|
|`Payment.FraudAnalysis.Cart.Items.GiftCategory`|Text|9|No|Identifies that it will evaluate the billing and delivery addresses for different cities, states or countries  <br/> [Value List - Payment.Fraudanalysis.Cart.Items {n} .GiftCategory]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.cart.tems[n].giftcategory|
|`Payment.FraudAnalysis.Cart.Items.HostHedge`|Text|6|No|Importance Level of Shopper IP Address and Email Address in Fraud Analysis  <br/> [Value List - Payment.Fraudanalysis.Cart.Items {n }.HostHedge]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.cart.items[n].hosthedge)|
|`Payment.FraudAnalysis.Cart.Items.NonSensicalHedge`|Text|6|No|Importance level of meaningless buyer data checks in fraud analysis  <br/> [List of Values - Cart.Items {n}.NonSensicalHedge]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.cart.items[n].nonsensicalhedge)|
|`Payment.FraudAnalysis.Cart.Items.ObscenitiesHedge`|Text|6|No|Importance level of checks on buyer data with obscenity in fraud analysis  <br/> [Value List - Payment.Fraudanalysis.Cart.Items {n} .ObscenitiesHedge]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.cart.items[n].obscenitieshedge)|
|`Payment.FraudAnalysis.Cart.Items.PhoneHedge`|Text|6|No|Importance level of checks on buyer phone numbers in fraud analysis  <br/> [List of Values - Payment.Fraudanalysis.Cart.Items {n} .PhoneHedge]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.cart.items[n].phonehedge)|
|`Payment.FraudAnalysis.Cart.Items.Name`|Text|255|Yes|Product Name|
|`Payment.FraudAnalysis.Cart.Items.Quantity`|Number|15|Yes|Product Quantity|
|`Payment.FraudAnalysis.Cart.Items.Sku`|Text|255|Yes|Stock Keeping Unit (SKU) of the product|
|`Payment.FraudAnalysis.Cart.Items.UnitPrice`|Number|15|Yes|Unit price  <br/> E.g.: 10950 = $ 109.50|
|`Payment.FraudAnalysis.Cart.Items.Risk`|Text|6|No|Product risk level associated with chargeback amount  <br/> [List of Values - Payment.Fraudanalysis.CartI.tems {n}.Risk]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.cart.items[n].risk)|
|`Payment.FraudAnalysis.Cart.Items.TimeHedge`|Text|6|No|Level of importance of the time of day in the fraud analysis that the buyer placed the order  <br/> [List of Payments - Payment.Fraudanalysis.Cart.Items {n }.TimeHedge]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.cart.items[n].timehedge)|
|`Payment.FraudAnalysis.Cart.Items.Type`|Text|19|No|Product Category  <br/> [Value List - Payment.Fraudanalysis.Cart.Items {n}.Type]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.cart.items[n].type)|
|`Payment.FraudAnalysis.Cart.Items.VelocityHedge`|Text|6|No|Importance level of buyer purchase frequency in fraud analysis within the previous 15 minutes  <br/> [List of Values - Payment.Fraudanalysis.Cart.Items { n}.VelocityHedge]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.cart.items[n].velocityhedge)|
|`Payment.FraudAnalysis.MerchantDefinedFields.Id`|Número|2|Sim|ID of additional information to be sent  <br/> [Tabela de MDDs]({{site.baseurl_root}}manual/braspag-pagador#tabela-de-mdds)|
|`Payment.FraudAnalysis.MerchantDefinedFields.Id`|Text|255|Yes|Value of additional information to be sent  <br/> [MDDs table]({{site.baseurl_root}}manual/braspag-pagador#tabela-de-mdds)|
|`Payment.FraudAnalysis.Shipping.Addressee`|Text|120|No|Full name of person responsible for receiving product at shipping address|
|`Payment.FraudAnalysis.Shipping.Method`|Texto|8|Não|Shipping Delivery Method  <br/> [Lista de Valores - Payment.Fraudanalysis.Shipping.Method]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.shipping.method)|
|`Payment.FraudAnalysis.Shipping.Phone`|Text|15|No|Phone number of the receiving party at shipping address  <br/> E.g.: 552121114700|
|`Payment.FraudAnalysis.Travel.JourneyType`|Texto|32|No|Trip Type <br/> [Value List - Payment.FraudAnalysis.Travel.JourneyType]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.travel.journeytype)|
|`Payment.FraudAnalysis.Travel.DepartureTime`|DateTime|---|No|Date and time of departure <br/> E.g.: 2018-03-31 19: 16: 38|
|`Payment.FraudAnalysis.Travel.Passengers.Name`|Text|120|No|Passenger Full Name|
|`Payment.FraudAnalysis.Travel.Passengers.Identity`|Text|32|No|Passenger Document Number|
|`Payment.FraudAnalysis.Travel.Passengers.Status`|Text|15|No|Airline Rating  <br/> [List of Values - Payment.FraudAnalysis.Travel.Passengers {n}.Status]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.travel.passengers[n].status)|
|`Payment.FraudAnalysis.Travel.Passengers.Rating`|Text|13|No|Passenger Type  <br/> [Value List - Payment.FraudAnalysis.Travel.Passengers {n}.PassengerType]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.travel.passengers[n].rating)|
|`Payment.FraudAnalysis.Travel.Passengers.Email`|Text|255|No|Passenger Email|
|`Payment.FraudAnalysis.Travel.Passengers.Phone`|Text|15|No|Passenger Phone  <br/> E.g.: 552121114700|
|`Payment.FraudAnalysis.Travel.Passengers.TravelLegs.Origin`|Text|3|No|Departure airport code. More information at [IATA 3-Letter Codes](http://www.nationsonline.org/oneworld/IATA_Codes/airport_code_list.htm)|
|`Payment.FraudAnalysis.Travel.Passengers.TravelLegs.Destination`|Text|3|No|Airport code of arrival. More information at [IATA 3-Letter Codes](http://www.nationsonline.org/oneworld/IATA_Codes/airport_code_list.htm)|

> The fields of the `FraudAnalysis.Travel` node become mandatory if your business segment is an airline company.

##### Response

```json
{
   "MerchantOrderId":"2017051002",
   "Customer":{
      "Name": "shopper Name",
      "Identity": "12345678910",
      "IdentityType": "CPF",
      "Email": "shopper@braspag.com.br",
      "Birthdate":"1991-01-02",
      "Phone": "5521976781114"
      "Address":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country": "BR",
         "District":"Alphaville"
      },
      "DeliveryAddress":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country": "BR",
         "District":"Alphaville"
      }
   },
   "Payment":{
      "Provider":"Simulado",
      "Type":"CreditCard",
      "Amount":10000,
      "Currency":"BRL",
      "Country":"BRA",
      "Installments":1,
      "Interest":"ByMerchant",
      "Capture":true,
      "Authenticate":false,
      "Recurrent":false,
      "SoftDescriptor":"Message",
      "DoSplit":false,
      "CreditCard":{
         "CardNumber":"455187******0181",
         "Holder": "Cardholder Name",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa",
         "SaveCard": "false"
      },
      "Credentials":{
         "code":"9999999",
         "key":"D8888888",
         "password":"LOJA9999999",
         "username":"#Braspag2018@NOMEDALOJA#",
         "signature":"001"
      },
      "ExtraDataCollection":[
         {
            "Name":"FieldName",
            "Value":"FieldValue"
         }
      ],
      "FraudAnalysis": {
         "Sequence": "AnalyseFirst",
         "SequenceCriteria": "Always",
         "Provider": "Cybersource",
         "CaptureOnLowRisk": false,
         "VoidOnHighRisk": false,
         "TotalOrderAmount": 10000,
         "FingerPrintId":"074c1ee676ed4998ab66491013c565e2",
         "Browser":{
            "CookiesAccepted":false,
            "Email": "shopper@braspag.com.br",
            "HostName":"Test",
            "IpAddress": "127.0.0.1",
            "Type": "Chrome"
         },
         "Cart": {
            "IsGift": false,
            "ReturnsAccepted": true,
            "Items":[
               {
                  "GiftCategory": "Undefined",
                  "HostHedge": "Off",
                  "NonSensicalHedge": "Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "Name":"ItemTest1",
                  "Quantity":1,
                  "Sku":"20170511",
                  "UnitPrice":10000,
                  "Risk":"High",
                  "TimeHedge":"Normal",
                  "Type":"AdultContent",
                  "VelocityHedge":"High"
               },
               {
                  "GiftCategory": "Undefined",
                  "HostHedge": "Off",
                  "NonSensicalHedge": "Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "Name":"ItemTest2",
                  "Quantity":1,
                  "Sku":"20170512",
                  "UnitPrice":10000,
                  "Risk":"High",
                  "TimeHedge":"Normal",
                  "Type":"AdultContent",
                  "VelocityHedge":"High"
               }
            ]
         },
         "MerchantDefinedFields":[
            {
               "Id": 2,
               "Value": "100"
            },
            {
               "Id": 4,
               "Value": "Web"
            },
            {
               "Id": 9,
               "Value":"SIM"
            }
         ],
         "Shipping":{
            "Addressee":"João das Couves",
            "Method":"LowCost",
            "Phone":"551121840540"
         },
         "Travel":{
            "JourneyType":"OneWayTrip",
            "DepartureTime":"2018-01-09 18:00",
            "Passengers":[
               {
                  "Name":"Passenger Test",
                  "Identity":"212424808",
                  "Status":"Gold",
                  "Rating":"Adult",
                  "Email":"email@mail.com",
                  "Phone":"5564991681074",
                  "TravelLegs":[
                     {
                        "Origin":"AMS",
                        "Destination":"GIG"
                     }
                  ]
               }
            ]
        },
        "Id": "0e4d0a3c-e424-4fa5-a573-4eabbd44da42",
        "Status": 1,
        "FraudAnalysisReasonCode": 100,
        "ReplyData": {
            "AddressInfoCode":"COR-BA^MM-BIN",
            "FactorCode":"B^D^R^Z",
            "Score":42,
            "BinCountry": "us",
            "CardIssuer":"FIA CARD SERVICES, N.A.",
            "CardScheme":"VisaCredit",
            "HostSeverity":1,
            "InternetInfoCode":"FREE-EM^RISK-EM",
            "IpRoutingMethod": "Undefined",
            "ScoreModelUsed": "default_lac",
            "CasePriority": 3,
            "ProviderTransactionId":"5220688414326697303008"
         }
      },
      "PaymentId": "c374099e-c474-4916-9f5c-f2598fec2925",
      "ProofOfSale": "20170510053219433",
      "AcquirerTransactionId": "0510053219433",
      "AuthorizationCode": "936403",
      "ReceivedDate": "2017-05-10 17:32:19",
      "CapturedAmount": 10000,
      "CapturedDate": "2017-05-10 17:32:19",
      "ReasonCode": 0,
      "ReasonMessage": "Successful",
      "Status": 2,
      "ProviderReturnCode": "6",
      "ProviderReturnMessage": "Operation Successful",
      "Links": [{
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/c374099e-c474-4916-9f5c-f2598fec2925"
      },
      {
        "Method": "PUT",
        "Rel": "void",
        "Href": "https://apisandbox.braspag.com.br/v2/sales/c374099e-c474-4916-9f5c-f2598fec2925/void"
      }]
   }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   "MerchantOrderId":"2017051002",
   "Customer":{
      "Name": "shopper Name",
      "Identity": "12345678910",
      "IdentityType": "CPF",
      "Email": "shopper@braspag.com.br",
      "Birthdate":"1991-01-02",
      "Phone": "5521976781114"
      "Address":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country": "BR",
         "District":"Alphaville"
      },
      "DeliveryAddress":{
         "Street":"Alameda Xingu",
         "Number":"512",
         "Complement":"27th floor",
         "ZipCode":"12345987",
         "City":"São Paulo",
         "State":"SP",
         "Country": "BR",
         "District":"Alphaville"
      }
   },
   "Payment":{
      "Provider":"Simulado",
      "Type":"CreditCard",
      "Amount":10000,
      "Currency":"BRL",
      "Country":"BRA",
      "Installments":1,
      "Interest":"ByMerchant",
      "Capture":true,
      "Authenticate":false,
      "Recurrent":false,
      "SoftDescriptor":"Message",
      "DoSplit":false,
      "CreditCard":{
         "CardNumber":"455187******0181",
         "Holder": "Cardholder Name",
         "ExpirationDate":"12/2021",
         "SecurityCode":"123",
         "Brand":"Visa",
         "SaveCard": "false"
      },
      "Credentials":{
         "code":"9999999",
         "key":"D8888888",
         "password":"LOJA9999999",
         "username":"#Braspag2018@NOMEDALOJA#",
         "signature":"001"
      },
      "ExtraDataCollection":[
         {
            "Name":"FieldName",
            "Value":"FieldValue"
         }
      ],
      "FraudAnalysis": {
         "Sequence": "AnalyseFirst",
         "SequenceCriteria": "Always",
         "Provider": "Cybersource",
         "CaptureOnLowRisk": false,
         "VoidOnHighRisk": false,
         "TotalOrderAmount": 10000,
         "FingerPrintId":"074c1ee676ed4998ab66491013c565e2",
         "Browser":{
            "CookiesAccepted":false,
            "Email": "shopper@braspag.com.br",
            "HostName":"Test",
            "IpAddress": "127.0.0.1",
            "Type": "Chrome"
         },
         "Cart": {
            "IsGift": false,
            "ReturnsAccepted": true,
            "Items":[
               {
                  "GiftCategory": "Undefined",
                  "HostHedge": "Off",
                  "NonSensicalHedge": "Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "Name":"ItemTest1",
                  "Quantity":1,
                  "Sku":"20170511",
                  "UnitPrice":10000,
                  "Risk":"High",
                  "TimeHedge":"Normal",
                  "Type":"AdultContent",
                  "VelocityHedge":"High"
               },
               {
                  "GiftCategory": "Undefined",
                  "HostHedge": "Off",
                  "NonSensicalHedge": "Off",
                  "ObscenitiesHedge":"Off",
                  "PhoneHedge":"Off",
                  "Name":"ItemTest2",
                  "Quantity":1,
                  "Sku":"20170512",
                  "UnitPrice":10000,
                  "Risk":"High",
                  "TimeHedge":"Normal",
                  "Type":"AdultContent",
                  "VelocityHedge":"High"
               }
            ]
         },
         "MerchantDefinedFields":[
            {
               "Id": 2,
               "Value": "100"
            },
            {
               "Id": 4,
               "Value": "Web"
            },
            {
               "Id": 9,
               "Value":"SIM"
            }
         ],
         "Shipping":{
            "Addressee":"João das Couves",
            "Method":"LowCost",
            "Phone":"551121840540"
         },
         "Travel":{
            "JourneyType":"OneWayTrip",
            "DepartureTime":"2018-01-09 18:00",
            "Passengers":[
               {
                  "Name":"Passenger Test",
                  "Identity":"212424808",
                  "Status":"Gold",
                  "Rating":"Adult",
                  "Email":"email@mail.com",
                  "Phone":"5564991681074",
                  "TravelLegs":[
                     {
                        "Origin":"AMS",
                        "Destination":"GIG"
                     }
                  ]
               }
            ]
        },
        "Id": "0e4d0a3c-e424-4fa5-a573-4eabbd44da42",
        "Status": 1,
        "FraudAnalysisReasonCode": 100,
        "ReplyData": {
            "AddressInfoCode":"COR-BA^MM-BIN",
            "FactorCode":"B^D^R^Z",
            "Score":42,
            "BinCountry": "us",
            "CardIssuer":"FIA CARD SERVICES, N.A.",
            "CardScheme":"VisaCredit",
            "HostSeverity":1,
            "InternetInfoCode":"FREE-EM^RISK-EM",
            "IpRoutingMethod": "Undefined",
            "ScoreModelUsed": "default_lac",
            "CasePriority": 3,
            "ProviderTransactionId":"5220688414326697303008"
         }
      },
      "PaymentId": "c374099e-c474-4916-9f5c-f2598fec2925",
      "ProofOfSale": "20170510053219433",
      "AcquirerTransactionId": "0510053219433",
      "AuthorizationCode": "936403",
      "ReceivedDate": "2017-05-10 17:32:19",
      "CapturedAmount": 10000,
      "CapturedDate": "2017-05-10 17:32:19",
      "ReasonCode": 0,
      "ReasonMessage": "Successful",
      "Status": 2,
      "ProviderReturnCode": "6",
      "ProviderReturnMessage": "Operation Successful",
      "Links": [{
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/c374099e-c474-4916-9f5c-f2598fec2925"
      },
      {
        "Method": "PUT",
        "Rel": "void",
        "Href": "https://apisandbox.braspag.com.br/v2/sales/c374099e-c474-4916-9f5c-f2598fec2925/void"
      }]
   }
}
```

|Property|Type|Description|
|:-|:-|:-|
|`MerchantOrderId`|Text|Order ID number.|
|`Customer.Name`|Text|Shopper's full name|
|`Customer.Identity`|Text|Shopper's identification number|
|`Customer.IdentityType`|Text|Shopper ID Type|
|`Customer.Email`|Text|Shopper's Email|
|`Customer.Birthdate`|Date|Shopper's Date of Birth|
|`Customer.Phone`|Text|Shopper's Phone Number|
|`Customer.Address.Street`|Text|Billing Address Street|
|`Customer.Address.Number`|Text|Billing Address Number|
|`Customer.Address.Complement`|Text|Billing Address Supplement|
|`Customer.Address.ZipCode`|Text|Billing Address Postcode|
|`Customer.Address.City`|Text|City of Billing Address|
|`Customer.Address.State`|Text|Billing Address State|
|`Customer.Address.Country`|Text|Billing Address Country|
|`Customer.Address.District`|Text|Billing Address Neighborhood|
|`Customer.DeliveryAddress.Street`|Text|Delivery Address Street|
|`Customer.DeliveryAddress.Number`|Text|Delivery Address Number|
|`Customer.DeliveryAddress.Complement`|Text|Delivery Address Supplement|
|`Customer.DeliveryAddress.ZipCode`|Text|Delivery Address Zipcode|
|`Customer.DeliveryAddress.City`|Text|Delivery Address City|
|`Customer.DeliveryAddress.State`|Text |Delivery Address Status|
|`Customer.DeliveryAddress.Country`|Text|Delivery address country.
|`Customer.DeliveryAddress.District`|Text|Delivery Address Neighborhood|
|`Payment.Provider`|Text|Name of authorization provider|
|`Payment.Type`|Text|Type of payment method|
|`Payment.Amount`|Number|Transaction amount in cents|
|`Payment.ServiceTaxAmount`|Number|Amount of authorization amount to be used for service charge|
|`Payment.Currency`|Text|Currency in which payment will be made|
|`Payment.Country`|Text|Country in which payment will be made|
|`Payment.Installments`|Number |Number of installments|
|`Payment.Interest`|Text|Installment Type|
|`Payment.Capture`|Boolean|Indicates whether authorization should be with automatic capture|
|`Payment.Authenticate`|Boolean|Indicates whether the transaction should be authenticated|
|`Payment.Recurrent`|Boolean|Indicates whether the transaction is a recurring type|
|`Payment.SoftDescriptor`|Text|Text that will be printed on the invoice|
|`Payment.DoSplit`|Boolean|Indicates whether the transaction will be split among multiple participants|
|`Payment.ExtraDataCollection.Name`|Text|Extra field identifier to be sent|
|`Payment.ExtraDataCollection.Value`|Text|Extra field value to be sent|
|`Payment.Credentials.Code`|Text|Acquirer affiliation|
|`Payment.Credentials.Key`|Text|Affiliate/Token Key generated by acquirer|
|`Payment.Credentials.Username`|Text|User generated on Accreditation with acquirer Getnet|
|`Payment.Credentials.Password`|Text|Password generated on Accreditation with Buyer Getnet|
|`Payment.Credentials.Signature`|Text|Terminal ID on affiliation with Acquirer Global Payments|
|`Payment.CreditCard.CardNumber`|Text|Credit Card Number|
|`Payment.CreditCard.Holder`|Text|Holder name printed on credit card|
|`Payment.CreditCard.ExpirationDate`|Text|Credit Card Expiration Date|
|`Payment.CreditCard.SecurityCode`|Text|Security Code printed on the back of Credit Card|
|`Payment.CreditCard.Brand`|Text|Credit Card brand|
|`Payment.CreditCard.SaveCard`|Boolean|Indicates whether credit card data will be stored on Cartão Protegido|
|`Payment.CreditCard.Alias`|Text |Credit Card Alias saved to Cartão Protegido|
|`Payment.CreditCard.CardToken`|GUID|Credit card identifier saved on Cartão Protegido|
|`Payment.FraudAnalysis.Sequence`|Text|Fraud Analysis Flow Type|
|`Payment.FraudAnalysis.SequenceCriteria`|Text|Fraud Analysis Flow Criteria|
|`Payment.FraudAnalysis.Provider`|Text|Anti-fraud provider|
|`Payment.FraudAnalysis.CaptureOnLowRisk`|Boolean|Indicates if transaction after fraud analysis will be captured|
|`Payment.FraudAnalysis.VoidOnHighRisk`|Boolean|Indicates whether the transaction after fraud analysis will be canceled|
|`Payment.FraudAnalysis.TotalOrderAmount`|Number|Total order value in cents|
|`Payment.FraudAnalysis.FingerPrintId`|Text|Identifier used to crosscheck information obtained from the shopper's device.
|`Payment.FraudAnalysis.Browser.HostName`|Text|Host name entered by the shopper's browser and identified through the HTTP header|
|`Payment.FraudAnalysis.Browser.CookiesAccepted`|Boolean|Identifies if the buyer's browser accepts cookies|
|`Payment.FraudAnalysis.Browser.Email`|Text|Email registered in the shopper's browser. May differ from store registration email (`Customer.Email`)|
|`Payment.FraudAnalysis.Browser.Type`|Text|Name of browser used by shopper and identified through HTTP header|
|`Payment.FraudAnalysis.Browser.IpAddress`|Text|Shopper's IP Address. IPv4 or IPv6 format|
|`Payment.FraudAnalysis.Cart.IsGift`|Boolean|Indicates if the order placed by the shopper is for gift|
|`Payment.FraudAnalysis.Cart.ReturnsAccepted`|Boolean|Indicates if the order placed by the buyer can be returned to the store|
|`Payment.FraudAnalysis.Cart.Items.GiftCategory`|Text|Identifies that will evaluate the billing and delivery addresses for different cities, states or countries|
|`Payment.FraudAnalysis.Cart.Items.HostHedge`|Text|Importance level of buyer IP and email addresses in fraud analysis|
|`Payment.FraudAnalysis.Cart.Items.NonSensicalHedge`|Text|Importance level of meaningless buyer data checks in fraud analysis|
|`Payment.FraudAnalysis.Cart.Items.ObscenitiesHedge`|Text|Importance level of buyer data checks with obscenity in fraud analysis|
|`Payment.FraudAnalysis.Cart.Items.PhoneHedge`|Text|Importance level of buyer phone number checks in fraud analysis|
|`Payment.FraudAnalysis.Cart.Items.Name`|Text|Product Name|
|`Payment.FraudAnalysis.Cart.Items.Quantity`|Number|Product Quantity|
|`Payment.FraudAnalysis.Cart.Items.Sku`|Text|Stock Keeping Unit (SKU) of the product|
|`Payment.FraudAnalysis.Cart.Items.UnitPrice`|Number|Unit price of the product|
|`Payment.FraudAnalysis.Cart.Items.Risk`|Text|Product risk level associated with chargeback amount|
|`Payment.FraudAnalysis.Cart.Items.TimeHedge`|Text|Level of importance of the time of day in the fraud analysis the buyer placed the order|
|`Payment.FraudAnalysis.Cart.Items.Type`|Text|Product Category|
|`Payment.FraudAnalysis.Cart.Items.VelocityHedge`|Text|Importance level of buyer purchase frequency in fraud analysis within 15 minutes|
|`Payment.FraudAnalysis.MerchantDefinedFields.Id`|Number|ID of the additional information to be sent|
|`Payment.FraudAnalysis.MerchantDefinedFields.Value`|Text|Value of additional information to be sent|
|`Payment.FraudAnalysis.Shipping.Addressee`|Text|Full name of person responsible for receiving product at shipping address|
|`Payment.FraudAnalysis.Shipping.Method`|Text|Order Delivery|
|`Payment.FraudAnalysis.Shipping.Phone`|Number|Phone number of the receiving party at shipping address|
|`Payment.FraudAnalysis.Travel.JourneyType`|Text|Trip Type|
|`Payment.FraudAnalysis.Travel.DepartureTime`|DateTime|Date and time of departure|
|`Payment.FraudAnalysis.Travel.Passengers.Name`|Text|Passenger Full Name|
|`Payment.FraudAnalysis.Travel.Passengers.Identity`|Text|Passenger Document Number|
|`Payment.FraudAnalysis.Travel.Passengers.Status`|Text|Airline Classification|
|`Payment.FraudAnalysis.Travel.Passengers.Rating`|Text|Passenger Type|
|`Payment.FraudAnalysis.Travel.Passengers.Email`|Text|Passenger Email|
|`Payment.FraudAnalysis.Travel.Passengers.Phone`|Number|Passenger Phone|
|`Payment.FraudAnalysis.Travel.Passengers.TravelLegs.Origin`|Text|Departure airport code|
|`Payment.FraudAnalysis.Travel.Passengers.TravelLegs.Destination`|Text|Airport code of arrival|
|`Payment.FraudAnalysis.Id`|GUID|Antifraud Transaction Id Braspag|
|`Payment.FraudAnalysis.Status`|Number|Antifraude Transaction Status Braspag  <br/> [List of Values - Payment.FraudAnalysis.Status]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.status)|
|`Payment.FraudAnalysis.FraudAnalysisReasonCode`|Number|Cybersouce Return Code [  <br/> List of Values - Payment.FraudAnalysis.FraudAnalysisReasonCode]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.fraudanalysisreasoncode)|
|`Payment.FraudAnalysis.ReplyData.AddressInfoCode`|Text|Codes indicate incompatibilities between buyer billing and delivery addresses  <br/> Codes are concatenated using the character ^ E.g.: COR-BA ^ MM-BIN  <br/> [List of Payments - Payment. FraudAnalysis.ReplyData.AddressInfoCode]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.replydata.addressinfocode)|
|`Payment.FraudAnalysis.ReplyData.FactorCode`|Text|Codes that affected the analysis score  <br/> Codes are concatenated using the ^ character. E.g.: B ^ D ^ R ^ Z <br/>[List of Values - ProviderAnalysisResult.AfsReply.FactorCode]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.replydata.factorcode)|
|`Payment.FraudAnalysis.ReplyData.Score`|Number|Score from fraud analysis. Value between 0 and 100|
|`Payment.FraudAnalysis.ReplyData.BinCountry`|Text|Card BIN country code used in the analysis. More information at [ISO 2-Digit Alpha Country Code](https://www.iso.org/obp/ui)|
|`Payment.FraudAnalysis.ReplyData.CardIssuer`|Text|Name of bank or credit card issuer|
|`Payment.FraudAnalysis.ReplyData.CardScheme`|Text|Card Brand|
|`Payment.FraudAnalysis.ReplyData.HostSeverity`|Number|Shopper's email domain risk level, from 0 to 5, where 0 is undetermined risk and 5 represents the highest risk|
|`Payment.FraudAnalysis.ReplyData.InternetInfoCode`|Text|Codes that indicate problems with the email address, IP address, or billing address  <br/> Codes are concatenated using the ^ character. E.g.: FREE-EM ^  RISK-EM <br/> [List of Payments - Payment.FraudAnalysis.ReplyData.InternetInfoCode]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.replydata.internetinfocode)|
|`Payment.FraudAnalysis.ReplyData.IpRoutingMethod`|Text|Buyer routing method obtained from the IP Address  <br/> [Value List - Payment.FraudAnalysis.ReplyData.IpRoutingMethod]({{site.baseurl_root}}manual/braspag-pagador#lista-de-valores-payment.fraudanalysis.replydata.iproutingmethod)|
|`Payment.FraudAnalysis.ReplyData.ScoreModelUsed`|Text|Name of the score model used in the analysis. If you have no template defined, the default template from Cybersource was used|
|`Payment.FraudAnalysis.ReplyData.CasePriority`|Number|Defines the priority level of merchant rules or profiles. The priority level ranges from 1 (highest) to 5 (lowest) and the default value is 3, and this will be assigned if you have not set the priority of rules or profiles. This field is only returned if the store subscribes to Enhanced Case Management|
|`Payment.FraudAnalysis.ReplyData.ProviderTransactionId`|Text|Transaction Id in Cybersource|
|`Payment.PaymentId`|GUID|Transaction identifier in Pagador Braspag|
|`Payment.AcquirerTransactionId`|Text|Transaction identifier on acquirer|
|`Payment.ProofOfSale`|Text|Voucher number at the acquirer (NSU - Unique Transaction Sequence Number)|
|`Payment.AuthorizationCode`|Text|Authorization Code on Purchaser|
|`Payment.ReceivedDate`|Datetime|Date the transaction was received at the Braspag Payer <br/> E.g.: 2018-01-16 16: 38: 19|
|`Payment.CapturedDate`|Datetime|Date the transaction was captured on the acquirer  <br/> E.g.: 2018-01-16 16: 38: 20|
|`Payment.CapturedAmount`|Number|Amount captured from transaction  <br/> E.g.: 123456 = $ 1,234.56|
|`Payment.ECI`|Text|Electronic Commerce Indicator. Code generated in a credit transaction with external authentication|
|`Payment.ReasonCode`|Text|Operation return code.|
|`Payment.ReasonMessage`|Text|Operation return message.|
|`Payment.Status`|Number|Transaction Status on Pagador <br/> [Transaction Status List]({{site.baseurl_root}}manual/braspag-pagador#lista-de-status-transação)|
|`Payment.ProviderReturnCode`|Text|Code returned by acquirer or bank|
|`Payment.ProviderReturnMessage`|Text|Message returned by acquirer or bank|

## Configuring Fingerprint

An important component of fraud analysis, Fingerprint is a Javascript that must be entered on your website to capture important data such as shopper's IP, browser version, operating system, etc.
Often, just cart data is not enough to warrant an assertive analysis. The data collected by Fingerprint complements the analysis and ensures your store is better protected.

This page describes how it works and how to set up fingerprint on your checkout and mobiles page.

### Cybersource

You will need to add two tags, *script* inside the *head* tag for proper performance and *noscript* inside the *body* tag, so that device data collection can be performed even if browser Javascript is disabled.

<aside class="warning">If the 2 code segments are not placed on the checkout page, the fraud analysis results may not be accurate.</aside>

**Domain**

|Environment|Description|
|:-|:-|
|`Testing`|Use h.online-metrix.net, which is the fingerprint server DNS, as shown in the HTML example below|
|`Production`|Change the domain to a local URL, and configure your web server to redirect this URL to h.online-metrix.net|

**Variables**
There are two variables to fill in the Javascript URL. The `org_id` and the` session_id`. `Org_id` is a default value according to the table below, while` session_id` is composed by concatenating the `ProviderMerchantId` and` FraudAnalysis.FingerPrintId` parameters, as shown below:

|Variable|Description|
|:-|:-|
|`org_id`|for Sandbox = 1snn5n9w  <br/> for Production = k8vif92e|
|`session_id`|`ProviderMerchantId` = Identifier of your Cybersource store. If not, contact Braspag  <br/> `FraudAnalysis.FingerPrintId` = Identifier used to crosscheck information obtained from the shopper's device.  <br/> Note: This identifier can be any value or order number, but must be unique for 48 hours.|

**Application**

The Javascript template is as follows:

![Example Code](https://braspag.github.io/images/braspag/af/exemploscriptdfp.png)

The variables, when properly filled in, would provide a URL similar to the example below:

![Url Example](https://braspag.github.io/images/braspag/af/urldfp.png)

<aside class="warning">Be sure to copy all data correctly and have replaced the variables correctly with their values.</aside>

**Setting Up Your Web Server**

All objects refer to h.online-metrix.net, which is the fingerprint server's DNS. When you are ready for production, you should change the server name to a local URL, and set up on your web server a URL redirection to h.online-metrix.net.

<aside class="warning">If you do not complete this section, you will not receive correct results, and the fingerprint provider domain (URL) will be visible, and your consumer is more likely to block it.</aside>

### Mobile Application Integration

**Downloading the SDK**
If you have not yet downloaded the iOS or Android SDK, you must do so before continuing. To do so, access one of the links below as desired.<br/> [Download iOS SDK Deviceprint]({{site.baseurl_root}}/files/braspag/antifraude/cybersource-iossdk-fingerprint-v5.0.32.zip)  <br/> [Download Android SDK Deviceprint]({{site.baseurl_root}}/files/braspag/antifraude/cybersource-androidsdk-fingerprint-v5.0.96.zip)

# Queries

## Querying a Transaction via PaymentID

<aside class="notice"><strong>Rule:</strong>
<ul>
<li>Transaction with life up to 3 months - consultation via API or Admin Panel Braspag</a></li>
<li>Transaction with life from 3 months to 12 months - only via consultation on Admin Braspag Panel</a> with “History” option selected</li>
<li>Transaction with life over 12 months - contact your Braspag Commercial Executive</li>
</ul>
</aside>

To query a credit card transaction, you must do a GET to the Payment feature as shown.

#### Request

<aside class="request"><span class="method get">GET</span> <span class="endpoint">/v2/sales/{PaymentId}</span></aside>

```shell
curl
--request GET "https://apiquerysandbox.braspag.com.br/v2/sales/{PaymentId}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`PaymentId`|Payment identification number.|Text|36|Yes (through endpoint)|

##### Response

```json
{
   "MerchantOrderId": "2017051001",
   "Customer": {
      "Name": "Nome do Cliente",
      "Identity": "01234567789",
      "Email": "cliente@email.com.br",
      "Address": {
         "Street": "GONCALO DA CUNHA",
         "Number": "111",
         "ZipCode": "04140040",
         "City": "SAO PAULO",
         "State": "SP",
         "Country": "BRA",
         "District": "CHACARA INGLESA"
      }
   },
   "Merchant": {
      "Id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "TradeName": "Lojas Teste"
   },
   "Payment": {
      "ServiceTaxAmount": 0,
      "Installments": 1,
      "Interest": "ByMerchant",
      "Capture": true,
      "Authenticate": false,
      "Recurrent": false,
      "CreditCard": {
         "CardNumber": "455187******0181",
         "Holder": "Nome do Portador",
         "ExpirationDate": "12/2021",
         "Brand": "Visa"
      },
      "ProofOfSale": "2539492",
      "AcquirerTransactionId": "0510042539492",
      "AuthorizationCode": "759497",
      "Eci": "0",
      "Refunds": [
         {
            "Amount": 10000,
            "Status": 3,
            "ReceivedDate": "2017-05-15 16:25:38"
         }
      ],
      "Chargebacks": [
         {
            "Amount": 10000,
            "CaseNumber": "123456",
            "Date": "2017-06-04",
            "ReasonCode": "104",
            "ReasonMessage": "Outras Fraudes - Cartao Ausente",
            "Status": "Received",
            "RawData": "Client did not participate and did not authorize transaction"
         }
      ],
      "FraudAlert": {
         "Date": "2017-05-20",
         "ReasonMessage": "Uso Ind Numeração",
         "IncomingChargeback": false
      },
      "VelocityAnalysis": {
         "Id": "f8078b32-be17-4c35-b164-ad74c3cd0725",
         "ResultMessage": "Accept",
         "Score": 0
      },
      "PaymentId": "f8078b32-be17-4c35-b164-ad74c3cd0725",
      "Type": "CreditCard",
      "Amount": 10000,
      "ReceivedDate": "2017-05-10 16:25:38",
      "CapturedAmount": 10000,
      "CapturedDate": "2017-05-10 16:25:38",
      "VoidedAmount": 10000,
      "VoidedDate": "2017-05-15 16:25:38",
      "Currency": "BRL",
      "Country": "BRA",
      "Provider": "Simulado",
      "ProviderDescription": "Simulado",
      "ReasonCode": 0,
      "Status": 1,
      "Links": [
         {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725"
         },
         {
            "Method": "PUT",
            "Rel": "capture",
            "Href": "https://apisandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725/capture"
         },
         {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725/void"
         }
      ]
   }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   "MerchantOrderId": "2017051001",
   "Customer": {
      "Name": "Nome do Cliente",
      "Identity": "01234567789",
      "Email": "cliente@email.com.br",
      "Address": {
         "Street": "GONCALO DA CUNHA",
         "Number": "111",
         "ZipCode": "04140040",
         "City": "SAO PAULO",
         "State": "SP",
         "Country": "BRA",
         "District": "CHACARA INGLESA"
      }
   },
   "Merchant": {
      "Id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "TradeName": "Lojas Teste"
   },
   "Payment": {
      "ServiceTaxAmount": 0,
      "Installments": 1,
      "Interest": "ByMerchant",
      "Capture": true,
      "Authenticate": false,
      "Recurrent": false,
      "CreditCard": {
         "CardNumber": "455187******0181",
         "Holder": "Nome do Portador",
         "ExpirationDate": "12/2021",
         "Brand": "Visa"
      },
      "ProofOfSale": "2539492",
      "AcquirerTransactionId": "0510042539492",
      "AuthorizationCode": "759497",
      "Eci": "0",
      "Refunds": [
         {
            "Amount": 10000,
            "Status": 3,
            "ReceivedDate": "2017-05-15 16:25:38"
         }
      ],
      "Chargebacks": [
         {
            "Amount": 10000,
            "CaseNumber": "123456",
            "Date": "2017-06-04",
            "ReasonCode": "104",
            "ReasonMessage": "Outras Fraudes - Cartao Ausente",
            "Status": "Received",
            "RawData": "Client did not participate and did not authorize transaction"
         }
      ],
      "FraudAlert": {
         "Date": "2017-05-20",
         "ReasonMessage": "Uso Ind Numeração",
         "IncomingChargeback": false
      },
      "VelocityAnalysis": {
         "Id": "f8078b32-be17-4c35-b164-ad74c3cd0725",
         "ResultMessage": "Accept",
         "Score": 0
      },
      "PaymentId": "f8078b32-be17-4c35-b164-ad74c3cd0725",
      "Type": "CreditCard",
      "Amount": 10000,
      "ReceivedDate": "2017-05-10 16:25:38",
      "CapturedAmount": 10000,
      "CapturedDate": "2017-05-10 16:25:38",
      "VoidedAmount": 10000,
      "VoidedDate": "2017-05-15 16:25:38",
      "Currency": "BRL",
      "Country": "BRA",
      "Provider": "Simulado",
      "ProviderDescription": "Simulado",
      "ReasonCode": 0,
      "Status": 1,
      "Links": [
         {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725"
         },
         {
            "Method": "PUT",
            "Rel": "capture",
            "Href": "https://apisandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725/capture"
         },
         {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725/void"
         }
      ]
   }
}
```

|Property|Description|Type|Size|Format|
|--------|-----------|----|----|-------|
|`MerchantOrderId`|Order ID number.|Text|50|Alphanumeric|
|`Customer.Name`|Customer's name.|Text|255|Alphanumeric|
|`Customer.Identity`|Customer ID, CPF or CNPJ number|Text|14|Alphanumeric text|
|`Customer.IdentityType`|Type of shopper identification document (CPF or CNPJ)|Text|255|CPF or CNPJ|
|`Customer.Email`|Shopper Email|Text|255|Alphanumeric|
|`Customer.Birthdate`|Shopper's Date of Birth|Date|10|format YYYY-MM-DD|
|`Customer.Address.Street`|Shopper's Contact Address|Text|255|Alphanumeric|
|`Customer.Address.Number`|Shopper's Contact Number|Text|15|Alphanumeric|
|`Customer.Address.Complement`|Shopper's Contact Address Supplement|Text|50|Alphanumeric|
|`Customer.Address.ZipCode`|Shopper's Contact Address Zip Code|Text|9|Alphanumeric|
|`Customer.Address.City`|City of Shopper's contact address|Text|50|Alphanumeric|
|`Customer.Address.State`|Shopper's Contact Address Status|Text|2|Alphanumeric|
|`Customer.Address.Country`|Shopper's Contact Address Country|Text|35|Alphanumeric|
|`Customer.Address.District`|Shopper's Neighborhood|Text|50|Alphanumeric|
|`Customer.DeliveryAddress.Street`|Shopper's Address|Text|255|Alphanumeric|
|`Customer.DeliveryAddress.Number`|Order Delivery Address Number|Text|15|Alphanumeric|
|`Customer.DeliveryAddress.Complement`|Order Delivery Address Supplement|Text|50|Alphanumeric|
|`Customer.DeliveryAddress.ZipCode`|Order Delivery Address Zip Code|Text|9|Alphanumeric|
|`Customer.DeliveryAddress.City`|Order Delivery Address City|Text|50|Alphanumeric|
|`Customer.DeliveryAddress.State`|Order Delivery Address Status|Text|2|Alphanumeric|
|`Customer.DeliveryAddress.Country`|Order Delivery Address Country|Text|35|Alphanumeric|
|`Customer.DeliveryAddress.District`|Shopper's neighborhood.|Text|50|Alphanumeric|
|`Merchant.Id`|MerchantID where the transaction was made|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`Merchant.TradeName`|Store name|50|Alphanumeric|--|
|`Payment.Provider`|Name of Payment Provider|Text|15|Consult the providers available in the annexes|
|`Payment.Country`|Country in which payment will be made|Text|3|BRA|
|`Payment.Type`|Payment Medium Type|Text|100|E.g.: CreditCard|
|`Payment.Amount`|Order Value (in cents)|Number|15|10000|
|`Payment.ServiceTaxAmount`|Number|Amount of authorization amount to be used for service charge|Note: This value is not added to the authorization value|Number|15|10000|
|`Payment.Currency`|Currency in which payment will be made|Text|3|BRL/USD/MXN/COP/PLC/ARS/PEN/EUR/PYN/UYU/VEB/VEF/GBP|
|`Payment.Installments`|Number of Installments|Number|2|6|
|`Payment.Interest`|Installment Type|Text|10|Shop (ByMerchant) or Issuer (ByIssuer)|
|`Payment.Capture`|Boolean which indicates whether the authorization should be with automatic capture (true) or not (false). You should check with the acquirer for the availability of this feature|Boolean|--- (Default false)|Boolean|
|`AcquirerTransactionId`|Transaction Id of the Payment Method Provider|Text|40|Alphanumeric|
|`Payment.Authenticate`|Boolean indicating whether the transaction should be authenticated (true) or not (false). You should check with the acquirer for the availability of this feature|Boolean|--- (Default false)|Boolean|
|`Payment.Recurrent`|Boolean that indicates whether the transaction is a recurring (true) or not (false) transaction. This value of true will not give rise to a new Recurrence, it will only allow the execution of a transaction without the need to send CVV. For Cielo transactions only. Authenticate must be false when Recurrent is true|Boolean|--- (Default false)|Boolean|
|`Payment.SoftDescriptor`|Text to be printed on invoice|Text|13|Alphanumeric text|
|`Payment.ExtraDataCollection.Name`|Name of the field to be written Extra Data|Text|50|Alphanumeric text|
|`Payment.ExtraDataCollection.Value`|Value of the field to be written Extra Data|Text|1024|Alphanumeric text|
|`ProofOfSale`|Proof of Sale Number|Text|20|Alphanumeric|
|`Payment.AuthorizationCode`|Authorization code|Text|300|Alphanumeric text|
|`Payment.Refunds.Amount`|Refunded Amount|Number|15|Amount in cents|
|`Payment.Refunds.Status`|Refund status|Number|1|Received = 1, Sent = 2, Approved = 3, Denied = 4, Rejected = 5|
|`Payment.Refunds.ReceivedDate`|Refund date|Text|19|AAAA-MM-DD HH:mm:SS|E.g.: "2018-06-19 01:45:57"|
|`Payment.Chargebacks [n].Amount`|Chargeback amount|Number|15|10000|
|`Payment.Chargebacks [n].CaseNumber`|Chargeback-related case number|Text|16|Alphanumeric text|
|`Payment.Chargebacks [n].Date`|Chargeback Date|Date|10|YYYY-MM-DD|
|`Payment.Chargebacks [n].ReasonCode`|Chargeback Reason Code|Text|10|Alphanumeric|
|`Payment.Chargebacks [n].ReasonMessage`|Chargeback Reason Message|Text|512|Alphanumeric|
|`Payment.Chargebacks [n].Chargeback Status`|[Value List - Payment.Chargebacks {n}.Status]({{site.baseurl_root}}/manual/braspag-pagador#lista-de-valores-payment.chargebacks[n].status)|Text|32|Text|
|`Payment.Chargebacks [n].RawData`|Data sent by the acquirer and may be the cardholder or other message|Text|512|Alphanumeric text|
|`PaymentId`|Order Identifier Field|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReceivedDate`|Date the transaction was received by Braspag|Text|19|YYYY-MM-DD HH:mm:SS|
|`Payment.ReasonCode`|Acquisition Return Code|Text|32|Alphanumeric|
|`Payment.ReasonMessage`|Acquisition Return Message|Text|512|Alphanumeric|
|`Payment.CapturedAmount`|Valor capturado|Número|15|10000|
|`Payment.CapturedDate`|Data da captura|Texto|19|AAAA-MM-DD HH:mm:SS|Ex. "2018-06-19 01:45:57"|
|`Payment.VoidedAmount`|Valor cancelado/estornado|Número|15|10000|
|`Payment.VoidedDate`|Data do cancelamento/estorno|Texto|19|AAAA-MM-DD HH:mm:SS|Ex. "2018-06-19 01:45:57"|
|`Payment.Status`|Transaction Status|Byte|2|E.g.: 1|
|`Payment.Provider`|Provider used|Texto|32|Simulado|
|`Payment.ProviderDescription`|Acquirer's name|Texto|512|Simulado|
|`CreditCard.CardNumber`|Shopper's Card Number|Text|16|
|`CreditCard.Holder`|Name of cardholder printed on card|Text|25|
|`CreditCard.ExpirationDate`|Expiration Date Printed on the Card|Text|7|
|`CreditCard.Brand`|Card Brand|Text|10|
|`CreditCard.SaveCard`|Boolean which identifies whether the card will be saved to generate the token (CardToken)|Boolean|--- (Default false)|

## Querying a Boleto transaction via PaymentID

<aside class="notice"><strong>Rule:</strong>
<ul>
<li>Transaction with life up to 3 months - consultation via API or Admin Panel Braspag</a></li>
<li>Transaction with life from 3 months to 12 months - only via consultation on Admin Braspag Panel</a> with “History” option selected</li>
<li>Transaction with life over 12 months - contact your Braspag Commercial Executive</li>
</ul>
</aside>

To query a credit card transaction, you must do a GET to the Payment feature as shown.

#### Request

<aside class="request"><span class="method get">GET</span> <span class="endpoint">/v2/sales/{PaymentId}</span></aside>

```shell
curl
--request GET "https://apiquerysandbox.braspag.com.br/v2/sales/{PaymentId}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`PaymentId`|Payment identification number.|GUID|36|Yes (through endpoint)|

##### Response

```json
{
  "MerchantOrderId": "2017051001",
  "Customer":{
    "Name": "Customer Name",
    "Identity":"12345678909",
    "Address":{
        "Street": "GONCALO DA CUNHA",
        "Number": "111",
        "ZipCode": "04140040",
        "City": "SAO PAULO",
        "State":"SP",
        "Country":"BRA",
        "District": "CHACARA INGLESA"
     }
  },
  "Payment":{
     "Instructions": "",
     "ExpirationDate": "2018-06-27",
     "Demonstrative": "",
     "Url": "https://www.pagador.com.br/post/pagador/reenvia.asp/3fda2279-1c45-4271-9656-XXXXXXXXXX",
     "BoletoNumber": "123464",
     "BarCodeNumber": "9999990276000001234864001834099999999",
     "DigitableLine": "99999.39027 60000.012348 64001.834007 7 75680999999999",
     "Assignor": "RAZAO SOCIAL DA LOJA LTDA.",
     "Address": "",
     "Identification": "01234567000189",
     "CreditDate": "2018-06-28",
     "PaymentId": "99992279-1c45-4271-9656-ccbde4ea9999",
     "Type": "Boleto",
     "Amount": 182000,
     "ReceivedDate": "2018-06-26 23:33:07",
     "CapturedAmount": 182000,
     "CapturedDate": "2018-06-27 01:45:57",
     "Currency":"BRL",
     "Country":"BRA",
     "Provider": "Bradesco2",
     "ReturnUrl": "https://www.loja.com.br/notificacao",
     "ExtraDataCollection": [],
     "ReasonCode": 0,
     "Status": 2,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725"
      },
      {
        "Method": "PUT",
        "Rel": "capture",
        "Href": "https://apisandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725/capture"
      },
      {
        "Method": "PUT",
        "Rel": "void",
        "Href": "https://apisandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725/void"
      }
    ]
  }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  "MerchantOrderId": "2017051001",
  "Customer":{
    "Name": "Customer Name",
    "Identity":"12345678909",
    "Address":{
        "Street": "GONCALO DA CUNHA",
        "Number": "111",
        "ZipCode": "04140040",
        "City": "SAO PAULO",
        "State":"SP",
        "Country":"BRA",
        "District": "CHACARA INGLESA"
     }
  },
  "Payment":{
     "Instructions": "",
     "ExpirationDate": "2018-06-27",
     "Demonstrative": "",
     "Url": "https://www.pagador.com.br/post/pagador/reenvia.asp/3fda2279-1c45-4271-9656-XXXXXXXXXX",
     "BoletoNumber": "123464",
     "BarCodeNumber": "9999990276000001234864001834099999999",
     "DigitableLine": "99999.39027 60000.012348 64001.834007 7 75680999999999",
     "Assignor": "RAZAO SOCIAL DA LOJA LTDA.",
     "Address": "",
     "Identification": "01234567000189",
     "CreditDate": "2018-06-28",
     "PaymentId": "99992279-1c45-4271-9656-ccbde4ea9999",
     "Type": "Boleto",
     "Amount": 182000,
     "ReceivedDate": "2018-06-26 23:33:07",
     "CapturedAmount": 182000,
     "CapturedDate": "2018-06-27 01:45:57",
     "Currency":"BRL",
     "Country":"BRA",
     "Provider": "Bradesco2",
     "ReturnUrl": "https://www.loja.com.br/notificacao",
     "ExtraDataCollection": [],
     "ReasonCode": 0,
     "Status": 2,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725"
      },
      {
        "Method": "PUT",
        "Rel": "capture",
        "Href": "https://apisandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725/capture"
      },
      {
        "Method": "PUT",
        "Rel": "void",
        "Href": "https://apisandbox.braspag.com.br/v2/sales/f8078b32-be17-4c35-b164-ad74c3cd0725/void"
      }
    ]
  }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`MerchantOrderId`|Order ID number.|Text|50|Alphanumeric|
|`Customer.Name`|Customer's name.|Text|255|Alphanumeric|
|`Customer.Identity`|Customer ID, CPF or CNPJ number|Text|14|Alphanumeric text|
|`Customer.IdentityType`|Type of shopper identification document (CPF or CNPJ)|Text|255|CPF or CNPJ|
|`Customer.Email`|Shopper Email|Text|255|Alphanumeric|
|`Customer.Birthdate`|Shopper's Date of Birth|Date|10|format YYYY-MM-DD|
|`Customer.Address.Street`|Shopper's Contact Address|Text|255|Alphanumeric|
|`Customer.Address.Number`|Shopper's Contact Number|Text|15|Alphanumeric|
|`Customer.Address.Complement`|Shopper's Contact Address Supplement|Text|50|Alphanumeric|
|`Customer.Address.ZipCode`|Shopper's Contact Address Zip Code|Text|9|Alphanumeric|
|`Customer.Address.City`|City of Shopper's contact address|Text|50|Alphanumeric text|
|`Customer.Address.State`|Shopper's Contact Address Status|Text|2|Alphanumeric|
|`Customer.Address.Country`|Shopper's Contact Address Country|Text|35|Alphanumeric|
|`Customer.Address.District`|Shopper's Neighborhood|Text|50|Alphanumeric|
|`Customer.DeliveryAddress.Street`|Shopper's Address|Text|255|Alphanumeric|
|`Customer.DeliveryAddress.Number`|Order Delivery Address Number|Text|15|Alphanumeric|
|`Customer.DeliveryAddress.Complement`|Order Delivery Address Supplement|Text|50|Alphanumeric|
|`Customer.DeliveryAddress.ZipCode`|Order Delivery Address Zip Code|Text|9|Alphanumeric|
|`Customer.DeliveryAddress.City`|Order Delivery Address City|Text|50|Alphanumeric|
|`Customer.DeliveryAddress.State`|Order Delivery Address Status|Text|2|Alphanumeric|
|`Customer.DeliveryAddress.Country`|Order Delivery Address Country|Text|35|Alphanumeric|
|`Customer.DeliveryAddress.District`|Shopper's neighborhood.|Text|50|Alphanumeric|
|`Payment.Provider`|Name of Payment Provider|Text|15|Consult the providers available in the annexes|
|`Payment.Type`|Payment Medium Type|Text|100|E.g.: Boleto|
|`Payment.Amount`|Order Value (in cents)|Number|15|10000|
|`Payment.CapturedAmount`|Amount paid from the ticket (in cents)|Number|15|10000|
|`Payment.Instructions`|Text about some specific statement for the boleto|Text|See Banks table|E.g.: "Do not pay after expiration date"|
|`Payment.Demonstrative`|Text about some  boleto specific information|Text|See Banks table|E.g: "Boleto for the Order #99999"|
|`Payment.Url`|URL for bill of exchange|Text|-|E.g.: "https://www.pagador.com.br/post/pagador/reenvia.asp/3fda2279-1c45-4271-9656-XXXXXXXXXX"|
|`Payment.BoletoNumber`|Nosso Número|Text|See Banks Table|E.g.: "12345678"|
|`Payment.BarCodeNumber`|Boleto's Bar Code|Text|44|E.g.: "99999390276000001234864001834007775680099999"|
|`Payment.DigitableLine`|Digitable Boleto line|Text|54|E.g.: "99999.39027 60000.012348 64001.834007 7 75680000199999"|
|`Payment.Assignor`|Name of the boleto's assignor|Text|200|E.g.: "SHOP LTDA SOCIAL REASON"|
|`Payment.Address`|Address of the boleto's assignor|Text|160|E.g.: "Alameda Xingu 512"|
|`Payment.Identification`|CNPJ of the assignor|Text|18|E.g.: "11.355.111/0001-11"|
|`Payment.ExpirationDate`|Boleto Due Date|Text|YYYY-MM-DD|E.g.: "2018-06-21"|
|`Payment.CreditDate`|Boleto's liquidation date|Text|YYYY-MM-DD|E.g.: "2018-06-19"|
|`Payment.CapturedDate`|Bill Payment Date|Text|YYYY-MM-DD HH:mm:SS|E.g.: "2018-06-19 01:45:57"|
|`Payment.ReceivedDate`|Date the transaction was received by Braspag|Text|YYYY-MM-DD HH:mm:SS|"2018-06-19 01:45:57"|
|`Payment.ReturnUrl`|URL of the store to which the customer redirects|Text|-|E.g.: "https://www.loja.com.br"|
|`Payment.Currency`|Currency in which payment will be made|Text|3|BRL/USD/MXN/COP/PLC/ARS/PEN/EUR/PYN/UYU/VEB/VEF/GBP|
|`Payment.Country`|Country in which payment will be made|Text|3|BRA|
|`Payment.ExtraDataCollection.Name`|Name of the field to be written Extra Data|Text|50|Alphanumeric text|
|`Payment.ExtraDataCollection.Value`|Value of the field to be written Extra Data|Text|1024|Alphanumeric text|
|'PaymentId'|Order Identifier Field|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`Payment.ReasonCode`|Acquisition Return Code|Text|32|Alphanumeric|
|`Payment.Status`|Transaction Status|Byte|2|E.g.: 1|

## Querying a Sale by Store Identifier

<aside class="notice"><strong>Rule:</strong>
<ul>
<li>Transaction with life up to 3 months - consultation via API or Admin Panel Braspag</a></li>
<li>Transaction with life from 3 months to 12 months - only via consultation on Admin Braspag Panel</a> with “History” option selected</li>
<li>Transaction with life over 12 months - contact your Braspag Commercial Executive</li>
</ul>
</aside>

You cannot directly query a payment for the store-submitted identifier (MerchantOrderId), but you can get all PaymentIds associated with the identifier.

To query a sale by store identifier, you must GET the resource/sales as shown.

#### Request

<aside class="request"><span class="method get">GET</span> <span class="endpoint">/v2/sales?merchantOrderId = {merchantOrderId}</span></aside>

```shell
--request GET "https://apiquerysandbox.braspag.com.brv2/sales?merchantOrderId={merchantOrderId}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`MerchantOrderId`|Order ID number.|Text|36|Yes (through endpoint)|

##### Response

```json
{
    "Payment": [
        {
            "PaymentId": "5fb4d606-bb63-4423-a683-c966e15399e8",
            "ReceveidDate": "2015-04-06T10:13:39.42"
        },
        {
            "PaymentId": "6c1d45c3-a95f-49c1-a626-1e9373feecc2",
            "ReceveidDate": "2014-12-19T20:23:28.847"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    Payments: [
        {
            "PaymentId": "5fb4d606-bb63-4423-a683-c966e15399e8",
            "ReceveidDate": "2015-04-06T10:13:39.42"
        },
        {
            "PaymentId": "6c1d45c3-a95f-49c1-a626-1e9373feecc2",
            "ReceveidDate": "2014-12-19T20:23:28.847"
        }
    ]
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`PaymentId`|Order Identifier field|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|

## Querying a Recurring Order

To query a Recurrence request, you need to get a GET as per the example.

#### Request

<aside class="request"><span class="method get">GET</span> <span class="endpoint">/v2/RecurrentPayment/{RecurrentPaymentId}</span></aside>

```shell
curl
--request GET "https://apiquerysandbox.braspag.com.br/v2/RecurrentPayment/{RecurrentPaymentId}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`MerchantId`|API Store Identifier|GUID|36|Yes (through header)|
|`MerchantKey`|Public Key for Dual Authentication in API|Text|40|Yes (through header)|
|`RequestId`|Store-defined Request identifier used when the merchant uses different servers for each GET/POST/PUT|GUID|36|No (through header)|
|`RecurrentPaymentId`|Recurrence Identifier field.|Text|36|Yes (through endpoint)|

##### Response

```json
{
  "Customer":{
    "Name": "Customer Name"
  },
  "RecurrentPayment": {
    "Installments":1,
    "RecurrentPaymentId": "f5a83c14-0254-4e73-bdd3-9afba1007266",
    "NextRecurrency": "2017-06-11",
    "StartDate": "2017-05-11",
    "EndDate":"2019-12-31",
    "Interval": "Monthly",
    "Amount":10000,
    "Country":"BRA",
    "CreateDate": "2017-05-11T00:00:00",
    "Currency":"BRL",
    "CurrentRecurrencyTry": 1,
    "OrderNumber": "2017051120",
    "Provider":"Simulado",
    "RecurrencyDay": 11,
    "SuccessfulRecurrences": 1,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.braspag.com.br/v2/RecurrentPayment/f5a83c14-0254-4e73-bdd3-9afba1007266"
      }
    ],
    "RecurrentTransactions": [
      {
        "PaymentNumber": 0,
        "RecurrentPaymentId": "f5a83c14-0254-4e73-bdd3-9afba1007266",
        "TransactionId": "cd694ffb-c0c4-47db-9390-737df70a2012",
        "TryNumber": 1,
        "Links": [
          {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/cd694ffb-c0c4-47db-9390-737df70a2012"
          }
        ]
      }
    ],
    "Status": 1
  }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  "Customer":{
    "Name": "Customer Name"
  },
  "RecurrentPayment": {
    "Installments":1,
    "RecurrentPaymentId": "f5a83c14-0254-4e73-bdd3-9afba1007266",
    "NextRecurrency": "2017-06-11",
    "StartDate": "2017-05-11",
    "EndDate":"2019-12-31",
    "Interval": "Monthly",
    "Amount":10000,
    "Country":"BRA",
    "CreateDate": "2017-05-11T00:00:00",
    "Currency":"BRL",
    "CurrentRecurrencyTry": 1,
    "OrderNumber": "2017051120",
    "Provider":"Simulado",
    "RecurrencyDay": 11,
    "SuccessfulRecurrences": 1,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.braspag.com.br/v2/RecurrentPayment/f5a83c14-0254-4e73-bdd3-9afba1007266"
      }
    ],
    "RecurrentTransactions": [
      {
        "PaymentNumber": 0,
        "RecurrentPaymentId": "f5a83c14-0254-4e73-bdd3-9afba1007266",
        "TransactionId": "cd694ffb-c0c4-47db-9390-737df70a2012",
        "TryNumber": 1,
        "Links": [
          {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.braspag.com.br/v2/sales/cd694ffb-c0c4-47db-9390-737df70a2012"
          }
        ]
      }
    ],
    "Status": 1
  }
}
```

|Property|Description|Type|Size|Format|
|-----------|---------|----|-------|-------|
|`RecurrentPaymentId`|Field Identifier of the next recurrence.|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`NextRecurrency`|Date of next recurrence.|Text|7|05/2019 (MM/YYYY)|
|`StartDate`|Date of start of recurrence.|Text|7|05/2019 (MM/YYYY)|
|`EndDate`|Date of end of recurrence.|Text|7|05/2019 (MM/YYYY)|
|`Interval`|Interval between recurrences.|Text|10|<ul><li>Monthly</li><li>Bimonthly</li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul>|
|`CurrentRecurrencyTry`|Indicates the current recurrence retry number|Number|1|1|
|`OrderNumber`|Store Order ID|Text|50|2017051101|
|`Status`|Recurring Order Status|Number|1<UL><LI>|</LI><LI> 1 -Active2 -Finished3,4,5</LI><LI> - Inactive</LI></UL>|
|`RecurrencyDay`|The day of recurrence|Number|2|22|
|`SuccessfulRecurrences`|Quantity of successful recurrence made|Number|2|5|
|`RecurrentTransactions.RecurrentPaymentId`|Recurrence Id|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`RecurrentTransactions.TransactionId`|Payment Transaction ID generated on recurrence|GUID|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`RecurrentTransactions.PaymentNumber`|Recurrence Number. The first is zero|Number|2|3|
|`RecurrentTransactions.TryNumber`|Number of current attempt at specific recurrence|Number|2|1|

# Notification Post

To receive notification of change of status you must have configured the registration of your store in Braspag, the URL Status Payment field to receive the parameters as the example below.

Expected Store Response: HTTP Status Code 200 OK

If the HTTP Status Code 200 OK is not returned, it will be retried twice to send the Notification Post.

```json
{
   "RecurrentPaymentId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
   "PaymentId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
   "ChangeType": "2"
}
```

|Property|Description|Type|Size|Required|
|-----------|---------|----|-------|-----------|
|`RecurrentPaymentId`|Identifier representing the Recurring order (only applicable for ChangeType 2 or 4|GUID|36|No|
|`PaymentId`|Identifier representing the transaction|GUID|36|Yes|
|`ChangeType`|Specifies the notification type. See table below|Number|1|Yes|

|ChangeType|Description|
|----------|---------|
|1|Payment Status Change|
|2|Recurrence Created|
|3|Antifraud Status Change|
|4|Recurring payment status change (E.g.: automatic deactivation)|
|5|Refund denied (applicable to Rede)|
|6|Underpaid Registered Boleto|
|7|Chargeback Notification  <br/> For More Details [Risk Notification](https://braspag.github.io//manual/risknotification)|

# ANNEXES

## List of Providers

### Providers for Credit Card

|Provider|Brand|Description|
|--------|-----|---------|
|Simulated|---|Sandbox Provider|
|Cielo30|Visa, Master, Amex, Elo, Aura, Jcb, Diners, Discover, Hipercard, Hiper|Provider for transactions on the Cielo 3.0 e-commerce platform|
|Rede2|Visa, Master, Hipercard, Hyper, Diners, Link, Amex|Provider for transactions in e-commerce platform Rede (e-Rede) in REST version|
|Getnet|Visa, Master, Elo, Amex|Provider for transactions on Getnet e-commerce platform|
|GlobalPayments|Visa, Master, Elo, Hiper, Hipercard, Cabal, Amex|Provider for transactions on Global Payments e-commerce platform|
|Stone|Visa, Master, Hipercard, Elo|Provider for transactions on e-commerce platform Stone|
|Safra|Visa, Master, Hipercard, Elo|Provider for transactions on e-commerce platform Safra|
|Safra2|Visa, Master, Hipercard, Elo|Provider for transactions on new e-commerce platform Safra|
|FirstData|Visa, Master, Elo, Hipercard, Cabal, Amex|Provider for Guarani (PYG), Argentine Pesos (ARG) and Real (BRL) transactions on the First Data e-commerce platform|
|Sub1|Visa, Master, Diners, Amex, Discover, Cabal, Orange and Nevada|Provider for Argentine Peso (ARG) transactions on the Sub1 First Data legacy platform|
|Banorte|Visa, Master, Carnet|Provider for Mexican Peso (MXN) transactions on Banorte e-commerce platform|
|Credibanco|Visa, Master, Diners, Amex, Credential|Provider for Colombian Peso (COP) transactions on Credibanco e-commerce platform|
|Transbank2|Visa, Master, Diners, Amex|Provider for Chilean pesos (CLP) transactions on Transbank e-commerce platform|
|DMCard|---|---|

### Providers for Debit Card

|Provider|Brand|Description|
|--------|-----|---------|
|Cielo|Visa, Master|Provider for debit transactions on legacy platform Cielo 1.5|
|Cielo30|Visa, Master|Provider for debit transactions on e-commerce platform Cielo 3.0|
|Rede2|Visa, Master|Provider for transactions on Rede e-commerce platform|
|Getnet|Visa, Master|Provider for transactions on Getnet e-commerce platform|
|FirstData|Visa, Master|Provider for debit transactions on the First Data e-commerce platform|
|GlobalPayments|Visa, Master|Provider for transactions on Global Payments e-commerce platform|

### Providers for Voucher

|Provider|Brand|Description|
|--------|-----|---------|
|Alelo|Elo|Provider for voucher (meal and food voucher) transactions on the Alelo platform|

### Providers for Zero Auth via VerifyCard

|Provider|
|--------|
|Simulado|
|Cielo30 (Cielo 3.0)|
|Rede2 (REST e-Rede)|
|Getnet|
|FirstData|
|GlobalPayments|

### Providers for BIN Query via VerifyCard

|Provider|
|--------|
|Simulado|
|Cielo30 (Cielo 3.0)|

### Providers for Registered Boleto

|Provider|
|--------|
|Braspag, Bradesco2, BancoDoBrasil2, ItauShopline, Itau2, Santander2, Caixa2, CitiBank2, BankOfAmerica|

### Providers for Electronic Transfer (Online Debit)

|Provider|
|--------|
|Bradesco, BancoDoBrasil, SafetyPay, Itau, PayMeeRedirectCheckout, PayMeeSemiTransparent|

## Transaction Status List

Status returned by API

|Code|Payment Status|Payment Means|Description|
|------|-------------------|-----------------|---------|
|0|NotFinished|All|Failed to process payment|
|1|Authorized|All|Payment method to be captured or paid (Boleto)|
|2|PaymentConfirmed|All|Payment confirmed and finalized|
|3|Denied|Credit & Debit Card (Wire Transfer)|
|10|Voided|All|Payment canceled|
|11|Refunded|Credit & Debit Card|Payment Canceled/Refunded|
|12|Pending|Credit & Debit Card (Wire Transfer)|Awaiting Return from Financial Institution|
|13|Aborted|All|Payment canceled due to processing failure|
|20|Scheduled|Credit Card|Scheduled Recurrence|

## Anti-Fraud Status List

|Code|Description|
|--------|------------|
|0|Unknown|
|1|Accept|
|2|Reject|
|3|Review|
|4|Aborted|
|5|Unfinished|

## MDD Table

> Level of Relevance  <br/> 1 - Relevant  <br/> 2 - Very Relevant  <br/> 3 - Extremely Relevant <br/><br/>
> Depending on the level of relevance of the fields and the possibility of designing the risk strategy according to the needs of your business, the validation of test transactions will be charged if not sent. With this, we request a prior analysis of the documentation and signaling of fields that will not be sent.<br/><br/>
> If you do not have the data to send, please kindly do not send the corresponding field as empty, ie just do not send.

|ID|Value|Type|Relevance Level|Segment|
|--|-----|----|-------------------|--------|
|1|Customer Logged In  <br/> If the final customer logged in to the site to buy, submit: their login  <br/> If they made a purchase as a visitor, submit: Guest  <br/> If the sale was made directly by a third party, an agent for example did not submit the field|string|2|All|
|2|Quantity in days the customer is your customer  <br/> E.g.: 314|int|3|All|
|3|Quantity of order's installments|int|3|All|
|4|Sales Channel   <br/> Possible values:   <br/> Call Center -> Web phone purchase   <br/> -> Web purchase   <br/> Portal -> one agent making the purchase for the customer   <br/> Kiosk -> Kiosk  purchases  <br/> -> Mobile or tablet purchases|string|3|All|
|5|Send coupon/discount code if customer uses purchase|string|1|All|
|6|Quantity in days since last customer purchase  <br/> E.g.: 55|int|3|All|
|7|Seller's code or name (seller)|string|1|All|
|8|Attempts by the customer to make the payment for the same order, which may be with different credit cards and/or other means of payment|int|2|All|
|9|Identifies if customer will pick up product in store  <br/> Possible values: YES or NO|string|3|Retail or Cosmetics|
|10|Identifies whether payment will be made by someone not present on the trip or package  <br/> Possible values: YES or NO|string|3|Air or Tourism|
|11|Hotel category (how many stars)  <br/> Possible values:  <br/> 1 -> Simple 2  -> <br/> Budget 3  -> <br> Tourism 4  -> <br/> Superior 5  -> <br/> Luxury|int|3|Tourism|
|12|Amount in days from purchase date to hotel check-in  <br/> E.g.: 123|int|3|Tourism|
|13|Amount of hotel nights  <br/> E.g.: 5|int|3|Tourism|
|14|Category of trip or package  <br> Possible values: National or International or National/International|string|3|Air or Tourism|
|15|Name of airline/car rental/hotel  <br/> Name each company name, separated by/|string|2|Air or Tourism|
|16|Reservation PNR Code  <br/> When there is a reservation change for this PNR, in advance of the flight date, it is important to do a new fraud analysis by resubmitting this PNR|string|3|Air|
|17|Identifies if the reservation was anticipated  <br/> Possible values: YES or NO  <br/> If yes, it is also essential to send field 16 - Reservation PNR code|string|3|Air|
|18|Rental vehicle category  <br/> Possible values:  <br/> 1 - Basic 2  <br/> - Sport  <br/> 3 - Prime  <br/> 4 - Utility  <br/> 5 - Armored|string|3|Tourism|
|19|Identifies if the package refers to cruise  <br/> Possible values: YES or NO|string|2|Tourism|
|20|Fraud review decision for last purchase  <br/> Possible values: ACCEPT or REJECTED|string|3|All|
|21|Shipping Value  <br/> Eg: 10599 = $ 105.99|long|1|Retail or Cosmetics|
|22|Code of store where product will be taken  <br/> This field should be sent when field 9 is sent equal to YES|string|3|Retail or Cosmetics|
|23|Credit card suffix (last 4 digits)|int|1|All|
|24|Number of days since first customer purchase  <br/> E.g.: 150|int|3|All|
|25|Customer gender  <br/> Possible values:  <br/> F -> Female  <br/> M -> Male|string|2|All|
|26|Credit card bin (first 6 digits)|int|1|All|
|27|Delivery address street type  <br/> Possible values:  <br/> R -> Residential  C -> <br/> Commercial|string|2|All|
|28|Average time taken by the customer to make the purchase|int|2|All|
|29|Number of retries the customer made to log in|int|2|All|
|30|Number of web pages the customer has previously visited with a purchase within 30 minutes|int|2|All|
|31|Number of credit card number exchanges the customer has made to make the order payment|int|2|All|
|32|Identifies whether the email was pasted or typed  <br/> Possible values: Typed or Pasted|string|3|All|
|33|Identifies whether the credit card number has been pasted or entered  <br/> . Possible values: Typed or Pasted|string|3|All|
|34|Identifies if the email has been verified for account activation  <br/> Possible values: YES or NO|string|2|All|
|35|Identifies the type of customer  <br/> Possible values: Local or Tourist|string|2|Tourism|
|36|Identifies whether a gift card was used as a payment method  <br/> Possible values: YES or NO|string|1|All|
|37|Order delivery method <br/> Possible values: Sedex <br/> Sedex 10 <br/> 1 day<br/> 2 days <br/> Motoboy <br/> Same day <br/>|string|3|Retail or Cosmetics|
|38|Customer phone number identified via Caller ID when sale made through sales channel equal to Call Center  <br/> Format: DDIDDNumber - E.g.: 552121114720|string|3|All|
|39|Call Center Username  <br/> This field should be sent when field 4 is sent equal to Call Center|string|1|All|
|40|Comments entered when order is present|string|1|All|
|41|Document type  <br/> Possible values: CPF or CNPJ or Passport|string|2|All|
|42|Age of client|int|2|Everyone|
|43|Customer income range  <br/> E.g.: 100000 = $ 1,000.00|long|2|All|
|44|Historical quantity of customer purchases|int|3|All|
|45|Identifies if it is a purchase by employee  <br/> Possible values: YES or NO|string|2|All|
|46|Credit card name (bearer)|string|3|All|
|47|Identifies whether the card is private label  <br/> Possible values: YES or NO|string|2|All|
|48|Amount of payment methods used to make the purchase|int|2|All|
|49|Average purchases made over the past 6 months  <br/> E.g.: 159050 = $ 1,590.99|long|3|All|
|50|Current purchase value deviation factor over last 6 months average|3|All|
|51|Identifies if you are a VIP client with differentiated risk treatment or positive list  <br/> Possible values: YES or NO|string|3|All|
|52|Product Category  <br/> Possible Values:  <br/> Animals & Pets  <br/> Clothing & Accessories  <br/> Business & Industry  <br/> Cameras & Optics  <br/> Electronics  <br/> Food, Beverage & Cigarette  <br/> Furniture  <br/> Tools  <br/> Health & Beauty  <br/> Home & Garden  <br/> Bags & Luggage  <br/> Adult  <br/> Guns & Ammo  <br/> Office Supplies  <br/> Religion & Ceremonials  <br/> Software  <br/> Sports Equipment  <br/> Toys & Games  <br/> Vehicles & Parts  <br/> Books  <br/> DVDs & Videos  <br/> Magazines & Newspapers  <br/> Music  <br/> Other Categories Unspecified|string|2|All|
|53|Identifies if there is SMS phone confirmation routine  <br/> Possible values: YES or NO|string|2|All|
|54|What is the 2nd payment method|string|2|All|
|55|What is the 3rd payment method|string|2|All|
|56|If 2nd payment method is credit card, send flag|string|1|All|
|57|If 3rd payment method is credit card, send flag|string|1|All|
|58|If 2nd payment method, enter amount paid  <br/> E.g.: 128599 = $ 1,285.99|long|2|All|
|59|If 3rd payment method, inform the amount paid  <br/> E.g.: 59089 = R $ 590,89|long|2|All|
|60|Quantity in days since last change  <br/> E.g.: 57|int|3|All|
|61|Identifies if there was any cadastral change|string|1|
|62|Number of points redeemed on last purchase|long|3|Loyalty|
|63|Amount of points left in balance|long|2|Loyalty|
|64|Number of days since last point exchange|long|2|Loyalty|
|65|Customer identifier in loyalty program|string|2|Loyalty|
|66|Number of minutes recharged over last 30 days|long|2|Digital Goods|
|67|Number of top-ups to last 30 days|long|2|Digital Goods|
|68|Quantity in days between departure date and return date|int|2|Air|
|69|Number of passengers traveling regardless of age group|int|2|Air|
|70|Flight identifier|string|1|Air|
|71|Number of infants traveling|int|2|Air|
|72|Number of children traveling|int|2|Air|
|73|Number of adults traveling|int|2|Air|
|74|Identifies if you are a Frequent Flyer  <br/> Possible values: YES or NO|string|2|Air|
|75|Frequently Flyer Number|string|2|Air|
|76|Frequently Flyer Category  <br/> This category may vary by airline|int|2|Air|
77|Boarding day <br/> Possible values: Sunday  <br/> Monday  <br/> Tuesday ( <br/> Wednesday <br/> Thursday  <br/> Friday<br/> Saturday|string|2|Air|
|78|Airline Code  <br/> E.g.: JJ or LA or AA or UA or G3 and etc.|string|1|Air|
|79|Ticket fare class  <br/> E.g.: W or Y or N and etc|string|2|Air|
|80|Passenger Mobile Number <br/> E.g.: Format: DDIDDNumber - E.g.: 5521976781114|string|2|Air|
|81|Identifies whether the credit card holder will travel  <br/> Possible values: YES or NO|string|3|Air|
|82|Identifies whether the seller will work with manual review or not  <br/> Possible values: YES or NO|string|1|All|
|83|Business Segment  <br/> E.g.: Retail|string|2|All|
|84|Platform Name Integrated with the Gateway Braspag Antifraud API  <br/> If this is a direct integration between the store and Braspag, send value equal to PROPRIA|string|3|All|
|85 to 89|Free and defined fields with the anti-fraud provider, according to business rules|-|-|-|
|90 to 100|Reserved|-|-|-|

## List of HTTP Status Code

|HTTP Status Code|Description|
|------------------|-----------------------|
|200|OK|
|400|Bad Request|
|404|Resource Not Found|
|500|Internal Server Error|

## Recurrence Status List

|Code|Description|
|--------|---------------------------|
|1|Active|
|2|Finished|
|3|DisabledByMerchant|
|4|DisabledMaxAttempts|
|5|DisabledExpiredCreditCard|

## ReasonCode/ReasonMessage List

|Reason Code|Reason Message|
|-------------|------------------------------|
|0|Successful|
|1|AffiliationNotFound|
|2|IssuficientFunds|
|3|CouldNotGetCreditCard|
|4|ConnectionWithAcquirerFailed|
|5|InvalidTransactionType|
|6|InvalidPaymentPlan|
|7|Denied|
|8|Scheduled|
|9|Waiting|
|10|Authenticated|
|11|NotAuthenticated|
|12|ProblemsWithCreditCard|
|13|CardCanceled|
|14|BlockedCreditCard|
|15|CardExpired|
|16|AbortedByFraud|
|17|CouldNotAntifraud|
|18|TryAgain|
|19|InvalidAmount|
|20|ProblemsWithIssuer|
|21|InvalidCardNumber|
|22|TimeOut|
|23|CardProtectedIsNotEnabled|
|24|PaymentMethodIsNotEnabled|
|98|InvalidRequest|
|99|InternalError|

## API Error Codes

Codes returned on error, identifying the reason for the error and its respective messages.

|Code|Message|Description|
|------|--------|---------|
|0|Internal error|Data sent exceeds field size|
|100|RequestId is required|Submitted field is empty or invalid|
|101|MerchantId is required|Submitted field is empty or invalid|
|102|Payment Type is required|Submitted field is empty or invalid|
|103|Payment Type can only contain letters|Special characters not allowed|
|104|Customer Identity is required|Submitted field is empty or invalid|
|105|Customer Name is required|Submitted field is empty or invalid|
|106|Transaction ID is required|Submitted field is empty or invalid|
|107|OrderId is invalid or does not exist|Submitted field exceeds size or contains special characters|
|108|Amount must be greater or equal to zero|Transaction value must be greater than "0"|
|109|Payment Type is required|Submitted field is empty or invalid|
|110|Invalid Payment Currency|Submitted field is empty or invalid|
|111|Payment Country is required|Submitted field is empty or invalid|
|112|Invalid Payment Country|Submitted field is empty or invalid|
|113|Invalid Payment Currency|Submitted field is empty or invalid|
|114|The provided MerchantId is not in correct format|The submitted MerchantId is not a GUID|
|115|The provided MerchantId was not found|MerchantID does not exist or belongs to another environment (EX: Sandbox)|
|116|The provided MerchantId is blocked|Shop locked, contact support Braspag|
|117|Credit Card Holder is required|Submitted field is empty or invalid|
|118|Credit Card Number is required|Submitted field is empty or invalid|
|119|At least one Payment is required|"Payment" node not sent|
|120|Request IP not allowed. Check your IP White List|IP blocked for security reasons|
|121|Customer is required|"Customer" node not sent|
|122|MerchantOrderId is required|Submitted field is empty or invalid|
|123|Installations must be greater or equal to one|Number of installments must be greater than 1|
|124|Credit Card Number is required|Submitted field is empty or invalid|
|125|Credit Card Expiration Date is required|Submitted field is empty or invalid|
|126|Credit Card Expiration Date is invalid|Submitted field is empty or invalid|
|127|You must provide CreditCard Number|Credit Card Number is required|
|128|Card Number length exceeded|Card number over 16 digits|
|129|Affiliation not found|Non-store payment method or invalid Provider|
|130|Could not get Credit Card|---|
|131|MerchantKey is required|Submitted field is empty or invalid|
|132|MerchantKey is invalid|The submitted Merchantkey is not a valid|
|133|Provider is not supported for this Payment Type|Provider submitted does not exist|
|134|FingerPrint length exceeded|Data sent exceeds field size|
|135|MerchantDefinedFieldValue length exceeded|Submitted data exceeds field size|
|136|ItemDataName length exceeded|Submitted data exceeds field size|
|137|ItemDataSKU length exceeded|Submitted data exceeds field size|
|138|PassengerDataName length exceeded|Data sent exceeds field size|
|139|PassengerDataStatus length exceeded|Data sent exceeds field size|
|140|PassengerDataEmail length exceeded|Data sent exceeds field size|
|141|PassengerDataPhone length exceeded|Data sent exceeds field size|
|142|TravelDataRoute length exceeded|Submitted data exceeds field size|
|143|TravelDataJourneyType length exceeded|Submitted data exceeds field size|
|144|TravelLegDataDestination length exceeded|Submitted data exceeds field size|
|145|TravelLegDataOrigin length exceeded|Submitted data exceeds field size|
|146|SecurityCode length exceeded|Data sent exceeds field size|
|147|Address Street length exceeded|Data sent exceeds field size|
|148|Address Number length exceeded|Data sent exceeds field size|
|149|Address Complement length exceeded|Data sent exceeds field size|
|150|Address ZipCode length exceeded|Data sent exceeds field size|
|151|Address City length exceeded|Data sent exceeds field size|
|152|Address State length exceeded|Data sent exceeds field size|
|153|Address Country length exceeded|Data sent exceeds field size|
|154|Address District length exceeded|Data sent exceeds field size|
|155|Customer Name length exceeded|Data sent exceeds field size|
|156|Customer Identity length exceeded|Data sent exceeds field size|
|157|Customer IdentityType length exceeded|Submitted data exceeds field size|
|158|Customer Email length exceeded|Data sent exceeds field size|
|159|ExtraData Name length exceeded|Data sent exceeds field size|
|160|ExtraData Value length exceeded|Submitted data exceeds field size|
|161|Boleto Instructions length exceeded|Data sent exceeds field size|
|162|Boleto Demostrative length exceeded|Data sent exceeds field size|
|163|Return Url is required|Return URL is not valid - Paging or extensions (E.g.:PHP) in the return URL|
|166|AuthorizeNow is required|---|
|167|Antifraud not configured|Antifraud not linked to merchant registration|
|168|Recurrent Payment not found|Recurrence not found|
|169|Recurrent Payment is not active|Recurrence is not active. Paralyzed Execution|
|170|Protected Card not configured|Protected Card not linked to merchant registration|
|171|Affiliation data not sent|Order Processing Failed - Contact Braspag Support|
|172|Credential Code is required|Validation of submitted credentials failed|
|173|Payment method is not enabled|Payment method not linked to merchant registration|
|174|Credit Card Number is required|Submitted field is empty or invalid|
|175|EAN is required|Submitted field is empty or invalid|
|176|Payment Currency is not supported|Submitted field is empty or invalid|
|177|Card Number is invalid|Submitted field is empty or invalid|
|178|EAN is invalid|Submitted field is empty or invalid|
|179|The max number of installments allowed for recurring payment is 1|Submitted field is empty or invalid|
|180|The provided Card PaymentToken was not found|Cartão Protegido's token not found|
|181|The MerchantIdJustClick is not configured|Cartão Protegido's Token Locked|
|182|Brand is required|Card brand not sent|
|183|Invalid customer birthdate|Invalid or future date of birth|
|184|Request could not be empty|Failed to form this request. Check the code sent|
|185|Brand is not supported by selected provider|Flag not supported by API Braspag|
|186|The selected provider does not support the options provided (Capture, Authenticate, Recurrent or Installments)|Payment method does not support sent command|
|187|ExtraData Collection contains one or more duplicated names|---|
|188|Avs with CPF invalid|---|
|189|Avs with length of street exceeded|Submitted data exceeds field size|
|190|Avs with length of number exceeded|Data sent exceeds field size|
|190|Avs with length of complement exceeded|Data sent exceeds field size|
|191|Avs with length of district exceeded|Submitted data exceeds field size|
|192|Avs with zip code invalid|Zip code sent is invalid|
|193|Split Amount must be greater than zero|Value for SPLIT must be greater than 0|
|194|Split Establishment is Required|SPLIT not enabled for store registration|
|195|The PlataformId is required|Platform Not Validated|
|196|DeliveryAddress is required|Required field not submitted|
|197|Street is required|Required field not submitted|
|198|Number is required|Required field not submitted|
|199|ZipCode is required|Required field not submitted|
|200|City is required|Required field not submitted|
|201|State is required|Required field not submitted|
|202|District is required|Required field not submitted|
|203|Cart item Name is required|Required field not submitted|
|204|Cart item Quantity is required|Required field not submitted|
|205|Cart item type is required|Required field not submitted|
|206|Cart item name length exceeded|Data sent exceeds field size|
|207|Cart item description length exceeded|Data sent exceeds field size|
|208|Cart item sku length exceeded|Data sent exceeds field size|
|209|Shipping address sku length exceeded|Data sent exceeds field size|
|210|Shipping data cannot be null|Required field not submitted|
|211|WalletKey is invalid|Invalid Visa Checkout data|
|212|Merchant Wallet Configuration not found|Visa Checkout not linked to merchant account|
|213|Credit Card Number is invalid|Credit card sent is invalid|
|214|Credit Card Holder Must Have Only Letters|Cardholder must not contain special characters|
|215|Agency is required in Boleto Credential|Required Field Not Submitted|
|216|Customer IP address is invalid|IP blocked for security|
|300|MerchantId was not found|---|
|301|Request IP is not allowed|---|
|302|Sent MerchantOrderId is duplicated|---|
|303|Sent OrderId does not exist|---|
|304|Customer Identity is required|---|
|306|Merchant is blocked|---|
|307|Transaction not found|Transaction not found or missing in environment|
|308|Transaction not available to capture|Transaction cannot be captured - Contact Braspag Support|
|309|Transaction not available to void|Transaction cannot be canceled - Contact Support Braspag|
|310|Payment method do not support this operation|Command sent not supported by payment method|
|311|Refund is not enabled for this merchant|Cancellation after 24 hours not released to retailer|
|312|Transaction not available to refund|Transaction does not allow cancellation after 24 hours|
|313|Recurrent Payment not found|Recurring transaction not found or not available in the environment|
|314|Invalid Integration|---|
|315|Cannot change NextRecurrency with pending payment|---|
|316|Cannot set NextRecurrency to past date|Not allowed to change recurrence data for a past date|
|317|Invalid Recurrency Day|---|
|318|No transaction found|---|
|319|Smart recurrency is not enabled|Recurrence not linked to merchant registration|
|320|Cannot Update Affiliation Because this Recurrency not Affiliation saved|---|
|321|Cannot set EndDate to before next recurrency|---|
|322|Zero Dollar Auth is not enabled|Zero Dollar not linked to merchant registration|
|323|Bin Query is not enabled|Bins Query not linked to merchant registration|

## Test Cards (Simulado)

"Simulado" is a payment method that emulates the use of Credit Card payments. With this payment method it is possible to simulate all Authorization, Capture and Cancellation flows.

For better use of Simulado Payment Methods, we are providing test cards in the table below.

Transaction status will be as each card is used.

|Transaction Status|Testing Cards|Return Code|Return Message|
|-------------------|----------------------------------|-----------------|-------------------|
|Authorized|0000.0000.0000.0000/0000.0000.0000.0001/0000.0000.0000.0004|4|Operação realizada com sucesso|
|Not Authorized|0000.0000.0000.0002|05|Not Authorized|
|Not Authorized|0000.0000.0000.0003|57|Expired Card|
|Not Authorized|0000.0000.0000.0005|78|Bloqued Card|
|Not Authorized|0000.0000.0000.0006|99|Time Out|
|Not Authorized|0000.0000.0000.0007|77|Canceled Card|
|Not Authorized|0000.0000.0000.0008|70|Problems with the Credit Card|
|Random Authorization|0000.0000.0000.0009|4/99|Operation Successful/Time Out|

Security Code (CVV) and validity information can be random, keeping the format - CVV (3 digits) Validity (MM/YYYY).

## List of Values - Payment.FraudAnalysis.Cart.Items [n].GiftCategory

|Value|Description|
|:-|:-|
|Yes|In case of divergence between billing and delivery addresses, assigns low risk to order|
|No|In case of divergence between billing and delivery addresses, assigns high risk to order (default)|
|Off|Differences between billing and shipping addresses do not affect score|

## List of Values - Payment.FraudAnalysis.Cart.Items [n].HostHedge

|Value|Description|
|:-|:-|
|Low|Low|
|Normal|Normal (default)|
|High|High|
|Off|Will not affect fraud analysis score|

## List of Values - Payment.FraudAnalysis.Cart.Items [n].NonSensicalHedge

|Value|Description|
|:-|:-|
|Low|Low|
|Normal|Normal (default)|
|High|High|
|Off|Will not affect fraud analysis score|

## List of Values - Payment.FraudAnalysis.Cart.Items [n].ObscenitiesHedge

|Value|Description|
|:-|:-|
|Low|Low|
|Normal|Normal (default)|
|High|High|
|Off|Will not affect fraud analysis score|

## List of Values - Payment.FraudAnalysis.Cart.Items [n].TimeHedge

|Value|Description|
|:-|:-|
|Low|Low|
|Normal|Normal (default)|
|High|High|
|Off|Will not affect fraud analysis score|

## List of Values - Payment.FraudAnalysis.Cart.Items [n].PhoneHedge

|Value|Description|
|:-|:-|
|Low|Low|
|Normal|Normal (default)|
|High|High|
|Off|Will not affect fraud analysis score|

## List of Values - Payment.FraudAnalysis.Cart.Items [n].VelocityHedge

|Value|Description|
|:-|:-|
|Low|Low|
|Normal|Normal (default)|
|High|High|
|Off|Will not affect fraud analysis score|

## List of Values - Payment.FraudAnalysis.Cart.Items [n].Type

|Value|Description|
|:-|:-|
|AdultContent|Adult Content|
|Coupon|Coupon applied to any order|
|Default|Default value for the product type. When no other value is sent, the type is assumed to be this|
|EletronicGood|Electronic product other than software|
|EletronicSoftware|Downloaded electronically distributed software|
|GiftCertificate|Gift Certificate|
|HandlingOnly|Fee you charge your customer to cover your administrative selling costs. E.g.: Convenience Fee/Installation Fee|
|Service|Service to be performed for the customer|
|ShippingAndHandling|Shipping amount and fee you charge your customer to cover their administrative costs of selling|
|ShippingOnly|Shipping Value|
|Subscription|Subscription. E.g.: Video Streaming/News Subscription|

## Value List - Payment.FraudAnalysis.Shipping.Method

|Value|Description|
|:-|:-|
|SameDay|Same Day Delivery|
|OneDay|Next Day Delivery|
|TwoDay|Two-day delivery|
|ThreeDay|Three-day delivery|
|LowCost|Low Cost Delivery|
|Pickup|Pick up on the store|
|Other|Other delivery method|
|None|No delivery since it is a service or subscription|

## List of Values - Payment.FraudAnalysis.Travel.JourneyType

|Value|Description|
|:-|:-|
|OneWayTrip|One way only|
|RoundTrip|Round Trip|

## Value List - Payment.FraudAnalysis.Travel.Passengers [n].Rating

|Value|Description|
|:-|:-|
|Adult|Adult|
|Child|Child|
|Infant|Infant|

## Value List - Payment.FraudAnalysis.Travel.Passengers [n].Status

|Value|
|:-|
|Standard|
|Gold|
|Platinum|

## List of Values - Payment.FraudAnalysis.Status

|Value|Description|
|:-|:-|
|Accept|Transaction accepted after fraud analysis|
|Review|Transaction under review after fraud analysis|
|Reject|Transaction rejected after fraud analysis|
|Pendent|Transaction pending, because submitting the same for fraud analysis there was a timeout in the response between Braspag and Cybersource|
|Unfinished|Transaction not finalized for any contract validation reason or internal error|
|ProviderError|Transaction with provider error being submitted for analysis|

## Value List - Payment.FraudAnalysis.FraudAnalysisReasonCode

|Value|Description|
|:-|:-|
|100|Operation successfully performed|
|101|Transaction submitted for fraud analysis is missing one or more required fields  <br/> Check in response for `ProviderAnalysisResult.Missing` field  <br/> Possible action: Resubmit the transaction with full information|
|102|Transaction submitted for fraud analysis has one or more fields with invalid values  <br/> Check in response for `ProviderAnalysisResult.Invalid` field  <br/> Possible action: Resubmit the transaction with the correct information|
|150|Internal Error  <br/> Possible Action: Wait a few minutes and try resubmitting the transaction|
|151|The transaction was received, but timeout occurred on the server. This error does not include client-server timeout  <br/> Possible action: Please wait a few minutes and try resending the transaction|
|152|The request was received but timeout occurred Possible  <br/> action: Please wait a few minutes and try resending the transaction|
|202|Transaction declined because card has expired or expiration date does not match Correct  <br/> action: Request another card or other payment method|
|231|Transaction declined because card is invalid  <br/> Possible action: Request another card or other payment method|
|234|Cybersource Store Configuration Issue Possible  <br/> Action: Please contact support to fix the configuration issue|
|400|Fraud score exceeds threshold  <br/> Possible action: Review buyer's transaction|
|480|The transaction has been marked for review by the Decision Manager (DM)|
|481|The transaction was rejected by the Decision Manager (DM)|

## Value List - Payment.FraudAnalysis.ReplyData.AddressInfoCode

|Value|Description|
|:-|:-|
|COR-BA|Billing address can be standardized|
|COR-SA|Shipping address can be standardized|
|INTL-BA|Billing address country is outside the US|
|INTL-SA|Shipping Address Country is outside the US|
|MIL-USA Military Address in the USA|
|MM-A|Billing and delivery addresses use different street names|
|MM-BIN|Card BIN (first six digits of card number) does not match country|
|MM-C|Billing and shipping addresses use different cities|
|MM-CO|Billing and shipping addresses use different countries|
|MM-ST|Billing and shipping addresses use different states|
|MM-Z|Billing and delivery addresses use different postal codes|
|UNV-ADDR|Address is unverifiable|

## Value List - Payment.FraudAnalysis.ReplyData.FactorCode

|Value|Description|
|:-|:-|
|A|Excessive address change. Buyer has changed billing address two or more times in the last six months|
|B|BIN of the card or risk authorization. Risk factors relate to credit card BIN and/or card authorization checks|
|C|High numbers of credit cards. Buyer has used more than six credit card numbers in the last six months|
|D|Impact of email address. Buyer uses a free email provider or email address is risky|
|E|Positive list. Buyer is on your positive list|
|F|Negative list. The account number, address, email address or IP address for this purpose appears in your negative list|
|G|Geolocation inconsistencies. Shopper's email address or domain, phone number, billing address, shipping address, or IP address is suspicious|
|H|Excessive name changes. Shopper has changed billing address two or more times in the last six months|
|I|Internet inconsistencies. IP address and email domain are not consistent with billing address|
|N|Meaningless input. Shopper name and address fields contain meaningless words or language|
|O|Obscenities. Shopper Data Contains Obscene Words|
|P|Identity morphing. Multiple values of an identity element are linked to a value of a different identity element. For example, multiple phone numbers are linked to a single account number|
|Q|Phone inconsistencies. Shopper's phone number is suspicious|
|R|Risky Order. Transaction, shopper and merchant show high risk correlated information|
|T|Time Coverage. Shopper is attempting a late purchase|
|U|Unverifiable address. Billing or shipping address cannot be verified|
|V|Card has been used many times in the last 15 minutes|
|W|Marked as suspicious. The billing or shipping address is similar to an address previously marked as suspicious|
|Y|The address, city, state, or country of the billing and shipping addresses do not match|
|Z|Invalid value. Because the request contains an unexpected value, a default value has been overridden. Although the transaction can still be processed, carefully examine the request for anomalies|

## Value List - Payment.FraudAnalysis.ReplyData.InternetInfoCode

|Value|Description|
|:-|:-|
|FREE-EM|Shopper's email address is from a free email provider|
|INTL-IPCO|Shopper's email address country is outside the US|
|INV-EM|Shopper's email address is invalid|
|MM-EMBCO|Shopper's email address domain is not consistent with billing address country|
|MM-IPBC|Shopper's email address is not consistent with city of billing address|
|MM-IPBCO|Shopper's email address is not consistent with billing address country|
|MM-IPBST|The shopper's IP address is not consistent with the state in the billing address. However, this information code cannot be returned when the inconsistency is between immediately adjacent states|
|MM-IPEM|Shopper's email address is not consistent with IP address|
|RISK-EM|Shopper's email domain (for example, mail.example.com) is associated with high risk|
|UNV-NID|The shopper's IP address is from an anonymous proxy. These entities completely hide IP address information|
|UNV-RISK|IP address is from risk place|
|UNV-EMBCO|Email address country does not match billing address country|

## Value List - Payment.FraudAnalysis.ReplyData.IpRoutingMethod

|Value|Description|
|:-|:-|
|Anonymizer|IP addresses are hidden because the shopper is extremely cautious, either absolute privacy or fraudulent|
|AOL, AOL dialup, AOL POP and AOL proxy|AOL members. In most cases, the country can be identified, but the state and city cannot|
|Proxy cache|Proxy used through an Internet accelerator or service content distribution. Shopper may be located in a country other than IP address|
|Fixed|The IP address is near or in the same place as the shopper|
|International proxy|Proxy that contains traffic from various countries. The shopper may be located in a country other than that indicated by the IP address. In many cases, corporate networks are routing international office traffic through a central hub, often corporate headquarters|
|Mobile gateway|Gateway for connecting mobile devices to the internet. Many carriers, especially in Europe, serve more than one country and traffic occurs through centralized network hubs. Shopper may be located in a country other than IP address|
|POP|Shopper dialing at a regional ISP probably near IP address location, but possibly across geographic boundaries|
|Regional proxy|Proxy that contains multi-state traffic within a single country. The shopper may be located in a country other than that indicated by the IP address. In many cases, corporate networks are routing international office traffic through a central hub, often corporate headquarters|
|Satellite|Satellite connections. If uplink and downlink are registered, the routing method is considered standard because the sender is known. However, if the downlink is not registered, the shopper may be anywhere within the standard satellite beam, which may span one continent or more|
|SuperPOP|The buyer is dialing from a multi-state or multinational ISP that is unlikely to find the IP address location. Shopper may be dialing across geographic boundaries|
|No value returned|Routing type is unknown|

## Value List - Payment.Chargebacks [n].Status

|Value|Description|
|:-|:-|
|Received|Chargeback received from acquirer|
|AcceptedByMerchant|Chargeback accepted by store. In this case the store understands that it has indeed suffered a chargeback and will not contest it|
|ContestedByMerchant|Chargeback contested by store. In this case the store has sent the necessary documents to try to reverse the chargeback|
