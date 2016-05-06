# Getting started wtih WFM

## Overview
A Field Workforce Management (WFM) application connects a business' back-office with it's fleet of mobilized employees.  FeedHenry WFM leverages commoditized mobile hardware, making use of each employee's mobile phone.

## WFM structure
WFM 2 consists of a set of re-usable modules.  A demo application is provided that showcases how these modules can be assembled into an application.

### WFM Modules
WFM modules are packaged and distributed via [npm](https://www.npmjs.com/).  They are best included in your application using [browserify](http://browserify.org/).  WFM modules will export one or more of:
1. Angular.js directives or services providing client-side functionality for both the mobile and portal clients.
2. Express.js routes providing a REST API to be consumed by the client-side portions of the module.
3. FeedHenry sync configurations for seamlessly enabling data-sync of a module's data

Refer to the READMEs of the respective modules for details on their purpose and consumption.

#### Loose coupling
WFM modules make use of the [mediator pattern](https://addyosmani.com/largescalejavascript/) to enable loose coupling between the WFM modules and their consuming applications.  Refer to the WFM mediator README for usage instructions and API details.

### WFM Demo apps
The demo applications run on Red Hat Application Platform (RHMAP).  The demo project consists of:

1. **wfm-portal**: a sample back-office application.  This is where work is collected and pushed to the workers out in the field.
2. **wfm-modile**: is the mobile application running on the field workers mobile devices.
3. **wfm-cloud**: the cloud applications provides the gateway into the FeedHenry mBaas.
4. **wfm-auth**: an mBaaS service that integrates with the FeedHenry authentication system.

## Running the WFM demo apps
The FeedHenry platform provides a set of WFM project template to help you get the WFM demo apps up and running.

### Create and configure the WFM demo project using the project templates
Steps:

1. Create a blank appform theme
  * Navigate to `Drag & Drop Apps` -> `Forms Themes`
    <br><img src="assets/images/select-forms-themes.png" title="Select Forms Themes" alt="Select Forms Themes" height="400px">
  * Click `New Theme`
  * Select the `Base Template`
  * Name the theme: eg. *wfm*
  * Click `Create`

1. Create a new project using the `WFM demo` project template
  * Select the `Projects` header menu item
  * Click `New project`
  * Select the `WFM Demo Project` template (under *Tech Preview*)
  * Name the project. eg: *wfm-demo*
  * Click `Create`
  * Wait, then click `Finish` (at the bottom of the page)
    <br><img src="assets/images/new-project.png" title="New Project" alt="New Project" height="400px">

1. Create a new mBaaS Service using the wfm-auth service template
  * Select the `Services & APIs` header menu item
  * Click `Provision MBaaS Service/API`
  * Select the `WFM Auth Service` template (under Authentication)
  * Name the service; eg. *wfm-auth*
  * Click `Next`
    <br><img src="assets/images/auth-service.png" title="Auth Service" alt="Auth Service" height="400px">
  * Select the environment for initial deployment
  * Click `Next`
  * Click `Finish`
  * On the wfm-auth page, select `Deploy` from them left-hand menu
  * Click `Deploy Cloud App`

1. Create an auth policy using this new mBaaS service
  * Navigate to `Admin` -> `Auth Policies`
    <br><img src="assets/images/new-project.png" title="New Project" alt="New Project" height="400px">
  * Click `Create`
  * Name the policy; eg. *wfm-auth-policy*
  * Select the `MBaaS Service` type
  * Select the `wfm-auth` service
  * Input `/api/wfm/user/auth` as the endpoint
  * Select the default environment
  * Validate the settings using the username *trever* and password *123*
    * The response JSON should have the `status` property with value *ok*
  * Click `Create Auth Policy`

1. Associate the wfm-auth mBaaS service with the project
  * Select the `Services & APIs` header menu item
  * Select the Service created above; eg. *wfm-auth*
  * Scroll down to the *Service Settings*, *Access Control*; select the project created above; eg. *wfm*
    <br><img src="assets/images/associate-service.png" title="Associate Service" alt="Associate Service" height="400px">
  * Click the `Save Service` button
  * From the L.H.S. menu, select `Environment Variables`
  * Compare the `FH_SERVICE_AUTHORISED_PROJECTS` in the App to the one in the system, if they differ, click the `Push Environment Variables` button
    <br><img src="assets/images/service-env-vars.png" title="Service Env Vars" alt="Service Env Vars" height="400px">

1. Copy the project id from the mBaaS service, and set it as the WFM_AUTH_GUID
  * From the L.H.S. menu, select `Details`
  * Click the `Copy` button next to the `Service ID field`
  <br><img src="assets/images/copy-service-id.png" title="Copy Service ID" alt="Copy Service ID" height="400px">
  * Select the Projects` header menu item
  * Select the project created above; eg. *wfm*
  * Select the cloud app
  * From the L.H.S. menu, select `Environment Variables`
  * Click the `Add Variable` button
    <br><img src="assets/images/project-add-env-var.png" title="Project Add Env Var" alt="Project Add Env Var" height="400px">
  * Type in `WFM_AUTH_GUID` for the *name*, and paste in the *Service ID* copied above for the *value*
  * Click the `Push Environment Variables` button


1. Finally, check that the auth service, the cloud app, and the portal app are all deployed and started
  * For each of the above mentioned apps, select `Deploy` from the L.H.S. menu
  * Ensure that the most recent deployment has a *result* of `Success`
  <br><img src="assets/images/deploy-status.png" title="Deploy Status" alt="Deploy Status" height="400px">
  * If it does not indicate success, click the `Deploy Cloud App` and verify the next deployment is successful

The apps are now created, configured, and deployed.  Next let's create some app forms to use with the workflows.

### Creating appforms
In this step we will create a couple of appforms for use with the WFM workflows.  See the wfm-appform README for a list of field types currently supported by appforms

Steps:

1. Navigate to ...

1. Create a new form, for example:
  * Name: signoff
  * Field 1: Text field
    * name: "Name"
    * code: "name"
    * required: "true"
  ...

1. Create another form, for example:
  * Name: identification
  * ...

1. Deploy the forms

1. Associate the forms with the project

### Embed the forms in a WFM workflows using the WFM portal app
Now that we have created some appforms, let's embed them in a WFM workflow.

1. Navigate to  the portal app

1. Click the *open in new window* button

1. log in as *daisy* with password *123*

1. Navigate to Forms, verify that the forms we created are present and listed

1. Navigate to workflows

1. Select the workflow *signoff*

1. ...

### Using the mobile app
Now that we have configured a workflow using our appforms, let's push a workorder through the workflow.

1. Navigate to eh mobile-app in the RHMAP studio

2. login as *trever*, password: *123*

3. From the list of workorders select workorder ...

4. ...

## Other WFM use cases to try

1. Create a new workflow using static HTML defined forms

1. Sending a message from the portal to the client

1. Create a new user, assign them to a group, assign workorders to him

1. Schedule workorders using the scheduler
