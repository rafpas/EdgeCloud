
# OPAG-CAMARA Traffic Influence API
## Overview

The reference scenario foresees a Service provided by many Application Server instances deployed in different geographical location on Telco Operator Edge sites (MEC sites).
The Traffic Influence API provides the fastest routing from the user equipment (e.g. a Smartphone) to the optimal Edge Application Server (EAS) Instance in a specific geographical location.
If a Service is offered by a Cloud Instance and by Edge Instances, the API can be used to route a selected set of users on the Edge Instance.

## 1. Introduction

The Traffic Influence API provides the capability to establish the best connectivity, in terms of latency, in a specific geographical area, between the User Equipment (UE), e.g. the user’s smartphone, and the optimal Edge Application Server instance nearby.
The Service architecture foresees one or more EASs and a component, the Application Function (AF), that is the API consumer. Invoking the TI API, the AF can create a "TrafficInfluence" resource specifying the desired request.
The TI API provider implements the intent specified in the Traffic Influence resource.
While the API is usually invoked to activate the fastest routing for any user,  it can also be used to request the routing for a specific user. Invoking the API for each user (using the same "trafficInfluenceID"), the TrafficInfluence resource provides the requested routing for a set of users.

## 2. Quick Start
The usage of the Traffic Influence API is based on the management of a "TrafficInfluence" resource, an object containing the intent requested invoking the Traffic Influence API and that is implemented by the platform configuring the Mobile Network for the optimal routing toward the EAS Instance.
The Traffic Influence resource can be created (providing the related parameters that specify the desired intent), queried, modified and deleted.
The API is asynchronous, a notification is available providing information about the status of the requested resource.  

Before starting to use the API, the developer needs to know about the below specified details:

**Base-Url**
The RESTful Traffic Influence API endpoint, for example [**https://tim-api.developer.tim.it/trafficinfluence**](https://tim-api.developer.tim.it/trafficinfluence)

**Authentication**
Configure security access keys such as OAuth 2.0 client credentials to be used by Client applications which will invoke the Traffic InfluenceAPI.

**TrafficInfluence**
This object represents the resource that carries the requirements from the user to be implemented. The Traffic Influence API is invoked for the life cycle management of this resource (CRUD). The resource contains the intents from the API Consumer. Managing this resource, the developer can specify in which geographical location the routing must be applied, toward which application,  maybe for a specific set of users or for a limited period pf time.

**trafficInfluenceID**
Identifier for the Traffic Influence resource. This parameter ir returned by the API and must be used to update it (e.g., adding new users or deleting it)

**applicationFunctionId**
Unique identifier for the Application Function (AF), the TI API consumer.

**region**
The developer can specify in which geographical area he requires the fastest routing toward the nearest application instance. A Region is a wide geographical area and it contains one ore more Zones. A Zone is where the Edge Datacenters are located.

**zone**
The developer can specify in which geographical area he requires the fastest routing toward the nearest application instance. A Zone is a smaller geographical area inside a Region. A Zone is where the Edge Datacenters are located.

**applicationId**
Unique Application identifier inside the Telco Operator Platform.

**trafficFilters**
The Application can expose different service on different interfaces, with this parameter it is possible to enable just some of those services maybe for different sets of users.

**usersId**
Optionally a user can be provided as an input. The routing toward the selected Application Instance is only applied for that user. To add more users the API must be invoked of each one always passing the reference to the same Traffic Influence resource (trafficInfluenceID)

**Notification URL and token**
Developers have a chance to specify callback URL on which notifications (eg. session termination) regarding the session can be received from the service provider. This is also an optional parameter.

## 3. Authentication and Authorization
The Traffic Influence API makes use of the client credentials grant which is applicable for server to server use cases involving trusted partners or clients without any protected user data involved.
In this method the API invoker client is registered as a confidential client with an authorization grant type of client\\_credentials [2].

## 4. API Documentation
## 4.1 Details

The Traffic Influence (TI) API is consumed by an Application Function (AF) requesting for the fastest routing of the traffic flow from the User Equipments toward an instance of the Edge Application Server (EAS) in a Telco Operator Edge sites (MEC sites).

When the EAS is onboarded and deployed in the MEC site, the Application is identified with a unique identifier (applicationId).

Using the application identifier ("applicationId") and specifying a Zone or a Region, the Telco Operator Platform, autonomously identifies the best instance of the EAS toward routing the traffic flow and configures the Mobile Network accordingly to get the fastest routing.
If just the application identifier is used, the Telco Operator Platform  identifies all the EAS Instances and activates the optimal routing on the Mobile Network.
If the fastest routing should be available just for a set of users, the API must be invoked for each user also passing the same TrafficInfluce resource identifier ("trafficInfluenceID")
If the Application offers different services on different interfaces a  traffic filter based on IP, Port and Protocol can be used. I this way it is also possible to redirect different users to different interfaces.

Here are some possible intents:

1) activate the optimal routing for any EAS Instance: the API must be invoked with the applicationId. The Telco Operator Platform identifies all the EAS Instances and activates the optimal routing on the Mobile Network.
2) activate the optimal routing in a specific Region or Zone: the API must be invoked with the applicationId and the Zones and Regions identifiers.
3) activate the optimal routing for a specific set of users: the API can also be invoked with a user identifier ("userId"). The same TrafficInfluce resource identifier ("trafficInfluenceID") must be used in each call.

### 4.2 Endpoint Definitions
## Version: 0.8.0

**License:** [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0.html)

[Product documentation at Camara](https://github.com/camaraproject/)
### /traffic-influences

#### GET
##### Summary

Retries existing TrafficInfluence Resources

##### Description

Reads all of the active TrafficInfluence resources owned by the AF authenticated via oAuth2

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | Returns the information about existing TrafficInfluence resources. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 500 | An unknow error has occurred |
| 503 | Service unavailable |
| 504 | Connection timeout towards backend service has occurred |

#### POST
##### Summary

Creates a new TrafficInfluence resource

##### Description

Gets in input an object containing the intents from the AF and creates a TrafficInfluence resourse accordingly.

##### Responses

| Code | Description |
| ---- | ----------- |
| 201 | TrafficInfluence resource created, the related object is returned with the resource ID (TIResource_id) valorised |
| 401 | Unauthorized |
| 403 | Forbidden |
| 500 | An unknow error has occurred |
| 503 | Service unavailable |
| 504 | Connection timeout towards backend service has occurred |

### /traffic-influences/{TIResource_id}

#### GET
##### Summary

read a specific TrafficInfluence resource identified by TIResource_id

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| TIResource_id | path | Identifier of the specific TrafficInfluence resource to be retrieved, modified or deleted | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | OK. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | The specified resource was not found |
| 500 | An unknow error has occurred |
| 503 | Service unavailable |
| 504 | Connection timeout towards backend service has occurred |

#### PUT
##### Summary

updates a specific TrafficInfluence resource, identified by TIResource_id

##### Description

The resource identified by TIResource_id can be modified, e.g. to add new users or to modify the validity time period

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| TIResource_id | path | Identifier of the specific TrafficInfluence resource to be retrieved, modified or deleted | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | OK. |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | The specified resource was not found |
| 500 | An unknow error has occurred |
| 503 | Service unavailable |
| 504 | Connection timeout towards backend service has occurred |

#### DELETE
##### Summary

Delete an existing TrafficInfluence resource

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| TIResource_id | path | Identifier of the specific TrafficInfluence resource to be retrieved, modified or deleted | Yes | string |
| callbackUrl | query | the location where updated data will be sent.  Must be network accessible by the source server  | No | string (uri) |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | The resource has been successfully deleted |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | The specified resource was not found |
| 503 | Service unavailable |
| 504 | Connection timeout towards backend service has occurred |

### Models

#### TrafficInfluenceNotification

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| trafficInfluenceChanged | object |  | Yes |

#### TrafficInfluence

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| trafficInfluenceID | string | Defines the local breakout required for the particular Telco Edge Node and Service Edge Node. I.E. TurinEast, TurinNorth | No |
| applicationFunctionId | string | Unique Identifier of the Application Function caller | Yes |
| applicationId | string | Unique ID representing the Edge Application<br>_Example:_ `"Virtual_Reality_Arena"` | Yes |
| region | string | Unique identifier representing a region  | No |
| zone | string | Unique identifier representing a zone  | No |
| usersId | object | User equipment identifier, it is composed by a value and a type for that value | No |
| state | string | _Enum:_ `"ordered"`, `"created"`, `"active"`, `"not active"`, `"error"`, `"deleted"` | No |
| trafficFilters | [ object ] | Identifies IP packet filters. To be used when a the Application needs a traffic flow towards a specific EAS interface | No |
| notificationUri | string | Defines the callback uri which should be notified in asynchronous way when the state for the requested resources changes (i.e. ordered to activated) | No |
| notificationAuthToken | string | Authentification token for callback API | No |

#### types_zone_Id

Unique identifier representing a zone

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| types_zone_Id | string | Unique identifier representing a zone  |  |

#### types_region_Id

Unique identifier representing a region

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| types_region_Id | string | Unique identifier representing a region  |  |

#### userId

User equipment identifier, it is composed by a value and a type for that value

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| UEIdentityType | string | Type of User Equipment identifier used in `UEIdentity`.<br>_Enum:_ `"IPAddress"`, `"MSISDN"`, `"IMEI"`, `"MDN"`, `"GPSI"` | No |
| UEIdentity | string | Identifier value for User Equipment. The type of identifier is defined by the 'UEIdentityType' parameter. | No |

#### types_UEIdentityType

Type of User Equipment identifier used in `UEIdentity`.

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| types_UEIdentityType | string | Type of User Equipment identifier used in `UEIdentity`. |  |

#### types_UEIdentity

Identifier value for User Equipment. The type of identifier is defined by the 'UEIdentityType' parameter.

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| types_UEIdentity | string | Identifier value for User Equipment. The type of identifier is defined by the 'UEIdentityType' parameter. |  |

#### FlowInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| flowId | integer | Indicates the IP flow. | Yes |
| flowDescriptions | [ string ] | Indicates the packet filters of the IP flow. | No |

### 4.3 Errors
#### ErrResponse

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| status | string | _Example:_ `"OK"` | No |
| message | string | _Example:_ `"OK"` | No |

#### ErrorInfo

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | string | Code given to this error | Yes |
| message | string | Detailed error description | Yes |
### 4.4 Policies

N/A

### 4.5 Code Snippets

N/A
###  4.6 FAQ's

(FAQs will be added in a later version of the documentation)

###  4.7 Terms

N/A

###  4.8 Release Notes

The Traffic Influence API reduces the complexity of the 3GPP Traffic Influence API exposed by the 3GPP Network Exposure Function (NEF) [1]. While the 3GPP TI API offers fastest routing activation and user mobility among different edge sites, this version of the CAMARA Traffic Influence API covers only the fastest routing activation, also for selected users. User mobility will be introduced in a future version.

##  References

[1] 3GPP TS 23.501: System architecture for the 5G System (5GS); Stage 2 (Release 17), V17.4.0 (2022-03)
[2] Camara Commonalities : Authentication and Authorization Concept for Service APIs https://github.com/camaraproject/WorkingGroups/blob/main/Commonalities/documentation/Working/CAMARA-AuthN-AuthZ-Concept.md