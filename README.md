
# iveri-webservice
[![Clojars Project](https://img.shields.io/clojars/v/iveri-webservice-consumer.svg)]  [![Build Status](https://travis-ci.org/lemaiyan/iveri-webservice-consumer.svg?branch=master)]


## Installation

#### Leiningen
`[iveri-webservice-consumer "1.1.0"]`

#### Gradle
`compile "iveri-webservice-consumer:iveri-webservice-consumer:1.1.0"`

#### Maven
     <dependency>
       <groupId>iveri-webservice-consumer</groupId>
       <artifactId>iveri-webservice-consumer</artifactId>
       <version>1.1.0</version>
     </dependency>

## Usage
To Use the service you need to require it in your code using as shown below

`(require [iveri-webservice-consumer.core :as iveri])`

#### Intialize
Once added to your namespace and before you make any calls you need to initialize the service using the function

`(iveri/initialize params)`

The `params` above are values provided by iveri which are:

    {:certificateID "The certificate id provided by Iveri",
     :mode "The enviroment you're using this can be either Test or Live",
     :applicationID "The application id provided by Iveri"
     :server "The server you're connecting to i.e 'portal.host.iveri.com'"
     :webServicePath "The webservice path on the server you're calling i.e '/iVeriWebService/Service.asmx'"}
   
#### Authorize
After a successful initialization you need to authorize the card. This way you can catch any issues with the card if any and if successful you reserve the amount.
Use the following function to reserve

    (iveri/authorizeCard {:amount "Amount in cents" 
                          :card-expiry "the card exipiy in the formt mmyyyy i.e. 092016" 
                          :cardCVV  "The card security code"
                          :PAN "The card number" 
                          :currency "Three letter international currency code i.e. KES" 
                          :merchant-ref "A unique identifier of the transaction"})
                          
When the authorization is successful you will get a response that looks like the one below

    {:message "Success", :success 1, :error nil, :data {:RequestID "{F88B6972-2596-4F30-8E19-476BC19785A0}", :MerchantTrace "d67ff425-aa44-4bfe-a863-5c249f7d806a", :ApplicationID "{XXXXXX-XXX-XXXX-XXXX-XXXXXXXXXX}", :Mode "Test", :Result {:Gateway "HOST", :DBServer "PRODGW2012DB2", :AppServer "PRODGW2012APP1", :Source "IMNaradaTestProvider", :Description "Duplicate MerchantReference", :Code "255", :Status "-1"}, :Currency "KES", :Command "Authorisation", :CCNumber "4242........4242", :Amount "1000", :MerchantReference "MoDe0005", :PAN "4242........4242"}} 

#### Debit

This means we have already reserved the amount so we now need to debit the card to actualize the payment and this is done using

    (iveri/debitCard {:amount "Amount in cents" 
                      :card-expiry "the card exipiy in the formt mmyyyy i.e. 092016" 
                      :cardCVV  "The card security code"
                      :PAN "The card number" 
                      :currency "Three letter international currency code i.e. KES" 
                      :auth-code "This is the authorization code you get back after a successful authorization in our case that is 777JLW"})
                  
                  
If the debit is successful you will get back a response that looks the one below

    {:message "Success", :success 1, :error nil, :data {:AcquirerReference "20160607:000000000000", :RequestID "{5A42FC45-C03F-4673-B208-5D48DEDA82CA}", :MerchantUSN "7771777", :ExpiryDate "092016", :AcquirerTime "092957", :MerchantName "iVeri Payment Technology", :MerchantTrace "8093fb5c-12b7-413e-bbc0-b6d27b5dd549", :AuthorisationCode "777JLW", :Issuer "Unknown Issuer", :ApplicationID "{XXXXXX-XXX-XXXX-XXXX-XXXXXXXXXX}", :AcquirerDate "20160606", :Mode "Test", :Terminal "Default", :MerchantCity "Sandton", :Result {:Gateway "HOST", :DBServer "PRODGW2012DB2", :AppServer "PRODGW2012APP1", :Status "0"}, :Currency "USD", :PANMode "Keyed", :MerchantCountry "South Africa", :Command "Debit", :MerchantCountryCode "ZA", :CCNumber "4242........4242", :Acquirer "IMNarada", :ReconReference "001300", :Amount "1000", :MerchantReference "1465198226262", :Jurisdiction "International", :BIN "4", :MerchantAddress "MERCHANT ADDRESS", :Association "VISA", :CardType "Unknown Card Type", :DisplayAmount "$ 10.00", :TransactionIndex "{77F45F17-577C-40CA-8518-15C90157874C}", :PAN "4242........4242", :DistributorName "IM Bank", :CardHolderPresence "CardNotPresent"}
 
 
#### Void
 
For one reason or another you may want to void a transaction, to do that use the function

    (iveri/voidCard {:merchant-trace "This is the metchant trace you get after a successful debit in our case the vealue is '8093fb5c-12b7-413e-bbc0-b6d27b5dd549'})      

When successful the response is as below 

    {:message "Success", :success -1, :error nil, :data {:RequestID "{302E5335-E574-494B-9E3E-93DC1F80D0C7}", :Mode "Test", :Command "Void", :ApplicationID "{XXXXXX-XXX-XXXX-XXXX-XXXXXXXXXX}", :OriginalRequestID "{E7E672D6-871C-42A6-8A76-F4AD922B0C0F}", :OriginalMerchantTrace "34cd44d9-e6cf-4275-a083-b13fd146b785", :Result {:Gateway "HOST", :DBServer "PRODGW2012DB2", :AppServer "PRODGW2012APP1", :Status "0"}}}




## License

Copyright © Jijo Lemaiyan, 2016 All rights reserved.

The use and distribution terms for this software are covered by the Eclipse Public License 1.0 (http://opensource.org/licenses/eclipse-1.0.php) which can be found in the file epl-v10.html at the root of this distribution. By using this software in any fashion, you are agreeing to be bound by the terms of this license. You must not remove this notice, or any other, from this software.
