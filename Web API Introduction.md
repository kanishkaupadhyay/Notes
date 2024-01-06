# API

## API - Application Programming Interface

1. Application - Software that does a task
2. Programming - Program that does the task in the Application
3. Interface - Place to tell the program to run in the application

## API is great

1. Just use the program, don't have to write it!
2. Platform independent
3. Upgrade safe

> e.g. Google search using Computer
1. Interface - Computer
2. Program - Search
3. Application - Google

## API in detail

1. A request is made for something to be done
3. A program is run to complete that request
3. A program sends a response

> e.g. Google search with a parameter : www.google.com/search?q=JBL

## API Mashup

API from other APIs

> e.g. Agoda, MakeMyTrip

# Web Service

1. Web - internet
2. Service - API
3. Web Service - API that uses the internet

- All web services are APIs but all APIs are not web services.  
- Web services use XML/JSON to format data over the internet.  
- REST, SOAP, or XML/RPC to transfer that data.

# HTTP

## Introduction to HTTP

Hyper Text Transfer Protocol

**Request**
1. Start Line : Version HTTP1.1, METHOD, Parameters
2. Headers :  Host: www.google.com, Tokens
3. Blank Line : Separates header from the body
4. Body : Attributes

**Response**
1. Start Line : Version HTTP/1.1, Method, Status code
2. Headers :  Cookies, Size, Response Format
3. Blank Line : Separates header from the body
4. Body : HTML, JSON, XML

### Start Line
First line in the http request and response.

**Request**

1. Name: Start Line, Request Line
2. HTTP Version: HTTP/1.1
3. Method: GET, POST, PUT, DELETE etc
4. API Program Folder Location: Yes
5. Parameters: Yes (eg. ?q=JBL)
6. Format: Method(space)API Program Folder Location+Parameters(space)HTTP Version
7. Example: GET /search?JBL HTTP/1.1

**Response**
1. Name: Start Line, Response Line, Status Line
2. HTTP Version: HTTP/1.1
3. Status Code: Yes (eg. 200 OK)
4. Format: HTTP Version + Status Code
5. Example: HTTP/1.1 200 OK


### Header Line
It contains request fields like Accept-Language, Cookie, Date, Cache-Control etc and response fields like Server, Last-Modified, Content-Type

### Blank Line

### Body
It contains the *content* that you want to sent to the API or the *content* that is recieved from the API as a response.

### Advanced Topics/Terms

1. **Stateless** : Request unknown  
    - REST and SOAP are stateless by default
2. **REST** : Representational State Protocol
3. **SOAP** : Simple Object Access Protocol
4. **Stateful** : File transfer Protocol(FTP) 
5. **Cookie** : Cookies are not executable  
    - Cookies are a part in the header file.
    - Cookies don't store password.  
    - Application store data with session id.  
    - Multi-Factor Authentication.
    - Antivirus software

# XML
> e**X**tensible **M**arkup **L**anguage
1. HTTP Header Line: Content-Type: application/xml
2. HTTP Body: XML
3. XML uses tags <> just like HTML
4. WSDL: Web Service Description Language
``` 
<cs>
    <os>Paging</os>
    <dbms>Transactions</dbms>
    <ai>
        <ml>Supervised</ml>
        <nn>Convolution Neural Network</nn>
    </ai>
</cs>
```

# JSON
> **J**ava**S**cript **O**bject **N**otation
1. HTTP Header Line: Content-Type: application/json
2. HTTP Body: JSON
3. JSON uses "Key" : "Value" notation
``` 
{
    "cs" : 
    {
        "os" : "Paging",
        "dbms" : "Transaction",
        "ai" : 
        {
            "ml" : "Supervised",
            "nn" : "Convolution Neural Network"
        }
    } 
}
```

## SOAP 
- Simple Object Access Protocol
- Rules to form HTTP Request/Response
- Uses a WSDL (Web Service Description Language)

## REST
- Representational State Transfer.
- Caching is done to minimize the time it takes for the web page to load by saving the page in the memory.
- REST uses JSON for data transfer using HTTP.
- REST does not have any rules.

## HTTPS
- Secure way to transfer data (encrypted)

## Authentication
- Proving your identity.

## Authorization
- Limited access.

## Authentication and authorization example
1. No Auth
2. Basic Auth
3. Bearer Token
4. OAuth
5. Two factor auth

## Apps
Native - OS
Web App - Browser
Hybrid App

## OAuth 2.0 RFC note 6749 specs
- OAuth allows a 3rd party access.
- Limited (authorised) access.
- To a web service (http).
- Problem giving credentials to 3rd party.
- OAuth introduces authorization layer.
- Instead of credentail 3rd party gets access token.
- Must use HTTP. 

## Roles
- Resource owner
- Resource server
- Client/Application
- Authorization server

## Protocol Flow
1. Client -> Authorization Request -> Resource Owner
2. Resource Owner -> Authorization Grant -> Client
3. Client -> Authorization Grant -> Auth Server
4. Auth Server -> Access Token -> Client
5. Client -> Access Token -> Resource Server
6. Resource Server -> Protected resource -> Client

**Access Token and Refresh Tokens**

## Section 2: Client Registration
1. Client HTTP to Auth Server:
    - Client Type (public, confidential)
    - Redirect URL
    - Client Secret
    - Additional Info (name, email, etc)

2. Auth Server HTTP to Client:
    - Client ID
    - Client Secret

## Section 4.1.1 Authorization Request
1. User clicks HTTP link to authorization endpoint HTTPRequest
    - response type
    - client id
    - redirect uri
    - scope
    - state(secret)

## Section 4.1.3 Access Token Request
1. HTTP method may be POST or GET sends:
    - Grant type = authorization code
    - Code = the authorization code 
    - client id
    - client secret

## Section 4.1.4 Access token request
1. Auth server sends back HTTP to client with:
    - Access token
    - Token type
    - Expires in (seconds)
    - Refresh token (optional)
    - Scope (optional)

## Open ID Connect 
To get the information about the resource owner


## Bearer Token
- Bearer : Holder
- Token : used to get access or something
- Bearer token : holder can get access to the API using this token
- Very simple : If you have the token, you get the access
- Must protect the token to not get in the hands of the unauthorized person
- Must use HTTPS

## OAuth
- Open Authorization
- Open : open to another
- Authorization : limited access
- OAuth API : delegate limited authorizaiton to an application (app)

## Webhooks
- Web: Web Service 
- Hook: connected to an event 
- Webhook is a reverse api
- e.g. a payment a occurred, time related, login

## Process of Webhooks
- Triggers an Event
- Configurations (endpoints to send the request to)
- Request
- Response
  
## Microservices
- Micro: Small 
- Services: multiple APIs
- Small APIs 

## Advantages
- Scalability
- Code language independent
- Smaller and specialized teams

## Disadvantages
- Lack of consistency
