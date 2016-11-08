<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Getting Started with Raincatcher](#getting-started-with-raincatcher)
  - [Overview](#overview)
  - [Raincatcher Structure](#raincatcher-structure)
    - [Raincatcher Modules](#raincatcher-modules)
  - [Running the Raincatcher Demo Apps In RHMAP](#running-the-raincatcher-demo-apps-in-rhmap)
    - [Create and Configure the Raincatcher Demo Project Using the Project Templates](#create-and-configure-the-raincatcher-demo-project-using-the-project-templates)
  - [Running The Raincatcher Apps Locally](#running-the-raincatcher-apps-locally)
  - [Integrating A New Module Into The Demo Solution](#integrating-a-new-module-into-the-demo-solution)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Getting Started with Raincatcher

## Overview
A Field Workforce Management (WFM) application connects a business' back-office with its fleet of mobilized employees.  FeedHenry Raincatcher leverages commoditized mobile hardware, making use of each employee's mobile phone.

## Raincatcher Structure
Raincatcher consists of a set of reusable modules. A demo application is provided as a project template that showcases how these modules can be assembled into an application. See [this guide](Demo-Solution-Architecture.md) for details on the Demo Solution.

### Raincatcher Modules
Raincatcher modules are packaged and distributed via [npm](https://www.npmjs.com/).  They are designed to be included in your application using [browserify](http://browserify.org/).  Raincatcher modules export one or more of:

* Angular.js directives or services providing client-side functionality for both the mobile and portal clients.
* Express.js routes providing a REST API to be consumed by the client-side portions of the module.
* FeedHenry sync configurations for enabling data synchronization of a module's data.

Refer to the READMEs of the respective modules for details on their purpose and consumption.

## Running the Raincatcher Demo Apps In RHMAP
RHMAP provides a set of Raincatcher project templates to help you get the Raincatcher demo apps up and running.

### Create and Configure the Raincatcher Demo Project Using the Project Templates

1. Create a blank Forms theme.
  1. Navigate to **Drag & Drop Apps -> Forms Themes**.
    <br><img src="assets/images/select-forms-themes.png" title="Select Forms Themes" alt="Select Forms Themes" height="400px">

  2. Click **New Theme**.
  3. Select the **Red Hat Theme**.
  4. Name the theme, for example **wfm**.
  5. Click **Create**.

2. Create a new project using the **WFM Demo Project** template.
  1. Select the **Projects** header menu item.
  2. Click **New project**.
  3. Select the **WFM Demo Project** template (under **Tech Preview**).
  4. Name the project, for example **wfm-demo**.
  5. Click **Create**.
  6. Wait until the project gets created, then click **Finish** at the bottom of the page.
    <br><img src="assets/images/new-project.png" title="New Project" alt="New Project" height="400px">

3. Associate the **Forms theme** with the **WFM Demo Project**
  1. Select the **Projects** header menu item
  2. Open the **wfm-demo** project that you have created
  3. Select **Forms** from the project menu
  4. Select the **wfm** theme you have created in the **Project  Theme** drop-down
  5. Click on **Save**

4. Create a new MBaaS Service using the **WFM Auth Service** template.
  1. Select the **Services & APIs** header menu item.
  2. Click **Provision MBaaS Service/API**.
  3. Select the **WFM Auth Service** template (under Authentication).
  4. Name the service, for example **wfm-auth**.
  5. Click **Next**.
    <br><img src="assets/images/auth-service.png" title="Auth Service" alt="Auth Service" height="400px">
 
  6. Click **Finish**.
  7. Select the environment for initial deployment.
  8. On the **Details** page of the MBaaS service, select **Deploy** from the left-hand side menu.
  9. Click **Deploy Cloud App**.

5. Create an auth policy using this new MBaaS service.
  1. Navigate to **Admin -> Auth Policies**.
    <br><img src="assets/images/auth-policy.png" title="Auth Policy" alt="Auth Policy" height="400px">

  2. Click **Create**.
  3. Name the policy : eg. **wfm**.
  4. Select the **MBaaS Service** type.
  5. Select the `wfm-auth` service.
  6. Enter `/api/wfm/user/auth` as the endpoint.
  7. Select the default environment.
  8. Validate the settings using the user name `trever` and password `123`.

    * The response JSON should have the `status` property with value `ok`.

  9. Click **Create Auth Policy**.

6. Associate the **WFM Auth Service** MBaaS service with the project.
  1. Select the **Services & APIs** header menu item.
  2. Select the **WFM Auth Service** created in step 3.
  3. Scroll down to the **Service Settings**, **Access Control**; select the project created above; eg. **wfm**.
    <br><img src="assets/images/associate-service.png" title="Associate Service" alt="Associate Service" height="400px">

  4. Click the **Save Service** button.
  5. From the left-hand side menu, select **Environment Variables**.
  6. Compare the `FH_SERVICE_AUTHORISED_PROJECTS` in the *App* section to the one in the *System* section. If they differ, click the **Push Environment Variables** button.
    <br><img src="assets/images/service-env-vars.png" title="Service Env Vars" alt="Service Env Vars" height="400px">

7. Copy the project ID from the MBaaS service, and set it as the `WFM_AUTH_GUID`.
  1. From the left-hand side menu, select **Details**.
  2. Click the **Copy** button next to the **Service ID field**.
  <br><img src="assets/images/copy-service-id.png" title="Copy Service ID" alt="Copy Service ID" height="400px">

  3. Select the **Projects** header menu item.
  4. Select the **wfm-demo** project created in step 2.
  5. Select the cloud app.
  6. From the left-hand side menu, select **Environment Variables**.
  7. Click **Add Variable**.
    <br><img src="assets/images/project-add-env-var.png" title="Project Add Env Var" alt="Project Add Env Var" height="400px">

  8. Enter `WFM_AUTH_GUID` for the **name**, and paste in the **Service ID** copied above for the **value**.
  9. Click **Push Environment Variables**.

8. Set the auth policy name as the `WFM_AUTH_POLICY_ID`.
  1. Select the **Projects** header menu item.
  2. Select the **wfm-demo** project.
  3. Select the cloud app.
  4. From the left-hand side menu, select **Environment Variables**.
  5. Click **Add Variable**.
  6. Enter `WFM_AUTH_POLICY_ID` for the **name**, and set the auth policy name created in step 5 for the **value**.
  7. Click **Push Environment Variables**.

9. Finally, check that the auth service, the cloud app, and the portal app are all deployed and started.
  1. For each of the above mentioned apps, select **Deploy** from the left-hand side menu.
  2. Ensure that the most recent deployment has a **result** of **Success**.
  <br><img src="assets/images/deploy-status.png" title="Deploy Status" alt="Deploy Status" height="400px">

  3. If it does not indicate success, click **Deploy Cloud App** and verify the next deployment is successful.

The apps are now created, configured, and deployed.

## Running The Raincatcher Apps Locally

See the [Running The Demo Raincatcher Solution Locally](Running-locally.md) guide for getting set up locally for Raincatcher development.

## Integrating A New Module Into The Demo Solution

The [Module Integration](Module-Integration.md) guide explains how a new module would be introduced to the overall solution to provide extra functionality.
