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
