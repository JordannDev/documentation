<p align="center"><img src="https://www.vatsim.net/sites/default/files/vatsim_0.png" width="400"></p>

# VATSIM Connect

VATSIM Connect is an OAuth2.0 authentication service which has been designed to replace the current OAuth 1.0 implementation.

# Process of Implementation
1. Sign into https://auth.vatsim.net and register an organization. Approval will NOT be instant and may take some time, please be patient. When your organization has been activated, the member who registered the organization will be notified via email. 
2. Implement Connect into you application.
3. Share your implementation with the Web Team to have it made available to the entirety of VATSIM.

# OAuth 2.0 (Connect) Authentication Flow
## 1. Redirect the user to connect to obtain an authorization code.<br>
To begin the authentication flow, redirect the user with the following information to the authorize endpoint specified below. The information should be given through the query string of the redirect. 

Required Information

> client_id (Given after Creating a Client through the Dashboard)<br>
> redirect_uri (Location in which a user should be redirected back to with an authorization code, user specified per client)<br>
> response_type (Always 'code')<br>
> scope (The requested scopes as a string, e.g. 'full_name vatsim_details email country')<br>

### Available Scopes 
```
"full_name" => Includes their First_name and Last_name.
"email" => Registered VATSIM email.
"vatsim_details" => Includes ATC Rating, Pilot Rating, Region, Division and Sub Division.
"country" => Registered country of residence. 
```

Endpoint 

>https://auth.vatsim.net/oauth/authorize

What should the url which I am going to redirect a user to look like?

>https://auth.vatsim.net/oauth/authorize?client_id=8&redirect_uri=https%3A%2F%2Fexample.com%2Fconnect%2Fcallback&response_type=code&scope=full_name+vatsim_details+email+country

## 2. Use the Authorization Code to Request an Access Token
Once a user has been redirected back from VATSIM you will then need to utilise the authorization code returned to gain an access token. The process of gaining an access token is quite simple, conduct a POST request to the token endpoint with the required information which will then return an access token and a refresh token. 

Response from VATSIM
Once the user has returned, a code will be given through a query string to the specified redirect_uri. 

Required Information 

> grant_type (Always 'authorization_code')<br>
> client_id (Given after Creating a Client through the Dashboard)<br>
> client_secret (Given after Creating a Client through the Dashboard)<br>
> redirect_uri (The same redirect_uri as when requesting an authorization code)<br>
> code (The code returned to specified redirect_uri after a user has return from VATSIM Connect)<br>

Authorization Endpoint

> https://auth.vatsim.net/oauth/token

**Please ensure you are making a POST request, not a GET request**

Response from VATSIM Example<br>
```
{
   "scopes":[
      "full_name",
      "vatsim_details",
      "email",
      "country"
   ],
   "token_type":"Bearer",
   "expires_in":604800,
   "access_token":"<access_token>",
   "refresh_token":"<refresh_token>"
}
```

### Token Validity & Continual Access
VATSIM allows users to choose whether they wish to provide continual access to their data. <br>
##### Should they wish NOT to provide continual access. <br>
  - Access Token is valid for 7 days.
  - Access Token is invalidated after one call to the user data endpoint. 
  - Refresh Token provided is invalid and cannot be used to refresh the access token.<br>
##### Should they wish to provide continual access.<br>
  - Access Token is valid for 7 days.
  - Access Token is valid for multiple api calls unless invalidated/revoked by the user.
  - Refresh Token is valid for 30 days and can refresh the access token provided the user has not invalidated/revoked the token. 
The term invalidated/revoked is when a user revokes access to an organisation through the "My Data" page at https://auth.vatsim.net.

## 3. Use the Access Token to get the users data
In order to gain a users data, you must make a GET request to the API with a valid access_token. The access token was returned in the JSON array in part 2. 

Make a get request to the API with the following headers.

```'Authorization' => 'Bearer <token>```
```'Accept' => 'application/json'```

API Endpoint
>https://auth.vatsim.net/api/user

#### Return Data Structure
Please Note: This includes all scopes.
```
{
   "data":{
      "cid":101,
      "personal":{
         "name_first":"Web",
         "name_last":"Team",
         "email":"web@vatsim.net",
         "country":{
            "id":"AU",
            "name":"Australia"
         }
      },
      "vatsim":{
         "rating":{
            "id":8,
            "long":"Instructor",
            "short":"I1"
         },
         "pilotrating":{
            "id":0
         },
         "division":{
            "id":"PAC",
            "name":"Australia (VATPAC)"
         },
         "region":{
            "id":"OCEN",
            "name":"Oceania"
         },
         "subdivision":{
            "id":null,
            "name":null
         }
      },
      "oauth":{
         "token_valid":"true"
      }
   }
}
```
# Development Environment

|CID|Password|ATC Rating|Pilot Rating|Region|Division|SubDivision|
|-----------|---------|------------------|--------------|------|------|------|
| 10000001 | 10000001 | Observer | NO P Rating | VATOCE | VATPAC | NULL |
| 10000002 | 10000002 | S1 Student | P1 | VATNA | VATUSA | NULL | 
| 10000003 | 10000003 | S2 Student | P1,P2 | VATAME | VATIL | NULL |
| 10000004 | 10000004 | S3 Student | P1, P2, P3 | VATEUR | VATEUD | VATSPA |
| 10000005 | 10000005 | C1 Controller | P1, P3 | VATASIA | VATWA | VATAFG | 
| 10000006 | 10000006 | C3 Senior Controller | P2, P3 | VATEUR | VATEUD | VATEIR |
| 10000007 | 10000007 | I1 Instructor | P1, P2, P3, P4 | VATEUR | VATUK | NULL |
| 10000008 | 10000008 | I3 Senior Instructor | P1, P4 | VATAME | VATNAF | NULL |
| 10000009 | 10000009 | SUP Supervisor | P2, P3, P4 | VATAME | VATSAF | NULL |
| 10000010 | 10000010 | ADM Administrator | P3, P4 | VATEUR | VATEUD | VATFRA |
| 10000011 | 10000011 | Suspended | P1, P3, P4 | VATOCE | VATNZ | NULL |

# Implementation Contents
  - Python (WIP)
  - Laravel (WIP)
  - [OsTicket](https://github.com/vatsimnetwork/connect-osticket)
  - NodeJS (WIP)
  - Express (WIP)
  - [IPBoard Forums](https://github.com/vatsimnetwork/connect-ipboard)
 
> Is your preffered implementation method missing? Submit your contribution to this documentation via a PR.
