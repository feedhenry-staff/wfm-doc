<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Module Integration](#module-integration)
  - [Overview](#overview)
  - [Scenario](#scenario)
  - [Changes Necessary To Integrate The Raincatcher-GPS Module](#changes-necessary-to-integrate-the-raincatcher-gps-module)
    - [1. Update the raincatcher-demo-mobile app to add the new raincatcher-gps module](#1-update-the-raincatcher-demo-mobile-app-to-add-the-new-raincatcher-gps-module)
      - [a. Include the raincatcher-gps module in the main app dependencies](#a-include-the-raincatcher-gps-module-in-the-main-app-dependencies)
      - [b. Create a new service to publish and subscribe to location events](#b-create-a-new-service-to-publish-and-subscribe-to-location-events)
      - [c. Add the new location service as a dependency of the wfm-mobile.auth module](#c-add-the-new-location-service-as-a-dependency-of-the-wfm-mobileauth-module)
    - [2. Update the raincatcher-user module to propagate user location updates to the Cloud](#2-update-the-raincatcher-user-module-to-propagate-user-location-updates-to-the-cloud)
      - [a. Update the UserClient to subscribe to the user location update topic.](#a-update-the-userclient-to-subscribe-to-the-user-location-update-topic)
      - [b. Update the backend cloud router with the new location update endpoint](#b-update-the-backend-cloud-router-with-the-new-location-update-endpoint)
      - [c. Update the MBaaS service endpoint to handle proxied requests from the backend cloud app](#c-update-the-mbaas-service-endpoint-to-handle-proxied-requests-from-the-backend-cloud-app)
    - [3. Update the raincatcher-demo-auth app to handle location update topics](#3-update-the-raincatcher-demo-auth-app-to-handle-location-update-topics)
    - [4. Update the raincatcher-map module to handle displaying user locations on the map](#4-update-the-raincatcher-map-module-to-handle-displaying-user-locations-on-the-map)
      - [a. Add the worker scope to the directive](#a-add-the-worker-scope-to-the-directive)
      - [b. Add the addUserMarkers function](#b-add-the-addusermarkers-function)
    - [5. Update the raincatcher-demo-portal application to pass users to the raincatcher-map scope](#5-update-the-raincatcher-demo-portal-application-to-pass-users-to-the-raincatcher-map-scope)
      - [a. Update the portal map module config to resolve workers](#a-update-the-portal-map-module-config-to-resolve-workers)
      - [b. Add the workers parameter to the map directive](#b-add-the-workers-parameter-to-the-map-directive)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Module Integration

## Overview

This document presents a guide to how a new Raincatcher module would be integrated into the existing Demo Solution to provide extra functionality.

It is recommended that you have a [local setup](Running-Locally.md) of the Raincatcher solution set up before following this guide. This will allow for easier editing and feedback.

## Scenario

The existing Raincatcher solution is capable of showing the current location of work orders on a map. However, what if an administrator would like to see where workers are at any give time? It is not currently a feature in the Raincatcher Demo project.

This guide will introduce this functionality by integrating a new raincatcher module: [raincatcher-gps](https://github.com/feedhenry-raincatcher/raincatcher-gps). This module publishes and subscribes to location based topics. 


## Changes Necessary To Integrate The Raincatcher-GPS Module


There are several changes required to integrate the raincatcher-gps module into the Demo Project: 

- Update the raincatcher-demo-mobile app to add the new raincatcher-gps module.
- Update the raincatcher-user module to propagate user location updates to the Cloud.
- Update the raincatcher-demo-auth app to handle location update topics.
- Update the raincatcher-map module to handle displaying user locations on the map.
- Update the raincatcher-demo-portal application to pass users to the raincatcher-map scope.

### 1. Update the raincatcher-demo-mobile app to add the new raincatcher-gps module

The goal of this change is to capture the location of mobile workers in the field as they complete work orders. To do this, we can add the raincatcher-gps module to the raincatcher-gps module by:

#### a. Include the raincatcher-gps module in the main app dependencies

Run `npm install fh-wfm-gps --save` to install the raincatcher-gps module and automatically add it to the dependencies in the `package.json` file.

In the `src/app.js` file, the `fh-wfm-gps` module is included in the main angular dependencies.

#### b. Create a new service to publish and subscribe to location events

The raincatcher-mobile app is responsible for managing the location based messages passing between the raincatcher-gps module and raincatcher-user module.

A new file, `src/app/location/user-location-service.js`, file is added to create a service that will be instantiated when the app is loaded.

This will create the `UserLocationService` that will be instantiated by AngularJS.

This service will periodically publish the `wfm:gps:location:update` topic that is subscribed to by the `raincatcher-gps` module.

Whenever the `done:wfm:gps:location:update` topic is published, a check is made to see if a user is logged in. If this is the case, then the `wfm:user:location:update` topic is published.

The `wfm:user:location:update` topic is subscribed to by the new functionality in the `raincatcher-user` module. See the [next](#update-the-raincatcher-user-module-to-propagate-user-location-updates-to-the-cloud) section for details on the updates to the `raincatcher-user` module.

```javascript
module.exports = 'wfm-mobile.location';
//The config values are obtained from the src/app/config.js file.
var config = require('../config');

angular.module('wfm-mobile.location', ['wfm.core.mediator']).service('UserLocationService', ['mediator', 'gps',  UserLocationService]);

/**
 * This service is responsible for subscribing to location updates from the raincatcher-gps module.
 *
 * These updates are then propigated to the raincatcher-user module to update the users current locaion.
 *
 * @param mediator
 * @constructor
 */
function UserLocationService(mediator) {
  var self = this;

  /**
   *
   * Getting the logged in user profile. This will be used to get the current user ID.
   *
   * @returns {object/null}
   */
  function retrieveProfileData() {
    var json = localStorage.getItem('fh.wfm.profileData');
    return json ? JSON.parse(json) : null;
  }

  //Subscribing to the `done` state for the `wfm:gps:location:update` topic.
  //This will update the users current location.
  mediator.subscribe('done:wfm:gps:location:update', function(location) {
    var coordinates = location.coords;

    var localUserData = retrieveProfileData();

    //The user may not be logged in yet.
    if(localUserData) {

      //Having a new location, we can publish the `wfm:user:location:update` topic for the raincatcher-user module.
      //The raincatcher-user module will propagate the updated location to the cloud.
      mediator.publish('wfm:user:location:update', localUserData.id, {
        latitude: coordinates.latitude,
        longitude: coordinates.longitude
      });
    }
  });

  self.interval = setInterval(function() {
    mediator.publish('wfm:gps:location:update');
  }, config.locationPollingInterval);

  //When the user logs out of the app, the polling process can stop.
  mediator.subscribe('wfm:user:logout', function() {
    clearInterval(self.interval);
  });
}
```

The `config.locationPollingInterval` value (in ms) is obtained from the main app config which is defined in the `src/app/config.js` file.

```javascript
{
  ...
  locationPollingInterval: 2000
  ...
}
```

#### c. Add the new location service as a dependency of the wfm-mobile.auth module

In the `src/app/auth/auth.js` file, add the `wfm-mobile.location` service as a dependency of the `wfm-mobile.auth` module. This is to ensure that the `UserLocationService` is instantiated when the `LoginCtrl` controller is loaded.

```javascript
angular.module('wfm-mobile.auth', [
  'ui.router',
  'wfm.core.mediator',
  //Loading the wfm-mobile.location module to allow polling of the mobile user location.
  'wfm-mobile.location'
])
```

```javascript
...
.controller('LoginCtrl', ['userClient', 'hasSession', 'UserLocationService', function(userClient, hasSession) {
...
```

### 2. Update the raincatcher-user module to propagate user location updates to the Cloud

In the previous step, we added functionality to the raincatcher-demo-mobile app to subscribe to an updated location published by the `raincatcher-gps` module and publish a `wfm:user:location:update` topic with the user ID and updated location.

To assign this updated value to the user and push the updated value to the cloud, something has to subscribe to the user position update topic. This functionality could be built into the `raincatcher-demo-mobile`, `raincatcher-demo-portal` and `raincatcher-demo-auth` applications directly without changing any of the modules.

However, updating a user location would be a useful feature to have in the raincatcher-user module. It would mean any applications that consume the raincatcher-user module could update the user location through whichever means they wish. It is not bound in any way to the `raincatcher-gps` module.


The update the raincatcher-user module, the following changes are made:

#### a. Update the UserClient to subscribe to the user location update topic.

In the `lib/user/user-client.js` file, the constructor for the `UserClient` is updated to subscribe to the user location update topic


```javascript
...
//Subscribing to the user location update topic.
//This subscriber will push the updated location to the cloud back end.
this.mediator.subscribe('wfm:user:location:update', function(userId, location) {
    self.updateLocation(userId, location);
});
...
```

The `updateLocation` function is also added to the `UserClient` to create the HTTP request to the cloud side of the raincatcher-user module.

```javascript
/**
 *
 * Pushing an updated user location, in latitude and longitude, to the cloud backend.
 *
 * @param {string} userId    The ID of the user to update
 * @param {object} location  The updated location to save
 * @param {number} location.latitude  The latitude of the location to update
 * @param {number} location.longitude The longitude of the location to update
 */
UserClient.prototype.updateLocation = function(userId, location) {
  return this.initPromise.then(function() {
    xhr({
      path: config.apiPath + '/' + userId + "/location",
      method: 'put',
      data: location
    })
  });
};
```

#### b. Update the backend cloud router with the new location update endpoint

In the Raincatcher solution, user authentication is provided by the `raincatcher-demo-auth` application. (See the [architecture guide](Demo-Solution-Architecture.md) for an illustration of the demo solution.)

Update the `lib/user/user-router.js` file to include a new endpoint for handling the location update requests from the `UserClient` in the previous requests.

```javascript
//Sending the update request to the mbaas handler in the raincatcher-auth service. This is where the user data is stored.
router.route('/:id/location').put( function(req, res, next) {
    //The delegate will handle sending the update request to the raincatcher-demo-auth application running elsewhere.
    //The raincatcher-demo-auth application is responsible for storing user details and authentication.
    //Therefore, the location update request must pass to this app.
    delegate.updateLocation(req.params.id, _.pick(req.body, 'longitude', 'latitude')).then(function(updatedUser) {
      res.json(updatedUser);
    }, next);
});
```

Also add the `updateLocation` function to the `delegate` handling sending the request to the `raincatcher-demo-auth` app.

```javascript
/**
 *
 * Function to proxy location update requests to the mbaas service endpoint for handling user location update requests.
 *
 * @param {string} userId    The ID of the user to update
 * @param {object} location  The updated location to save
 * @param {number} location.latitude  The latitude of the location to update
 * @param {number} location.longitude The longitude of the location to update
 */
Delegate.prototype.updateLocation = function(userId, location) {
  return this.xhr({
    path: '/api/wfm/user/' + userId + "/location",
    method: 'PUT',
    params: {
      location: location
    }
  });
};
```

#### c. Update the MBaaS service endpoint to handle proxied requests from the backend cloud app

In the previous step, we update the backend cloud app to handle requests from the `UserClient` in Step 1.

We must now update the MBaaS service endpoint to handle the proxied request from Step 2.

In the `lib/router/mbaas.js` file, add the handler for the location update http request.

```javascript
//Route for updating the location for a single user in the mbaas service.
router.route('/:id/location').put(function(req, res) {
var userId = req.params.id;
var locationToUpdate = req.body.location;

//Only interested in the location update for this single user
mediator.once('done:wfm:user:location:update:' + userId, function(saveduser) {
    //Returning the updated user to the cloud request.
    res.json(saveduser);
});

mediator.publish('wfm:user:location:update', userId, locationToUpdate);
});
```


### 3. Update the raincatcher-demo-auth app to handle location update topics

In the demo solution, the management of saved users is the responsibility of the `raincatcher-demo-auth` app.

The `lib/user.js` module sets up subscribers to the CRUD operations that affect the current set of users. Therefore, it is the the place where the subscriber to the `wfm:user:location:update` topic should reside.


```javascript
//Update a user location
var topicUpdateLocation = 'wfm:user:location:update';
mediator.subscribe(topicUpdateLocation, function(userId, location) {
    //The setTimeout is included to indicate that the update of a user can be an asynchronous process.
    //E.g. the users are saved to a mongodb database instead of in-memory.
    setTimeout(function() {
      var index = _.findIndex(users, function(_user) {
        return _user.id === userId;
      });
      users[index].location = location;
      mediator.publish('done:' + topicUpdateLocation + ':' + userId, users[index]);
    }, 0);
});
```


### 4. Update the raincatcher-map module to handle displaying user locations on the map

In the steps above, we have completed the full process of capturing user locations on the mobile application and propagating them to the cloud and MBaaS service for storage.

If storage of user locations was the only motivation for this change, then this guide would be complete.

However, the goal of this guide is to also allow portal users to view the location of workers on a map.
 
The [raincatcher-map](https://github.com/feedhenry-raincatcher/raincatcher-map) module is responsible for the functionality related to displaying map content.

The functionality for displaying map content is located in the `lib/angular/directive.js` file.

To allow the inclusion of worker location tags, the following steps are taken:

#### a. Add the worker scope to the directive

To allow the map directive to have access to the workers from the `raincatcher-demo-portal` application, the isolated scope of the directive must include the `workers` parameter. For more information on isolated scopes, see the AngularJS [documentation](https://docs.angularjs.org/guide/directive).

```javascript
{
 ...
    scope: {
      list: '=',
      center: '=',
      workorders: '=',
      //Added to display worker locations in addition to work orders.
      workers: '=',
      containerSelector: '@'
    },
  ...
}
```


#### b. Add the addUserMarkers function

The `addUserMarkers` function to display user location markers.

```javascript
/**
* Function for adding worker markers to the map in addition to work order markers.
* @param {object} map     - The map to add worker markers to.
* @param {Array}  workers - An array of user object describing the workers.
*/
function addUserMarkers(map, workers) {
    //If there are no workers, then there is no need to add any worker markers on the map.
    if(!workers) {
      return;
    }
    
    workers.forEach(function(worker) {
    
      //There is no guarantee that a worker will have a location. (e.g. a new worker has been added but has never logged into a mobile app before.)
      if (worker.location) {
        var lat = worker.location.latitude;
        var long = worker.location.longitude;
        var marker = new google.maps.Marker({map: map,position: new google.maps.LatLng(lat, long)});
        //Using a different color marker to easily separate workers from work orders
        marker.setIcon('http://maps.google.com/mapfiles/ms/icons/green-dot.png');
    
        //Displaying the username and ID when the marker is clicked
        var infowindow = new google.maps.InfoWindow({content:'<strong>Worker #'+worker.id+'</strong><br>'+worker.name+'<br>'});
    
        //Whenever the marker is clicked, display the user data added above.
        google.maps.event.addListener(marker, 'click', function() {
          infowindow.open(map,marker);
        });
      }
    });
}
```


### 5. Update the raincatcher-demo-portal application to pass users to the raincatcher-map scope

Having prepared the `raincatcher-map` module to display user locations, use the following steps to update the raincatcher-demo-portal application to allow the display of worker locations.


#### a. Update the portal map module config to resolve workers

In the `src/app/map/map.js` file, the config for the `app.map` module already has a resolver for the work orders.

We need to add another resolver for the *workers* to ensure a list of workers is available to the `mapController`.

```javascript
{
  ...
  resolve : {
    ...
    //Getting a list of workers
    //This will allow adding worker locations to the map if available.
    //Using the userClient from the raincatcher-user module to list the users.
    workers: function(userClient) {
      return userClient.list();
    }
    ...
  
  }
  ...
}
```

#### b. Add the workers parameter to the map directive

In the map template, the scope parameters are defined in the `workorder-map` directive defined by the `raincatcher-map` module. This is related to step 4.a where the isolated scope parameter list was updated.

The update is made to the `src/app/map/map.tpm.html` template file.
 
```html
<!-- Using the raincatcher-map module directive to render the map in the portal --->
<workorder-map workorders="ctrl.workorders" center="ctrl.center" workers="ctrl.workers" container-selector="#content"></workorder-map>
```