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

Endpoint 

>https://auth.vatsim.net/oauth/authorize

What should the url which I am going to redirect a user to look like?

>https://auth.vatsim.net/oauth/authorize/client_id=8&redirect_uri=https%3A%2F%2Fexample.com%2Fconnect%2Fcallback&response_type=code&scope=full_name+vatsim_details+email+country

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

Response from VATSIM

# Available Scopes 

# Return Data Structure



# Implementation Contents
  - [PHP](#php)
  - Python
  - Go
  - DotNet
  - [Laravel](#laravel)
  - OsTicket
  - PHPBB Forums
  - IPBoard Forums
 
> Is your preffered implementation method missing? Submit your contribution to this documentation via a PR.

# Implementation

## PHP
~~~php
<?php echo 'some code';?>
~~~

## Laravel
~~~php
<?php echo 'some beautiful laravel code';?>
~~~
