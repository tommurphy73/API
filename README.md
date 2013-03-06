##1.0 API Introduction
======================

</br>

### 1.1 Response Codes

<table>
 <tr><th width=308 align=left>   
             Code </th><th width=308 align=left> 
                               Explanation         </th></tr>
                               
 <tr><td>    200  </td><td>    OK                  </td></tr>
 <tr><td>    400  </td><td>    Bad Request         </td></tr>
 <tr><td>    401  </td><td>    Not Authorised      </td></tr>
 <tr><td>    403  </td><td>    Forbidden           </td></tr>
 <tr><td>    405  </td><td>    Method Not Allowed  </td></tr>
 <tr><td>    406  </td><td>    Not Acceptable      </td></tr>
 <tr><td>    408  </td><td>    Timeout             </td></tr>
</table>


### 1.2 API Formats

<table>
 <tr><th width=308 align=left> 
             Type               </th><th width=308 align=left>   
                                              Format                      </th><th width=308 align=left> 
                                                                                            Notes                 </th></tr>
 <tr><td>    DateTime           </td><td>     YYYY-MM-DD hh:mm:ss         </td><td>         Must always be UTC    </td></tr>
 <tr><td>    DateTime -> String </td><td>     YYYYMMDDhhmmss              </td><td>         Must alwayys be UTC   </td></tr>
 <tr><td>    SocialNetworkType  </td><td>     Facebook, Twitter, LinkedIn </td><td>         Enumerator            </td></tr>
 <tr><td>    AddressType        </td><td>     Billing, Delivery           </td><td>         Enumerator            </td></tr>
 <tr><td>    StatusType         </td><td>     Init, Placed, Refunded, 
                                              Rejected, Fulfilled, 
                                              Completed, Refreshed,
                                              Payment                     </td><td>         Enumerator            </td></tr>
 <tr><td>    ReasonType         </td><td>     System, Fraud, Complaint,
                                              Remorse, Other              </td><td>         Enumerator            </td></tr>
</table>

##2.0 Authentication API
========================

The Trustev API is secured using Token Authentication. Where an API method requires authentication, a valid token must be provided.


Details of how to retrieve a token are below. Your Username, Password and Application Secret are all available on the Trustev Developer Portal.

### 2.1 Getting a Token

#### 2.1.1 Format

<table>                        
 <tr><td width=308>    URL                      </td><td>    https://api.trustev.com/v1/Authentication/GetToken </td></tr>
 <tr><td>              Authentication Required  </td><td>    No                                                 </td></tr>
 <tr><td>              Format                   </td><td>    JSON                                               </td></tr>
 <tr><td>              Method                   </td><td>    POST                                               </td></tr>
</table>

#### 2.1.2 Request 

      {
      “request”: 
      {
        “Username” : String,
        “Password” : String,
        “Sha256Hash” : String,
        “Timestamp” : DateTime 
      }
      }

#### 2.1.3 Response

      {
      “Message” : String,
      “Code” : Int,
      “Token” : 
      {
        “Token” : String,
        “ExpireAt” : DateTime
      }
      }
#### 2.1.4 Creating the Password Hash

The generation of the hashed value required for the Password parameter of the request object is a 2 stage process. This process is as follows:

##### <i>Stage 1</i>

Create a Sha256Hash of a string in the format of {0}.{1} where {0} is the timestamp being sent in the request (transformed to a string in the specified format), and {1} your password.

##### <i>Stage 2</i>

Create a Sha256Hash of a string in the format of {0}.{1}, where {0} is the result of Stage 1, and {1} is your Application Secret.

#### 2.1.5 Creating the Request Hash

The generation of the hashed value required for the Sha256Hash parameter of the request object is a 2 stage process. This process is as follows:

##### <i>Stage 1</i>

Create a Sha256Hash of a string in the format of {0}.{1} where {0} is the timestamp being sent in the request (transformed to a string in the specified format),, and {1} your username.


##### <i>Stage 2</i>

Create a Sha256Hash of a string in the format of {0}.{1}, where {0} is the result of Stage 1, and {1} is your Application Secret.

### 2.2 Using a Token

Each Token generated is valid for 1 hour from the time of creation. The expiration time of the token is specified in the Token object returned with a succesful GetToken call.

When calling a method or service that requires authenticaiton, you must include both a valid token, and your username.


## 3.0 Social API
=================

### 3.1 Add Profile

This API method allows you to share authentication and access information relating to one or more social network accounts. The API will create a new Trustev account for every time is invoked, unless an account already exists for any of the specified social network accounts.

#### 3.1.1 Format


<table>                        
 <tr><td width=308>    URL                      </td><td>    https://api.trustev.com/v1/Social/AddProfile       </td></tr>
 <tr><td>              Authentication Required  </td><td>    Yes                                                </td></tr>
 <tr><td>              Format                   </td><td>    JSON                                               </td></tr>
 <tr><td>              Method                   </td><td>    POST                                               </td></tr>
</table>


#### 3.1.2 Request 

       {
       “request” :
       {
         “SocialNetworks”:
         [
         “SocialNetwork”:
         {
           “Type” : SocialNetworkType,
           “Id” : String,
           “ShortTermAccessToken” : String,
           “LongTermAccessToken” : String,
           “ShortTermAccessTokenExpiry” : DateTime,
           “LongTermAccessTokenExpiry” : DateTime,
           “Secret” : String
         }
         ]
       }
       }

#### 3.1.3 Response

      {
      “Message” : string,
      “Code” : int
      }
      
### 3.2 Update Profile

This API method allows you to update an existing Trustev account with additional social networking accounts. This method requires that a Trustev account exists at least one of the specified social network accounts already attached

#### 3.2.1 Format


<table>                        
 <tr><td width=308>    URL                      </td><td>    https://api.trustev.com/v1/Social/UpdateProfile    </td></tr>
 <tr><td>              Authentication Required  </td><td>    Yes                                                </td></tr>
 <tr><td>              Format                   </td><td>    JSON                                               </td></tr>
 <tr><td>              Method                   </td><td>    POST                                               </td></tr>
</table>

#### 3.2.2 Request 

     {
     “request” :
     {
       “Type” : SocialNetworkType,
       “Id” : String,
       “SocialNetworks”:
       [
       “SocialNetwork”:
       {
         “Type” : SocialNetworkType,
         “Id” : String,
         “ShortTermAccessToken” : String,
         “LongTermAccessToken” : String,
         “ShortTermAccessTokenExpiry” : DateTime,
         “LongTermAccessTokenExpiry” : DateTime,
         “Secret” : String
       }
       ]
     }
     }
     
#### 3.2.3 Response

     {
     “Message” : string,
     “Code” : int
     }

### 3.3 Delete Profile

This API method allows you delete one or many social profile accounts from a Trustev account. This will delete the social network authentication data that has been shared with Trustev, and remove any data relevant to the social profile from Trustev’s systems.

#### 3.3.1 Format

<table>                        
 <tr><td width=308>    URL                      </td><td>    https://api.trustev.com/v1/Social/DeleteProfile    </td></tr>
 <tr><td>              Authentication Required  </td><td>    Yes                                                </td></tr>
 <tr><td>              Format                   </td><td>    JSON                                               </td></tr>
 <tr><td>              Method                   </td><td>    POST                                               </td></tr>
</table>


#### 3.3.2 Request 

      {
      “request” :
      {
        “SocialNetworks”:
        [
        “SocialNetwork”:
        {
          “Type” : SocialNetworkType,
          “Id” : String 
        }
        ]
      }
      }

#### 3.3.3 Response

     {
     “Message” : string,
     “Code” : int
     }
     
     
## 4.0 Transaction API
===================

### 4.1 Add Transaction

This API method allows you to create a transaction. The transaction can be created at any point during the checkout process, and will return a Trustev Profile as a return type. The request object for this API method must contain a Trustev Session Id, which is generated and expossed by Trustev.js. Details of how to obtain this SessionId are details in the Trustev.js Integration Document, or on trustev.com.

#### 4.1.1 Format

<table>                        
 <tr><td width=308>    URL                      </td><td>    https://api.trustev.com/v1/Transaction/AddTransaction     </td></tr>
 <tr><td>              Authentication Required  </td><td>    Yes                                                       </td></tr>
 <tr><td>              Format                   </td><td>    JSON                                                      </td></tr>
 <tr><td>              Method                   </td><td>    POST                                                      </td></tr>
</table>


#### 4.1.2 Request 

    {
    “request” :
    {
      “SocialNetwork”:
      {
        “Type” : Facebook OR Twitter OR LinkedIn,
        “Id” : String 
      },
      “Transaction” :
      {
        “TransactionNumber” : String,
        “Currency” : String,
        “TotalDelivery” : Decimal,
        “TotalBeforeTax” : Decimal,
        “TotalDiscount” : Decimal,
        “TotalTax” : Decimal,
        “Timestamp” : DateTime,
        “Addresses” : 
        [
        “Address” : 
        {
          “Type” : AddressType,
          “FirstName” : String,
          “LastName” : String,
          “Address1” : String,
          “Address2” : String,
          “Address3” : String,
          “City” : String,
          “State” : String,
          “PostalCode” : String,
          “CountryIsoA2Code” : String
        }
        ],
        “Items” :
        [
        “Item” : 
        {
          “Name” : String,
          “Url” : String,
          “ImageUrl” : String,
          “Quantity” : Int,
          “TotalBeforeTax” : Decimal,
          “TotalDiscount” : Decimal,
          “TotalTax” : Decimal
        }
        ]
      },
      “SessionId” : Guid
    }
    }
    
#### 4.1.3 Response

    {
    “Message” : String,
    “Code” : Int,
    “TrustProfile” :
    {
      “OverallScore” : Decimal,
      “BillingAddressScore” : Decimal,
      “DeliveryAddressScore” : Decimal,
    }
    }


### 4.2 Add Transaction Status

This API method allows you to update the current status of the specified transaction. A newly created transaction is set to an initialised status. Upon a change in status (ie. succesful completion of order, failed payment, refund of order etc.) the transaction must be updated with the equivalent Trustev status.

#### 4.2.1 Format

<table>                        
 <tr><td width=308>    URL                      </td><td>    https://api.trustev.com/v1/Transaction/AddStatus          </td></tr>
 <tr><td>              Authentication Required  </td><td>    Yes                                                       </td></tr>
 <tr><td>              Format                   </td><td>    JSON                                                      </td></tr>
 <tr><td>              Method                   </td><td>    POST                                                      </td></tr>
</table>



#### 4.2.2 Request 
