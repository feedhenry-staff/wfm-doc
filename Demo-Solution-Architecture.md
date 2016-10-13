<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [The Raincatcher Demo Solution Architecture](#the-raincatcher-demo-solution-architecture)
  - [Overview](#overview)
  - [What Is A Raincatcher Module?](#what-is-a-raincatcher-module)
    - [Raincatcher Mediator](#raincatcher-mediator)
      - [Mediator Pattern Namespacing](#mediator-pattern-namespacing)
    - [Raincatcher Module User Interface](#raincatcher-module-user-interface)
    - [Bridging The Mediator Events Between Client And Cloud](#bridging-the-mediator-events-between-client-and-cloud)
  - [Demo Solution Architecture](#demo-solution-architecture)
    - [Overview](#overview-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# The Raincatcher Demo Solution Architecture

## Overview

This guide presents an introduction to the Demo solution available for the Raincatcher project. This solution is intended to demonstrate how the Raincatcher module ecosystem can combine to produce a functional Workforce Management solution.

The Raincatcher Demo solution is intended to be a reference implementation of a Workforce management solution. This solution contains modules that are related specifically to Red Hat Mobile Application Platform features, and is therefore currently only functional with access to a RHMAP platform instance.

Some examples of RHMAP plaform specific modules include:

- The [raincatcher-appform](https://github.com/feedhenry-raincatcher/raincatcher-appform) module uses APIs specific to the Drag & Drop Apps feature of the RHMAP platform.
- The [raincatcher-push](https://github.com/feedhenry-raincatcher/raincatcher-push) module uses the Aerogear Push API to send push notifications.
- The [raincatcher-sync](https://github.com/feedhenry-raincatcher/raincatcher-sync) module uses Client and Cloud APIs specific to the RHMAP Sync framework.
- The [raincatcher-user](https://github.com/feedhenry-raincatcher/raincatcher-user) modules uses RHMAP Auth Policies to validate users.

This guide will:
 
- Present the concept of a Raincatcher module.
- Present the architecture of the Demo solution, highlighting how the Demo solution combines different Raincatcher modules.
- Illustrate how the functionality of the Demo solution can be expanded by adding an additional Raincatcher module.

## What Is A Raincatcher Module?

Before describing the Demo solution, it is necessary to introduce the concept of a Raincatcher module.

The Raincatcher project is based on the principle of multiple, loosely-coupled modules communicating through the mediator pattern. Raincatcher modules make use of the [mediator pattern](https://addyosmani.com/largescalejavascript/) to enable loose coupling between the Raincatcher modules and their consuming applications / modules.

There are several important concepts that the Raincatcher modules exploit to work correctly:

- The Raincatcher Mediator
- The Raincatcher Module User Interface
- Bridging The Mediator Events Between Client And Cloud 

To illustrate these concepts, there is a smaller tutorial solution that incorporates:

- [A simple Raincatcher module](https://github.com/feedhenry-raincatcher/raincatcher-tutorial-module).
- [A simple Client App incorporating the Raincatcher module](https://github.com/feedhenry-raincatcher/raincatcher-tutorial-client)
- [A simple Cloud App incorporating the Raincatcher module](https://github.com/feedhenry-raincatcher/raincatcher-tutorial-cloud)

You can clone these repositories and run them locally to see the Raincatcher module bridging functionality. It is recommended that you become familiar with this simpler solution to get a good understanding of Raincatcher concepts before attempting to 

### Raincatcher Mediator

In the Raincatcher project, the [Raincatcher mediator](https://github.com/feedhenry-raincatcher/raincatcher-mediator/blob/master/README.md) module is a module implementing the mediator pattern. All Raincatcher modules must publish and subscribe to the same mediator in order to communicate.

In the next section, the naming conventions for how Raincatcher modules communicate are described.

#### Mediator Pattern Namespacing

In the Raincatcher project, all modules can subscribe and publish messages to the mediator. In order to differentiate between mediator events between topics, it is necessary to namespace the messages that are published.

Topics in Raincatcher are defined as text fields separated by a *:*. E.g. "wfm:user:list". Raincatcher topics are prefixed by *wfm* by convention.

Topics can have a state prefix that defines the result of a topic.

- *done* : The `done` state defines a topic that has executed successfully. E.g. "done:wfm:user:list"
- *error* : The `error` state defines a topic that has encountered an error when executing. E.g. "error:wfm:user:list"

### Raincatcher Module User Interface

Raincatcher modules use the [AngularJS 1](https://angularjs.org/) framework to enable Raincatcher modules to have their own user interface. Having this separation of UI and business logic allows the modules to maintain their loose coupling.

Raincatcher modules define the UI functionality in the `angular` folder in each module. Each module is exported as an AngularJS module (e.g. "wfm.user" is the AngularJS module name for the [Raincatcher User](https://github.com/feedhenry-raincatcher/raincatcher-user/blob/master/lib/angular/user-ng.js) module.)

Currently, AngularJS 1 is the only UI framework implemented into the Raincatcher modules.

### Bridging The Mediator Events Between Client And Cloud

One of the main concepts in the Raincatcher modules is the bridging of mediator events between the Client and Cloud in the same module. By doing this, it allows client topics to propagate to the Cloud and vice versa. Any other dependent modules on the cloud can then subscribe to the event for further processing on the cloud side.

![Illustration Of Bridging Between Client and Cloud In Raincatcher Modules](assets/images/mobile-bridging.png)

In the illustration above, we can see that the Client and Cloud apps have a very similar structure. The same Raincatcher modules are added as dependencies in both the Client and Cloud apps. The key here is that the Raincatcher modules are capable of propagating topics from Client to Cloud and vice versa. By doing this, all of code required for both client and cloud functionality is contained in the same module.

From the point of view of the application developer, the mechanism for bridging the client/cloud divide is abstracted by the Raincatcher module. 

*Note:* Not all Raincatcher modules must have topic propagation between client and cloud. For example, the [raincatcher-camera](https://github.com/feedhenry-raincatcher/raincatcher-camera) module only has a client side component. However, there is nothing stopping a developer expanding this module to automatically forward taken photos to a server if they wish.

As an example: 

 - The [raincatcher-user](https://github.com/feedhenry-raincatcher/raincatcher-user) module uses HTTP requests to send topics to the cloud endpoints. This is only a client-cloud communication.
 - The [raincatcher-workorder](https://github.com/feedhenry-raincatcher/raincatcher-workorder) module uses the [raincatcher-sync](https://github.com/feedhenry-raincatcher/raincatcher-sync) module. The *raincatcher-sync* module provides a mechanism to have two-way client-cloud topic propigation.

## Demo Solution Architecture

### Overview

The Demo Raincatcher Solution is based on all of the concepts above. A 

The Demo Raincatcher Solution is comprised of 4 applications: 

- The [Raincatcher Portal App](https://github.com/feedhenry-raincatcher/raincatcher-demo-portal).
- The [Raincatcher Mobile App](https://github.com/feedhenry-raincatcher/raincatcher-demo-mobile).
- The [Raincatcher Cloud App](https://github.com/feedhenry-raincatcher/raincatcher-demo-cloud).
- The [Raincatcher Authentication App](https://github.com/feedhenry-raincatcher/raincatcher-demo-auth).

![Full Raincatcher Demo Solution Architecture](assets/images/full-solution-diagram.png)

