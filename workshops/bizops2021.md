id: biz-ops
summary: BizOps powered by Davis AI
categories: biz-ops, business-analytics
tags: biz-ops, Intermediate
status: Published 
Feedback Link: mailto:APAC-SE-Central@dynatrace.com
Analytics Account: UA-175467274-1

# BizOps with Dynatrace
<!-- ------------------------ -->
## Overview
Duration: 15

This Repo contains the labs we are going to work though as part of the BizOps – Bridging the Gap to the Business Hands On Workshop.

For the purposes of the Hands-On, we will automate and make the steps seamless for the participants.

### Prerequisites
- Access to a Dynatrace tenant/environment

### Lab Setup
The following steps are used for this lab:
- Sample Application 
  * Sample App is based on [Easy Travel](https://confluence.dynatrace.com/community/display/DL/easyTravel)
  * There are 2 interfaces running for Easy Travel
    - Classic UI
	![Intro](assets/bizops2021/ETClassic.png)
	- Angular UI
	![Intro](assets/bizops2021/ETAngular.png)

### What You’ll Learn
- Bring business context to IT metrics
  - Identify individual users for session analysis
  - Conversion goals
  - Session Replay
- Visualize the data and enhance collaboration between Ops and Line-of-business teams
- Action on the data
- Learn Dynatrace Capabilities such as
  * Session Replay
  * User Session Properties
  * User Session Query Language (USQL)
  * Dynatrace Dashboards

<!-- ------------------------ -->
## Activity-1 - Bring business context to IT metrics
Duration: 15

The goal of this exercise is to enhance the basic Real User Monitoring configuration in Dynatrace to bring business context to IT metrics. As such, we will not be going through the basic configuration of Dynatrace and have pre-setup the environment with the configuration:
* Organizing applications
* Enable JavaScript framework support​
* Logical grouping of user actions (configure User Action Naming​)

If you are interested in the basic setup, the following lab reference will be a good guide [Digital Experience Management with Dynatrace](https://dynatrace-apac.github.io/workshops/dem/index.html?index=..%2F..index#0)

## Activity-1a - Identify individual users for session analysis

One of the key features of Dynatrace Real User Monitoring is the ability to uniquely identify individual users across different browsers, devices, and user sessions.

By default, Dynatrace assigns a unique, random ID to each new user. You can however assign more meaningful custom user tags that are comprised of, for example, user names or email addresses.

In this exercise, we will use the approach of tagging users based on ***page source***. This approach to user tagging works by capturing available data in your application’s page source.

The Easy Travel Angular app displays the user name at the top right hand corner. This is done via the text of a DOM element or a JavaScript variable. 

To configure this in Dynatrace,

1. Select Applications from the left navigation menu
2. Select the **EasyTravel Angular** application
3. Click the Browse button **(...)** and select **Edit**
4. Under the **Capturing** header, look for **User tag**
   ![User Tag](assets/bizops2021/task1a-usertag.gif)
5. Click **Add user tag rule**
   - Source type: **CSS Selector**
   - CSS Selector field:

     ```
     a.greeting
     ```

   - Toggle Apply cleanup rule **on**
   - In the Regex box, copy the following

     ```
     Hi, (.*+)
     ```

6. Click on **Add user tag rule** button
7. Remember to click on **Save changes** button

Once the configuration is complete, this is how it should look like:
![User Tag](assets/bizops2021/task1a-usertag-complete.png)

Positive
: Reference document: [Identify Individual Users for Session Analysis](https://www.dynatrace.com/support/help/how-to-use-dynatrace/real-user-monitoring/how-to-use-real-user-monitoring/cross-application-user-session-analytics/identify-individual-users-for-session-analysis/)


<!-- ------------------------ -->
## Activity-1b - Creating Conversion Rates
Duration: 15

Conversion goals are useful for understanding if users are meeting critical milestones in a user journey, for example, successful checkouts, newsletter signups, or demo signups

In this exercise, we will create conversion goals for the Easy Travel Angular application.

1. In the same screen as the previous activity, scroll down to **Session Replay and behavior** heading, and select **Conversion Goals**
   ![Conversion](assets/bizops2021/task1b-conversion.gif)
2. Click **Add goal** button and enter the following:
   - Name:

     ```
     Credit card validated
     ```

   - Type of goal: **User Action**  
   - Rule applies to: **XHR actions**  
   - Rule
     - **Page URL**
	 - **contains**

       ```
       easytravel/rest/validate-creditcard
       ```

   - The completed configuration look like this
     ![Conversion](assets/bizops2021/task1b-conversion-complete.png)

### Validating conversion goal setting

Check results aginst the application: **EasyTravel Angular** > **User behavior**  

![Conversion](assets/bizops2021/tast1b-checkconversiongoal.png)

<!-- ------------------------ -->
## Activity-1c - Session and User action Properties
Duration: 15

In this exercise, we will create session and user action properties to expose additional data to Dynatrace. This is useful for bringing additonal context regarding the users (e.g. campaign source, various items selected by users etc.) for analytics purposes.

Dynatrace has pre-defined a list of popular software that uses tags to give context to the sessions. Some of them are
- Google Analytics
- Adobe Analytics
- Tealeaf 
- Etc.

These are under the **Property pack** tab.

For additional data sources that can be extracted from your application, use **custom defined properties** to define string, numeric, and date properties for your monitored user actions and user sessions.

Property values are then captured as part of each of your users’ journeys. Property values can be leveraged for unrivaled visibility into all the details of your users’ interactions with your application.

1. In the same screen as the previous activity, go to the **Capturing** heading, and select **Session and action properties**
   ![Session properties](assets/bizops2021/tast1c-sessionproperties.png)
2. Click on the button **Add properties**

### Property packs - Google Analytics/Adobe/UTM codes etc.
Since these are pre-defined, all we need to do is to select from the list. In this section, we will select the following:
- Click on the drop down, select **web properties**
- Under **Configure properties**, select the corresponding **Add** button for the following:
  - UTM Source
  - UTM campaign
  - UTM term
  ![Web property pack](assets/bizops2021/task1c-webprop-complete1.png)
- Click on **Next**
- Expand each of the items and toggle the **store as user action property**
  ![Web property pack](assets/bizops2021/task1c-webprop-complete2.png)
- Click on **Create properties**

### Custom properties - Transaction amount of each user
- Select the **Custom defined property** tab
- Select the Expression type: **Server side request attribute**
- Request attribute name: **Revenue**
- Display name:

  ```
  Booking
  ```

- (auto fill) Key: **booking**
- Storage type:
  - Store as **session** property
- Click **"Save property"**

Here is how the completed configuration will look like:
![Custom property](assets/bizops2021/task1c-custprop-booking-complete.png)

### Custom properties - Browsed travel package amount
Following the above configuration, continue to add a CSS Selector type of custom properties
- Expression type: **CSS Selector**
- Data type: **Double**
- CSS Selector

  ```
  #summary > div:nth-child(5) > p
  ```

- Display name:

  ```
  Trip Cost
  ```

- (auto fill) Key: **tripcost**
- Storage type:
  - Store as **session** property
- Click **"Save property"**

Here is how the completed configuration will look like:
![Custom property](assets/bizops2021/task1c-custprop-tripcost-complete.png)

### Custom properties - Travel destination browsed/booked
- Expression type: **Server side request attribute**
- Request attribute name: **Destination**
- Display name:

  ```
  Destination
  ```

- (auto fill) Key: **destination**
- Storage type:
  - Store as **session** property
  - Store as **user action** property
- Click **"Save property"**

Here is how the completed configuration will look like:
![Custom property](assets/bizops2021/task1c-custprop-dest-complete.png)

### Completed configuration screen
Once everything is configured, you should have this list of session/useraction properties:
![Session properties](assets/bizops2021/task1c-properties-complete.png)

Positive 
: Reference documents - [Session Properties Help page](https://www.dynatrace.com/support/help/shortlink/user-session-properties) and [Request Attribute Help page](https://www.dynatrace.com/support/help/shortlink/request-attributes)

<!-- ------------------------ -->
## Activity-1d - Session Replay
Duration: 15

In this exercise, we will cover configuring Session Replay in Dynatrace.

Follow the steps below to enable session Replay:

1. In the same screen as the previous activity, scroll down to **Session Replay and behavior** heading, and select **Session Replay**
2. Toggle ON **Enable Session Replay**
   ![SessionReplay](assets/bizops2021/task1d-sr1.png)
3. Scroll down to **Recoding mask settings** tab, select **Mask user input**
   ![SessionReplay](assets/bizops2021/task1d-sr2.png)
4. Click on the **Playback masking settings** tab, select **Mask user input**
   ![SessionReplay](assets/bizops2021/task1d-sr3.png)
5. Finally, scroll all the way down down and Click on **Save**
   ![SessionReplay](assets/bizops2021/task1d-sr4.png)

Positive
: Reference document: [Session Replay](https://www.dynatrace.com/support/help/how-to-use-dynatrace/real-user-monitoring/basic-concepts/session-replay/)

<!-- ------------------------ -->
## Activity-2 - Visualize the data

We are now ready to visualize the data collected in Dynatrace. In this exercise, you can see how **conversion goals**, **session/action properties** can provide much more insights and context to the IT metrics for analysis.

This is the dashboard that we will aim to create.

![Dashboard](assets/bizops2021/task2-Dashboard-complete.png)

<!-- ------------------------ -->
## Activity-2a - Visualize Payment funnel

Positive
: Goal: to visulize a user journey when user is making a payament on your site

![Dashboard](assets/bizops2021/task2-PaymentFunnel-tile.png)

1. Navigate to **Dashboards** on the left hand naviation menu
2. Click on the buton **Create Dashboard** and enter a name and click on **Create**
3. Drag a **User Sessions Query** tile and click on **Configure tile**
   ![Dashboard](assets/bizops2021/task2-CreateDashboard.gif)
4. Enter the following query:

   ```SQL
   SELECT FUNNEL(useraction.name like "*journeys*" AS "Journey Search", useraction.name = "click on book now (xhr: /easytravel/rest/journeys/)" AS "Click on Book Now", useraction.name = "click on sign in (xhr: /easytravel/rest/login)" AS "Login", useraction.name="click on book journey  (xhr: /easytravel/rest/validate-creditcard)" AS "Submit Payment") FROM usersession
   ```

5. Click on the **run query** button
6. Rename this to

   ```
   Payment funnel
   ```

7. Click on the **save changes to dashboard** button

![Dashboard](assets/bizops2021/task2-PaymentFunnel-usql.png)

<!-- ------------------------ -->
## Activity-2b - Visualize Business KPI metrics

Positive
: Goal - to visulize some critical metrics to indicate health of business

### Conversion over time

Positive
: Purpose - Conversions over time help to track the trend in meeting conversion goals. Very low conversion rates indicate that your customers are facing friction in completing the customer journey.

![Dashboard](assets/bizops2021/task2-Conversions-tile.png)

1. While having the Dashboard in **Edit** mode, click on the previously created tile and click on the **clone** button, then click on the **configure tile** button
   ![Dashboard](assets/bizops2021/task2-CloneDashboard.gif)
2. Enter the following query:

   ```SQL
   select datetime(starttime, "E HH:mm", "10m"), count(*) as "Conversions" from usersession where useraction.matchingConversionGoals="Credit card validated" group by datetime(starttime,"E HH:mm","10m")
   ```

3. Click on the **run query** button
4. Select **Line chart**
5. Rename this to

   ```
   Conversions over time
   ```

6. Click on the **save changes to dashboard** button

![Dashboard](assets/bizops2021/task2-Conversions-usql.png)

### Booking Revenue

Positive
: Purpose - Since Easy Travel is a travel booking site, the main business KPI is to ensure a steady stream of booking revenue. Depending on your "business" drivers, these KPIs can range be a wide range.

![Dashboard](assets/bizops2021/task2-Booking-tile.png)

1. Clone the tile from above and click on **configure tile**
2. Modify the query:

   ```SQL
   select sum (doubleProperties.booking) as Revenue from usersession
   ```

3. Click on the **run query** button
4. It will automatically select a **Single value**
5. Rename this to

   ```
   Booking revenue
   ```

6. Click on the **save changes to dashboard** button

![Dashboard](assets/bizops2021/task2-Booking-usql.png)

### Abandoned cart totals

Positive
: Purpose - Often, the "basket value" of a virtual "shopping cart" are potentially the amount that your visitors to your site will transact. However, for whatever reason, like a payment failure or an invalid promo code, will cause your visitors to abandon the entire transaction or change the items in the "shopping cart". Tracking this value and also the list of users affected will give you an indication if your visitors are dropping out before "checkout" or "purchase" and arrest the problem before it impacts the business.

![Dashboard](assets/bizops2021/task2-Abandoned-tile.png)

1. Clone the tile from above and click on **configure tile**
2. Modify the query:

   ```SQL
   SELECT sum (doubleProperties.tripcost) as "Revenue Lost" from usersession where useraction.matchingConversionGoals IS NULL AND doubleProperties.tripcost > 0
   ```

3. Click on the **run query** button
4. It will automatically select **Single value**
5. Rename this to

   ```
   Abandoned cart value
   ```

6. Click on the **save changes to dashboard** button

![Dashboard](assets/bizops2021/task2-Abandoned-usql.png)

### Users affected by an abandoned cart

![Dashboard](assets/bizops2021/task2-AbandonedUsers-tile.png)

1. Clone the tile from above and click on **configure tile**
2. Modify the query:

   ```SQL
   SELECT userid from usersession where useraction.name = "click on book journey  (xhr: /easytravel/rest/validate-creditcard)" and doubleProperties.booking is null
   ```

3. Click on the **run query** button
4. Select **Table only**
5. Rename this to

   ```
   Users affected by abandoned cart
   ```

6. Click on the **save changes to dashboard** button

![Dashboard](assets/bizops2021/task2-AbandonedUsers-usql.png)

<!-- ------------------------ -->

## Activity-3 - Action on the data
Duration: 15

Let's challange DAVIS™️!

Access the EasyTravel Angular actual booking application
- Open Dashboards
- Click on the **...Welcome to the Dynatrace Workshop 🔬** Dashboard
- Click on the **Booking Portals:✈ EasyTravel Angular** link

![Action on the data](assets/bizops2021/task3-prework1.gif)

- Book a travel package
- You can use userid **alex** or **peter**
- What do you notice?
- Try clicking on **(3) Payment** multiple times
- Before you close your browser, do the following:
  - Open your browser's **developer tools**

![Action on the data](assets/bizops2021/task3-prework2.png)

- Go to **Console tab**
- Type the following:

  ```
  dtrum.endSession()
  ```

![Action on the data](assets/bizops2021/task3-prework3.png)

### Scenario #1
**Conversions** are dropping! And so is **Revenue**!

![Scenario1](assets/bizops2021/task3-scenario1-1.png)

However, the monitoring dashboards of IT Operations team show all systems are **green**!
![Scenario1](assets/bizops2021/task3-scenario1-2.png)

Even if it is not an IT issue, something must be causing vistors to be **frustrated** and not completing the transactions. 

Negative
: Use Dynatrace to determine what is causing friction to your visitors.

***Hint:***

Some dashboards in the **Sample BizDevOps Dashboard** might give a clue to what is happening.

![Scenario1](assets/bizops2021/task3-scenario1-3.png)

Also, investigating some user sessions together with **Session Replay** might uncover some facts about your application that cannot be detected by logs or investigating application code.

### Scenario #2
DAVIS has detected an Anomaly! You also have observed that there are users who are affected and have abandoned their booking process!

![Scenario2](assets/bizops2021/task3-scenario2-1.png)

Negative
: Will you be faster than DAVIS in determining the root cause?

***Hint:***

The dashboard that you created just now would have contain all the information for you to understand what is causing errors.

You can start with the **Problems** tile and drill down from there.

You can also investigate each individual user session by using the tiles that list the user names who are facing difficulties.

<!-- ------------------------ -->
## Activity-4 - Putting it together (BizDevOps use cases)

By applying Business Analytics in Dynatrace, we could easily achieve the following goals:

![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase.png)

<!-- ------------------------ -->
## Activity-4a - Create a "friction-less" user experience by reducing errors
Duration: 15

The #1 cause of drop-outs/user frustration is errors!

Errors can come from various sources:
- Server-side applications - business logic, coding errors etc.
- Client-side applications (browser/mobile app) - JavaScript errors, AJAX calls, pop-up screens etc.
- 3rd-party - CDNs, external providers like Facebook, Twitter etc.

More so, DDoS attacks and cross-site scripting attacks that cause [Content Security Policy (CSP) violations](https://www.dynatrace.com/news/blog/automatically-detect-potential-frontend-attacks-that-cause-content-security-policy-csp-violations/
) can make your website unresponsive or steal data and personally identifiable information of your users.

Prioritizing what needs to be fixed is usually a challenging task as there are many types of errors and analyzing them one-by-one will not be efficient.

![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-errors1.png)

Positive
: Goal of this activity is to help you in: (1) Increasing Conversion & Customer Engagement, (2) Optimization, and (3) Operational Quality & Reporting

This workflow will benefit the following personas
- UX designers/developers
- Customer Experience Lead
- Web programmers/developers

A few entry points to start the analysis:
   - Overview of all the errors in a particular application 
   - Errors happening in a specific user action
   - Errors happening to a specific group of user sessions
   - Dashboard tiles
   - etc.

We will use 2 workflows as examples.

### Workflow #1 - Navigate from an overview of a specific application
1. Go to Applications -> Easy Travel Angular -> Errors
2. Click **Analyze errors**
3. You will see a small handful of errors, select one which shows **HTTP 500** under the **HTTP request** tabs
4. You can modify the **dectection rules**
5. One of the rules cover detection of **CSP errors**
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-errors-CSP.png)

### Workflow #2 - Navigate from User Sessions
1. Got to **User Sessions**
2. Use the filters above
3. Use USQL for a more granular manipulation of the data setting

### Reference
- [Automatically detect potential frontend attacks that cause Content Security Policy (CSP) violations](https://www.dynatrace.com/news/blog/automatically-detect-potential-frontend-attacks-that-cause-content-security-policy-csp-violations/)
- [Ensure unrivaled customer experience with Davis® AI-powered HTTP and custom error analytics](https://www.dynatrace.com/news/blog/extended-davis-awareness-of-http-and-custom-errors/)
- [Abandoned Shopping Cart – Minimizing errors, maximizing revenue](https://www.dynatrace.com/news/blog/abandoned-shopping-cart-minimizing-errors-maximizing-revenue/)
- [How to detect impacting 3rd Party API calls with Dynatrace Real User Monitoring (RUM)](https://www.dynatrace.com/news/blog/how-to-detect-impacting-3rd-party-api-calls-with-dynatrace-real-user-monitoring/)
- [Optimize your marketing campaign investment by leveraging BizDevOps](https://www.dynatrace.com/news/blog/optimize-your-marketing-campaign-investment-by-leveraging-bizdevops/)
- [Improve user experience with more visibility into CDN-related HTTP errors](https://www.dynatrace.com/news/blog/improve-user-experience-with-more-visibility-into-cdn-related-http-errors-part-1/)

<!-- ------------------------ -->
## Activity-4b - Peformance optimization to improve usability
Duration: 15

Google introduced their [Core Web Vitals](https://web.dev/vitals/) initiative last year.

Goal is to **standarize** on the measurements to provide "quality signals that are essential to delivering a great user experience on the web".

Google recently [included these metrics as "user experience signals" in their search result rankings](https://developers.google.com/search/blog/2020/11/timing-for-page-experience), demonstrating that these metrics aren’t just nice to have but are a necessity.

Dynatrace complements Google Core Web Vital measurements with insights across your application front and back ends. With zero configuration, you can leverage all three Core Web Vital metrics in Dynatrace and get details with just a few clicks.

### Page and Page groups analysis workflow
1. Start with the Application overview page (go to Applications and select an application name).
2. The Application overview page displays a **Top 3 pages** section, which includes tabs for Page groups and Pages. Choose the tab that displays the granularity you want.
   - Select a group Name to go directly to the Page group overview for the selected group.
   - Select View all page groups to display Multidimensional analysis for all page groups.

Positive
: ***Tip!*** Multidimensional analysis offers a particularly useful overview of all pages or page groups, with answers to questions such as: Which are the most viewed?, Which are the most visited?, Which are busy pages, where lots of actions happen?, Which contain the most errors?, Which are slow on their initial page load?, Which are slow on a route change?

### Analysis workflow
1. Select **View all page groups**
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti1.png)
2. In the **Performance metric** drop down, select **Largest contenful paint**. Leave the rest as defaults.
3. Click on **home**
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti2.png)
4. Scroll down to the **Performance** section and click **Perform waterfall analysis**
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti3.png)
5. The waterfall analysis allows you to analyze the underlying actions of the route change for these pages. Here you can put detailed performance and error information into context.

![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti4.png)

### Dashboarding workflow

1. Use the breadcrumbs on the top of the screen to go back one screen, i.e. Applications > EasyTravel Angular > Page groups > home
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti5.png)
2. Scroll down to the **Performance** section and click **Analyze performance**
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti6.png)
3. Scroll down to the **Detailed analysis...** section
4. Select **UTM source** from the "Analyze by" drop-down.
5. Select either **Largest contenful paint** or **First contentful paint** from the "Performance metric" drop-down.
6. Click **Create metric**

![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti7.png)

7. Enable **Split by UTM source**, and enter the value **5**.
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti8.png)
8. Click **Create metric** to complete the metric creation.
9. You can chart this on a dashboard now.

### Reference
- [Core Web Vitals: Practical metrics for optimal user experiences](https://www.dynatrace.com/news/blog/core-web-vitals-practical-metrics-for-optimal-user-experiences/)
- [Full support for Google’s Core Web Vitals improves your user experience and search rankings](https://www.dynatrace.com/news/blog/full-support-for-googles-core-web-vitals-improves-your-user-experience-and-search-rankings/)
- [Optimize modern web applications with automatic insights into pages and page groups](https://www.dynatrace.com/news/blog/optimize-modern-web-applications-with-automatic-insights-into-pages-and-page-groups/)
- [Business Insights extends support for optimizing Core Web Vitals](https://www.dynatrace.com/news/blog/business-insights-for-core-web-vitals/)


<!-- ------------------------ -->
## How to get CSS selectors?

1. Assess the EasyTravel Angular homepage
   ![CSS](assets/bizops2021/ETAngular.png)
2. Select any travel package, click on **book** and login with username: **alex** and password: **alex** and click **Sign in**
   ![CSS](assets/bizops2021/ETAngular-signin.png)
3. Right click on the name **Alex Elliot** and click **inspect**
4. Click on the **...** on the highlighted row and select **Copy** > **Copy Selector**
   ![CSS](assets/bizops2021/ETAngular-css.gif)
5. You can do this for any DOM element that has been defined in your web application.

<!-- ------------------------ -->
## How to make use of request attributes?
Duration: 15

As we have shown earlier, 2 request attributes **revenue** and **destination** have been pre-created for you.

### Defining new request attributes
You can create request attributes from various data sources and in this part of the hands on, we will expose the **loyalty status** of customers who sign up for travel packages. 

![RequestAttribute](assets/bizops2021/RequestAttributeSource.png)

1. Go to Settings > Server side service monitoring > Under **Deep monitoring**, Request Attributes
2. Click on the button **Define a new request attribute**
   ![RequestAttribute](assets/bizops2021/RequestAttributePreConfig.png)
3. Fill in the **Request attribute name** as `loyalty status`
4. Leave the data type as **text**
5. Click on the button **Add new data source**
   ![RequestAttribute](assets/bizops2021/RequestAttributeSetup1.png)
6. Configure 1st source:
   - Request attribute source: **Web request URL query parameter**
   - Parameter name: **loyalty**
   - Remember to click on **Save**
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup2.png)
7. Click on the button **Add new data source**
8. Configure 2nd source:
   - Request attribute source: **Java method parameter**
   - Click on the button **Select method sources**
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup3.png)
   - Select the process group: **com.dynatrace.easytravel.business.backend.jar easytravel**
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup4.png)
   - Click on **Continue** button
   - Enter this in the search box and click on **Search**

     ```
     com.dynatrace.easytravel.business.webservice.BookingService
	 ```
   - Select the class and click **Continue**
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup5.png)
   - Click on the radio button beside **Use the selected class** and click **Continue**
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup6.png)
   - Select the method: **private void checkLoyaltyStatus**
   - Click on the **Finish** button	
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup7.png)
   - In the Capture dropdown, select **2: java.lang.String**
   - Click **Save**
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup8.png)
9. Once done, the final configuration will look like the screen below
10. Please remember to click on the final **Save** button on the **top right hand**

![RequestAttribute](assets/bizops2021/RequestAttributeFinal.png)
   
### Adding the request attribute in Session Properties
1. Select Applications from the left navigation menu
2. Select the **EasyTravel Angular** application
3. Click the Browse button **(...)** and select **Edit**
4. Under the **Capturing** header, look for **Session and action properties**
5. Click on the button **Add properties**
6. Select the **Custom defined property** tab
7. Select the Expression type: **Server side request attribute**
8. Request attribute name: **loyalty status**
- Display name:

  ```
  Member Status
  ```
- (auto fill) Key: **memberstatus**
- Storage type:
  - Store as **session** property
- Click **"Save property"**

### Visualizing the data via USQL
1. Follow the Activity-2 steps on adding a **USQL tile** on a dashboard.
2. Use the following USQL, and chose a **pie chart**

   ```SQL
   select stringProperties.memberstatus, sum (doubleProperties.booking) as Revenue from usersession where stringProperties.memberstatus IS NOT NULL group by stringProperties.memberstatus order by sum(doubleProperties.booking) DESC
   ```
3. You will get the following tile

![RequestAttribute](assets/bizops2021/RequestAttributeChart.png)


<!-- ------------------------ -->

## Feedback

We hope you enjoyed this lab and found it useful. We would love your feedback!
<form>
  <name>How was your overall experience with this lab?</name>
  <input value="Excellent" />
  <input value="Good" />
  <input value="Average" />
  <input value="Fair" />
  <input value="Poor" />
</form>

<form>
  <name>What did you benefit most from this lab?</name>
  <input value="Learning Digital Business Analytics" />
  <input value="User Session Properties setup" />
  <input value="How to use Dynatrace to improve BizDevOps colloabration" />
  <input value="Using USQL" />
</form>

<form>
  <name>How likely are you to recommend this lab to a friend or colleague?</name>
  <input value="Very Likely" />
  <input value="Moderately Likely" />
  <input value="Neither Likely nor unlikely" />
  <input value="Moderately Unlikely" />
  <input value="Very Unlikely" />
</form>

Positive
: 💡 For other ideas and suggestions, please **[reach out via email](mailto:APAC-SE-Central@dynatrace.com?subject=BizOps - Ideas and Suggestions")**.