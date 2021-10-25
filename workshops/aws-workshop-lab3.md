summary: Enhance IT Operations with SLOs, SLIs and AIOps!
id: aws-workshop-lab3
categories: dt
tags: aws-workshop
status: Published 
authors: Rob Jahn
Feedback Link: mailto:alliances@dynatrace.com
Analytics Account: UA-175467274-1

# 3. Modernize workloads for the cloud #3 - Operate better

## Objectives of this Lab 
In order to do more with less and scale, organizations must transcend IT silos, foster collaboration and improve productivity. Automation and a common data model are key components of this, but it takes platforms that support operational teams and workflows.

Objectives of this Lab

🔷 Examine Dynatrace Service Level Objectives (SLOs)

🔷 Create a custom dashboard with SLOs

🔷 Use the SLO dashboard, combined with DAVIS AI to help you in Intelligent and automated observability

<!-- -->
## Service Level Objectives

Positive
: **Definition**: the level that you expect a service to achieve most of the time and against which an SLI is measured.

Example: ***"Service responses shall be faster than 400 milliseconds (ms) for 95% of all valid requests measured over 14 days."***

Dynatrace provides all the necessary real-time information that your Site-Reliability Engineering (SRE) teams need to monitor their defined objectives.

An SRE team is responsible for finding good service-level indicators (SLIs) for a given service in order to closely monitor the reliable delivery of that service. This is important when running workloads on a highly distributed and hyper-scale infrastructure like AWS.

Even more important is then to align development and operations teams around a single agreed-to objective. This helps to reduce the natural tension that exists between their objectives — creating and iterating products (development) and maintaining system integrity (operations).

In short, a well-defined SLO can help teams make data-driven operational decisions that increase development velocity without sacrificing stability.

SLIs can differ from one service to the other, as not all services are equally critical in terms of time and error constraints. Dynatrace offers more than 2000 different metrics that are ready for use as dedicated SLIs.

And using the available metrics, each SLO definition can be evaluated by following two result metrics:

* **SLO status:** The current evaluation result of the SLO, expressed as a percentage. The semantics of this percentage (for example, 99.3% of all service requests are successful, or 99.99% of all website users are “satisfied” in terms of Apdex rating) and the target defined for this percentage are up to the SRE team.

* **SLO error budget:** The remaining buffer until the defined SLO target is considered as failed. For example, if an SLO defines a 95% target and its current SLO status is evaluated as 98%, the remaining error budget is the difference between the SLO status and the SLO target.
Two SLOs were created for you, so review those.

Here is an example custom dashboard with SLO dashboard tiles.

![image](assets/aws-workshop/lab2-slo-dashboard.png)

### Review your environment

From the left menu in Dynatrace, click the `SLO` option to review the two SLOs that are already setup.  Edit one of them to review the configuration.

![image](assets/aws-workshop/lab2-slo-list.png)

### 👍 How this helps

You can review the current health status, error budgets, target and warning, along with the timeframe of all your SLOs on the SLOs overview page.

Davis provides quick notifications on anomalies detected, along with actionable root causes. If your SLO has turned red, this is most likely because Davis has already raised a problem for the underlying metrics, showing you the root cause.

<!-- -->
## Create a SRE inspired dashboard

### Select pre-created Dashboard

1. From the left side menu in Dynatrace, pick the **dashboard** menu.
2. On the dashboard page, select the **Cloud Migration Success** dashboard.

![image](assets/aws-workshop/lab2-dashboard-view.png)

### Edit Dashboard

Now you need to edit the dashboard and adjust the tiles with the SLOs and databases in your environment.

On the top right of the page, click the **edit** button and then follow these steps:

### Edit Dynamic requests tile

1. Click on the title of the Dynamic requests tile to open the Service properties window on the right side 
2. On the Service properties window, pick the monolith **frontend (monolith-frontend)** service
3. Click the **Done** button

![image](assets/aws-workshop/lab2-dashboard-edit-tile.png)

### Edit remaining tiles

1. Repeat the same steps above for the Cloud services tile, but pick the **frontend (dev-frontend)** in the Service properties window
2. Repeat for the two SLO tiles, but pick the associated SLO from the drop down list in the SLO properties window
3. Repeat for the two database tiles. For Cloud services application there are 3 databases, so just pick one of the database of a demo.
4. Click the **Done** button to save the dashboard

<!-- -->
## Deploy new patch for monolith

As part of day to day mainteance, you are tasked to deploy a patch for the monolith application.

From the AWS CLoudShell, run these commands to set the backend service to version 2

```bash
cd ~/aws-modernization-dt-orders-setup/learner-scripts/
./set-version.sh backend 2
```

### View app in browser
----------------------

The event has the URL back to the sample application, so just click that if you don't have the sample app up already. You should see `version 2` for the customer app now too.

![image](assets/aws-workshop/lab3-app-backend-version-2.png)

<!-- -->
## Dynatrace events

Before we get to the problems, let's review <a href="https://www.dynatrace.com/support/help/dynatrace-api/environment-api/events/post-event/" target="_blank">Dynatrace
information events</a>.

Dynatrace information events enable continuous delivery tools to provide additional details for Dynatrace. Here is an example of two informational events for deployment and performance testing being be sent to Dynatrace.

![image](assets/aws-workshop/lab3-pipeline.png)

There are several Dynatrace information event types:
- CUSTOM_ANNOTATION
- CUSTOM_CONFIGURATION
- CUSTOM_DEPLOYMENT
- CUSTOM_INFO
- **MARKED_FOR_TERMINATION**

Below we can see both the **CUSTOM_INFO** (\#1) and **CUSTOM_DEPLOYMENT** events (\#2) for a service that was the root cause to a problem the Dynatrace AI engine, Davis®, determined.

![image](assets/aws-workshop/lab3-problemcard-events.png)

Each event has a timestamp, event source, a few standardized fields (depending on the event type), and the option to add additional custom fields. See <a href="https://www.dynatrace.com/support/help/shortlink/api-events-post-event#parameters-mapping" target="_blank">this table</a> for details. Here are two example events.

***NOTE*** the URL back to the pipeline making the change.

![image](assets/aws-workshop/lab3-event-examples.png)

### 👍 How this helps

Having information events speeds up triage by adding context to what's happening with the application. Just imagine getting alerted about an issue and immediately seeing a load test or deployment took place, and in one click of the event, URL review the system, job, and team responsible!

### Dynatrace tags

When creating information events, you must target the entities to receive the event so that it's only associated with the relevant component. That is where Dynatrace tags come in.

In a nutshell, tags in Dynatrace are labels or markers used for organizing entities in large monitoring environments. Below shows a few tags for my catalog service: **stage:staging** and **service:catalog-service**

![image](assets/aws-workshop/lab3-tags.png)

### API call Example

Here is an example of a typical events API call. You do not need to executed this. It is an example and it has already been automated for you in the deployment scripts

```
    curl -X POST \
      https://mySampleEnv.live.dynatrace.com/api/v1/events \
      -H 'Authorization: Api-token abcdefjhij1234567890' \
      -H 'Content-Type: application/json' \  
      -d '{
      "eventType" : "CUSTOM_DEPLOYMENT",
      "source" : "Pipeline Script" ,
      "deploymentName" : "Set order-service to version 1",
      "deploymentVersion" : "1"  ,
      "deploymentProject" : "dt-orders project" ,
      "ciBackLink" : "http://link-back-to-my-pipeline",
      "customProperties": {
          "Example Custom Property 1" : "Example: Commit SHA",
          "Example Custom Property 2" : "Example: Name of person who run pipeline",
          "Example Custom Property 3" : "Example: Application owner name",
          "Example Custom Property 4" : "Example: Ticket Number approving change"
      },
      "attachRules": {
                "tagRule" : [
                    {
                      "meTypes":["PROCESS_GROUP_INSTANCE"],
                      "tags": [
                          {
                                "context": "CONTEXTLESS",
                                "key": "service",
                                  "value": "order-service"
                          },
                              {
                                "context": "CONTEXTLESS",
                                "key": "project",
                                  "value": "dt-orders"
                          },
                              {
                                "context": "CONTEXTLESS",
                                "key": "stage",
                                  "value": "production"
                          }
                ]}
              ]}
            }
    }
```

<!-- -->
## What did I break?

Let DAVIS® tell you what broke. The problem may take a minute to show up, but this is what the problem will look like once it does. Also, you may see two problems that eventually get merged into one as Dynatrace is performing the problem analysis.

1.  Impact Summary - Multiple services affected
2.  Root cause

![image](assets/aws-workshop/lab3-backend-problem.png)

### Analyze problem - top findings
---------------------------------

Click on the **Analyze Response Time Degradation** button to view the real issue with the request. To open the top findings page.

Here you can see how Dynatrace automatically analyzes the problem and lets you know whether the problem is related to code, waiting, or other services and queues.

Click in the **active wait time** line with the top findings to open the execution time breakdown detail.

![image](assets/aws-workshop/lab3-backend-analysis.png)

### Analyze problem - execution time breakdown
---------------------------------------------

Dynatrace automatically show the breakdown of the execution time. To see more, click the **View method hotspots** button.

![image](assets/aws-workshop/lab3-backend-hotspots.png)

### Analyze problem - hot spots
------------------------------

Here the code call breakdown is shown and by expanding this tree, you can locate the method where the slow down is occurring.

**NOTE: You will need to expand several stack frames to get to method causing the slow down.**

![image](assets/aws-workshop/lab3-backend-analysis-trace.png)

### Analyze problem impact
-------------------------

From the breadcrumb menu, click on the **backend** to open the service page.

![image](assets/aws-workshop/lab3-backend-breadcrumb.png)

Then click on the response time box to open the service details page. You can see exactly when the problem started.

![image](assets/aws-workshop/lab3-backend-problem-details.png)

<!-- -->
## Recover the service

You'll need to recover the service first before more people get impacted. Run these commands to set the backend to version 1.

```bash
cd ~/aws-modernization-dt-orders-setup/learner-scripts/
./set-version.sh backend 1
```
<!-- -->
## Why was the monolith SLO not affected?

![image](assets/aws-workshop/lab3-backend-problem-usecase.png)

<!-- -->
## Why does the problem card say 'Custom Threshold'?
The Dynatrace AI engine is evaluating metrics and dependencies for daily and weekly traffic patterns. Since we just setup our sample application, there is not a lot of history to review we setup a fixed threshold as a global service setting.

To review this setting, on the left side menu, click **settings**, click **Anomaly Detection** and the the **Services** setting.

![image](assets/aws-workshop/lab3-global-anomoly-rules.png)

<!-- -->
## Summary

In this section, you should have completed the following:

✅ Examine Dynatrace Service Level Objectives (SLOs)

✅ Create a custom dashboard with SLOs 

✅ Learn how DAVIS® helps you operater better via AI-assisted Operations