<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Running The Demo Raincatcher Solution Locally](#running-the-demo-raincatcher-solution-locally)
  - [Overview](#overview)
  - [Requirements](#requirements)
    - [MongoDB](#mongodb)
    - [Ruby (Optional)](#ruby-optional)
  - [Clone All Of The Raincatcher Modules and Demo Apps](#clone-all-of-the-raincatcher-modules-and-demo-apps)
  - [Notes On Running Locally](#notes-on-running-locally)
    - [Using Drag & Drop Apps Locally](#using-drag-&-drop-apps-locally)
    - [Using A Local Service To Authenticate Users](#using-a-local-service-to-authenticate-users)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Running The Demo Raincatcher Solution Locally

## Overview

This guide presents the steps necessary to:

- Run the Raincatcher demo solution locally.
- Edit Raincatcher modules and have the results reflected in the demo solution.

## Requirements

There are some requirements to getting up and running locally with Raincatcher

### MongoDB

The *raincatcher-demo-cloud* app and *raincatcher-demo-auth* app requires a running MongoDB instance running. See the [MongoDB Installation Guide](https://docs.mongodb.com/manual/installation/) for details on how to install MongoDB.

### Ruby (Optional)

If you want to use the [Raincatcher CLI Tool](https://github.com/feedhenry-raincatcher/raincatcher-cli) to manage starting all of the demo apps, then you will need to install [Ruby](https://www.ruby-lang.org/en/documentation/installation/).


## Clone All Of The Raincatcher Modules and Demo Apps

The [Raincatcher CLI Tool](https://github.com/feedhenry-raincatcher/raincatcher-cli) is a useful tool for getting set up with all of the Raincatcher modules and Demo Apps. Follow the steps in the README file to get set up for local development.

## Notes On Running Locally

Some Raincatcher modules require features in the Red Hat Mobile Application Platform (RHMAP) and therefore require access to the RHMAP platform.

### Using Drag & Drop Apps Locally

The *raincatcher-appforms* module requires that the containing cloud app has access to a running MBaaS. This allows the local *raincatcher-demo-cloud* app to access

- Forms
- Themes
- Drag & Drop App Config

See [this guide](http://docs.feedhenry.com/v3/guides/create_a_forms_project_single_theme.html) to for more Drag & Drop Apps feature detail.

If you have access to the RHMAP Platform, you can use an existing Cloud App environment variables to populate environment variables from the Cloud App in a Forms Project.

These environment variables can be placed in the `raincatcher-demo-cloud/Gruntfile.js` file in the Demo Cloud App.

```javascript
{
  ...
  env: {
    local: {
      FH_APPNAME: "<FH_APPNAME value>",
      FH_DOMAIN: "<FH_DOMAIN value>",
      FH_ENV: "<FH_ENV value>",
      FH_INSTANCE: "<FH_INSTANCE value>",
      FH_MBAAS_ENV_ACCESS_KEY: "<FH_MBAAS_ENV_ACCESS_KEY value>",
      FH_MBAAS_HOST: "<FH_MBAAS_HOST value>",
      FH_MBAAS_PROTOCOL: "<FH_MBAAS_PROTOCOL value>",
      FH_WIDGET: "<FH_WIDGET value>",
      FH_APP_API_KEY: "<FH_APP_API_KEY value>",
    }
  }
  ...
}
```


### Using A Local Service To Authenticate Users

The Raincatcher Demo solution uses a RHMAP Service [raincatcher-demo-auth](https://github.com/feedhenry-raincatcher/raincatcher-demo-auth) to perform user authentication functionality. The [raincatcher-demo-cloud](https://github.com/feedhenry-raincatcher/raincatcher-demo-cloud) App uses the $fh.service Cloud API to make requests to the *raincatcher-demo-auth* service.
 
 The *raincatcher-demo-auth* service can be run locally for ease of editing. In order for the *raincatcher-demo-auth* Cloud App to contact the service, the `WFM_AUTH_GUID` environment variable needs to be set in the `raincatcher-demo-cloud/Gruntfile.js` file.
  
  The value can be set to the service GUID in the `FH_SERVICE_MAP` environment variable.
