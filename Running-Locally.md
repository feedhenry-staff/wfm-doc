<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Running The Demo RainCatcher Solution Locally](#running-the-demo-raincatcher-solution-locally)
  - [Overview](#overview)
  - [Requirements](#requirements)
    - [MongoDB](#mongodb)
    - [RainCatcher CLI Tool (Optional)](#raincatcher-cli-tool-optional)
  - [RHMAP Access if Running Locally](#rhmap-access-if-running-locally)
    - [Using Drag & Drop Apps Locally](#using-drag--drop-apps-locally)
    - [Using A Local Service To Authenticate Users](#using-a-local-service-to-authenticate-users)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# Running The Demo RainCatcher Solution Locally

## Overview

This guide describes the steps necessary to:

- Run the Raincatcher demo solution locally.
- Edit Raincatcher modules and have the results reflected in the demo solution.

## Requirements

To run RainCatcher locally, requirements must be met

### MongoDB

Both the *raincatcher-demo-cloud* app and *raincatcher-demo-auth* app require a running instance of MongoDB. To install MongoDB, refer to the [MongoDB Installation Guide](https://docs.mongodb.com/manual/installation/).

### RainCatcher CLI Tool (Optional)

The [RainCatcher CLI Tool](https://github.com/feedhenry-raincatcher/raincatcher-cli) is a useful tool for setting up all of the Raincatcher modules and demo apps. It also allows for the management of all demo apps, for exmaple: all demo apps can be started using this tool. If you want to use this tool:

1. Install [Ruby](https://www.ruby-lang.org/en/documentation/installation/).

2. Follow the steps in the README file to setup a local development environment.

## RHMAP Access if Running Locally

A number of RainCatcher modules require access to features from the Red Hat Mobile Application Platform (RHMAP) and therefore require access to RHMAP.

### Using Drag & Drop Apps Locally

The *raincatcher-appforms* module requires the containing cloud app to have access to a running RHMAP MBaaS. This allows the local *raincatcher-demo-cloud* app to access

- Forms
- Themes
- Drag & Drop App Config

For more information about the Drag & Drop App, refer to this [guide](http://docs.feedhenry.com/v3/guides/create_a_forms_project_single_theme.html).

If you have access to RHMAP, you can use existing Cloud App environment variables to populate environment variables from the Cloud App in a Forms Project.

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

The RainCatcher Demo solution uses the RHMAP Service [raincatcher-demo-auth](https://github.com/feedhenry-raincatcher/raincatcher-demo-auth) to perform user authentication functionality. The [raincatcher-demo-cloud](https://github.com/feedhenry-raincatcher/raincatcher-demo-cloud) App uses the $fh.service Cloud API to make requests to the *raincatcher-demo-auth* service.

The *raincatcher-demo-auth* service can be run locally for ease of editing. In order for the *raincatcher-demo-auth* Cloud App to contact the service, the `WFM_AUTH_GUID` environment variable needs to be set in the `raincatcher-demo-cloud/Gruntfile.js` file. The value can be set to the service GUID in the `FH_SERVICE_MAP` environment variable.
