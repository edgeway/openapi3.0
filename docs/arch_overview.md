# Overview
<hr />

## Architectural style
 Microservices architecture is selected for known advantage of the architecture 
<br />

  ![Banner](https://raw.githubusercontent.com/arpsch/openapi3.0/master/docs/microservice_architecture.PNG)
  
  **useradm**: Handles user administration and role assignment.
  
  **deviceauth**: Handles device authorisation, any request from the device will be handled by this service before authorization.

  **deployments**: Handles Over the air (OTA) updates, scheduling, triggering and etc...

  **configuration**: Handles configurations creation, updation and deletion.

  **localconf**: Local configurations, this handles storing the local configruations of the each device in the fleet. This will help to restore the device if it gets bricked for any reason without much hassles.

  **actions**: This handles te actions triggered on the device. Every action will be tracked for the status after triggering and stores the updates from the device.

  **mongoDB**: MongoDB is used to as persistant store, this has a seperate database created from each service needing persistant store.

  **multitenant**: This service handles multitenancy of the fleet, creation and management.

## Failsafe Over The Air (OTA) updates
To be failsafe a redundant approach is followed with **active** and **passive** partitions. Device will use active partition to boot normally. Downloads will will happen on passive partion. If artefact verification succeeds, active and partions are swapped. In the next boot new image will be boot. In case of failure of artefact verification, nothing will be done, box still boots with old version.

*Note*: as the filesystem is stateless updates will overwrite personall settings or modifications. Such modifications need to stored in separate partion.

## Notifications
Device supports three types of notifications

**Info**: Usual information of the device.

**Warning**: Warning alert with minimal information of system which generated it.

**Critical**: Any critical event that happened with minimal information of originating system.

## Authentication
For authentication of all the APIs - device, user are authenicated by the JSON Web Tokens (JWT). Relying on the power of JWT to securely transact. Server will renew valid JWT when it expired during the session.
