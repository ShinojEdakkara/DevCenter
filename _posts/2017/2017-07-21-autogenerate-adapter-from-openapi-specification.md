```
---
title: 'Auto Generate Adapters for Microservices and backend systems from it's OpenAPI Specification'
date: 2017-07-31
tags:
- MobileFirst_Foundation
- Bluemix
- Adapter
version:
- 8.0
author: 
name: Shinoj Zacharias
additional_authors:
- Yathendra P Neela
---
```

## Overview of Adapters

Adapters are Maven projects that contain server-side code implemented in either Java or JavaScript. Adapters are used to perform any necessary server-side logic, and to transfer and retrieve information from backend systems to client applications and cloud services. See more on [Developing Adapters](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters)

### Challenges in developing Adapters
An adapter can be created using either Maven commands or by using the MobileFirst CLI. The adapter code needs to be written by the developers using the IDE of their choice, such as Eclipse or IntelliJ. Developer writes the server-side code that often connects to a backend services to transfer or retrieve the information from backend systems that eventually be used by the client applications or cloud service. This means that developer needs to fully understand the backend service APIs and need to code all the logic by themseleves. Java adapters can use server-side Java APIs to perform operations that are related to MobileFirst Server, such as calling other adapters, logging to the server log, getting values of configuration properties, reporting activities to Analytics and getting the identity of the request issuer. It would be easy for developers if the an adapter code can be generated automatically for a backend system/service. See more on [Creating Adapters](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/creating-adapters)

## OpenAPI specification
The OpenAPI Specification, originally known as the Swagger Specification, is a specification for machine-readable interface files for describing, producing, consuming, and visualizing RESTful Web services. OpenAPI specification is adapoted by many companies as the industry standard for defining REST APIs. OpenAPI specifications are the key to supporting a great toolchain for developers and is used for documentation of the REST APIs. One of the great benefits to using OpenAPI Specification is that you can generate the client libraries in variety of languages and use the client APIs easily to transfer and retrieve information from the backend systems.

## Auto-generation of Adapter from OpenAPI Specification
IBM Mobile Foundation Service provides an extension Adapters that can be used to auto-generate adapters from OpenAPI specification. The extension Adapter takes an OpenAPI specification and generates the adapter from the Open API specification, using the client libaries generated from the OpenAPI specification, and downloads the generated adatpers. The adapter that is generated and downloaded can then be deployed to Mobile Foundation Server and directly be used by the client applications. This takes the pain out of developers who need to spends lot of time on developing the adapters rather they can focus more on building the client applications.

### Microservice Connector - Extension Adapter for generating adapter from OpenAPI Specification
IBM Mobile Foundation Service ships an Extension Adapter, called Microservice Connector, that can be download from the Download Center of MobileFirst Operations Console. This adapter needs to be downloaded and deployed on to the Mobile Foundation Server. 

    The MobileFoundation Dashboad showing information on fast tracking your app development using adapter autogeneration feature
    
`![Navigate to Microservice Connector from Dashboard]({{site.baseurl}}/assets/blog/2017-07-21-autogenerate-adapter-from-openapi-specification/navigate-to-microservice-connector-from-dashboard.png)`

    The Microservice Connector adapter, also known as Microservice Adapter Generator, can be downloaded from the Tools tab of Download Center

`![Download Microservice Connector]({{site.baseurl}}/assets/blog/2017-07-21-autogenerate-adapter-from-openapi-specification/microservice-connector-in-downloadcenter-tools.png)`

    Once deployed the Adapter will be appearing under the Extensions category in the left navigation pane of the MobileFirst Operations Console. Clicking on the Microservice Adapter Generator will open up the page where you can select the OpenAPI specification json of your microservice/backend system to generate the adapter. The generated adapter will be automatically downloaded that you can deploy on to MFP server.

`![Microservice Connector]({{site.baseurl}}/assets/blog/2017-07-21-autogenerate-adapter-from-openapi-specification/microservice-adapter-generator-ui.png)`

### Generating a working adapter from OpenAPI specification
The correctness of the adapter generated depends on the OpenAPI specification. There may be cases where OpenAPI specification doesn't match with that of REST API of the backend service. In this case , even if the adapter generation is successful, the call from adapter to the backend will not work because of the mismatch in  specification and the backend REST API. In the case, unless the specification is corrected, you won't get a working adapter. 

#### Adding Security in OpenAPI specification
Adapter generator support BASIC authentication only. Authentication types such as API Key and OAUTH2 is not supported. For BASIC authentication, security definition specification needs to be added to the specification json

```
"securityDefinitions": {
    "basicAuth": {
        "type": "basic",
        "description": "HTTP Basic Authentication."
    }   
},
"security": [
{
    "basicAuth": []
}
],

```

#### Adding host and basepath in OpenAPI specification

Make sure that the host with correct schemes is provided in the specification and also the basePath of the API

```
"host": "gateway.watsonplatform.net",
"schemes": [
"https"
],
"basePath": "/natural-language-understanding/api"

```

### Adding Custom Scope for Adapter REST Endpoints

If there is no scope added in the OpenAPI specification, a default scope is added to all the REST endpoints. You can add custom scope either at the global level or for individual REST endpoints, here is an example of adding scope globally

```
"securityDefinitions": {
    "basicAuth": {
        "type": "basic",
        "description": "HTTP Basic Authentication."
    },
    "OauthSecurity": {
        "type": "oauth2",
        "flow": "implicit",
        "scopes": {
            "adminauth": "Admin scope",
            "customauth": "custom scope"
        }
    }
},
"security": [
    {
        "basicAuth": []
    },
    {
        "OauthSecurity": [
            "customauth"
        ]
    }
]
```

Here is an example of overriding the global scope in each of the REST endpoints

```
"paths": {
    "/v1/workspaces": {
        "parameters": [
        {
            "$ref": "#/parameters/VersionQueryParam"
        }
        ],
        "post": {
            "summary": "Create workspace",
            "description": "",
            "tags": [
                "Workspaces"
            ],
        "security": [
        {
            "basicAuth": []
        },
        {
        "OauthSecurity": [
            "adminauth"
        ]
    }
],
"consumes": [
    "application/json"
],
"produces": [
    "application/json"
],
...
...

```

#### Empty Response Object

If there is an empty reponse object in the specification, MFP has issue with generation of the adapter, so we recommend to remove the empty reponse object in openAPI specification

Here is an example of empty response object :

```
"responses": {
    "200": {
        "schema": {
        "$ref": "#/definitions/EmptyObject"
    },
    "description": "Successful request."
},

"EmptyObject": {
    "properties": {}
}

```

In the above example, EmptyObject don't have any properties defined and is empty. If you have this kind of specification in the json, MFP will have issue with generation of adapter, in this case we recommend to remove the EmptyObject schema from the respose object, that means, the above specification should be changed to :

```
"responses": {
    "200": {
        "description": "Successful request."
    }
}

```

#### Missing Consumes and Produces Content-types in specification

All the REST endpoints in the OpenAPI specification should have Consumes and Produces Content-types. The generated adapter can fail in calling the backend service if this is not specified in the specification

```
"consumes": [
    "application/json"
],
"produces": [
    "application/json"
]
```

#### Mismatch in specification on request and response contents
Many of the OpenAPI specification is not usually updated with the changes in the backend REST API. This can results adapter call failing due to the content mismatch. Make sure that the request and response contents in the specification matches with what is expected by the backend REST API


