---
title: 'Auto-generation of Adapter in MobileFoundation Service from OpenAPI specification of a micro-service'
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

## Overview of Adapters

Adapters are Maven projects that contain server-side code implemented in either Java or JavaScript. Adapters are used to perform any necessary server-side logic, and to transfer and retrieve information from back-end systems to client applications and cloud services. See more on Adapter [here](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters)

## Challenges in writing Adapters
An adapter can be created using either Maven commands or by using the MobileFirst CLI. The adapter code needs to be
written by the developers using the IDE of their choice, such as Eclipse or IntelliJ. Developer writes the server-side code that often connects to a back-end services to transfer or retrieve the information from back-end systems that eventually be used by the client applications or cloud service. This means that developer needs to fully understand the back-end service APIs and need to code all the logic by themseleves. Java adapters can use server-side Java APIs to perform operations that are related to MobileFirst Server, such as calling other adapters, logging to the server log, getting values of configuration properties, reporting activities to Analytics and getting the identity of the request issuer. It would be easy for developers if the an adapter code can be generated automatically for a back-end system/service. See more on creating adapters [here](http://mobilefirstplatform.ibmcloud.com/tutorials/en/foundation/8.0/adapters/creating-adapters)

## OpenAPI specification
The OpenAPI Specification, originally known as the Swagger Specification, is a specification for machine-readable interface files for describing, producing, consuming, and visualizing RESTful Web services. OpenAPI specification is adapoted by many companies as the industry standard for defining REST APIs. OpenAPI specifications are the key to supporting a great toolchain for developers and is used for documentation of the REST APIs. One of the great benefits to using OpenAPI Specification is that you can generate the client libraries in variety of languages and use the client APIs easily to transfer and retrieve information from the back-ends system for which the client libraties are generated.

## Auto-generation of Adapter from OpenAPI Specification
IBM Mobile Foundation Service provides an extension Adapters that can be used to auto-generate adapters from OpenAPI specification. The extension Adapter takes an OpenAPI specification and generates the adapter from the Open API specification, using the client libaries generated from the OpenAPI specification, and download the generated adatpers. The adapter that is generated and downloaded can then be deployed to Mobile Foundation Service and directly be used by the client applications. This makes the life of a adapter developers easy and the developers can focus more on building the client applications rather than spending time on writing the adapters

## Microservice Connector - Extension Adapter for generating adapter from OpenAPI Specification
IBM Mobile Foundation Service ships an Extension Adapter, called Microservice Connector, that can be download from the Download Center of MobileFirst Operations Console. This adapter needs to be downloaded and deployed on to the Mobile Foundation Server. 

    The MobileFoundation Dashboad showing information on fast tracking your app development using adapter autogeneration feature
    ![Naviage to Microservice Connector from Dashboard]({{site.baseurl}}/assets/blog/2017-07-21-autogenerate-adapter-from-openapi-specification/navigate-to-microservice-connector-from-dashboard.png)

    The Microservice Connector adapter, also known as Microservice Adapter Generator, can be downloaded from the Tools tab of Download Center
    
    ![Download Microservice Connector]({{site.baseurl}}/assets/blog/2017-07-21-autogenerate-adapter-from-openapi-specification/microservice-connector-in-downloadcenter-tools.png)

    Once deployed the Adapter will be appearing unser the Extensions category in the left navigation pane of the MobileFirst Operations Console and clicking on the Microservice Adapter Generator will open up the page where you can select the OpenAPI specification json of your microservice/back-end system and generate the adapter. The generated adapter will be automatically downloaded that you can deploy on to MFP server.

    ![Microservice Connector]({{site.baseurl}}/assets/blog/2017-07-21-autogenerate-adapter-from-openapi-specification/microservice-adapter-generator-ui.png)


   
