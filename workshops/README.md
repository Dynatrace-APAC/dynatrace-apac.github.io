Use this format below in the beginnig of the of the MD file

- Summary: Short summary of your tutorial. Text is used within overview page
- ID: The ID will define the folder that the tutorial contents will be generated and put into
- Categories: The first item in the categories list defines the "icon" that you will attached to your lab in the tutorial overview screen. You can add up to 3 items / icons. From the end of the list, use categories which you want the lab to appear in. Options are : infrastructure-monitoring, application-microservices-monitoring, cloud-automation, digital-experience, business-analytics
- Tags: Other searchable elements from the lab. End the list with level - (Beginner, Intermediate, Advanced)
- Status: set it to Draft or Published (doesn't have any effect right now)
- Authors: Please list yourself as an author
- Feedback link: either the tutorial repostory or any other link you want to provide 

Example

``` bash
id: my-first-tutorial-unique-id     
summary: Summary on the learning lab
categories: dynatrace, kubernetes, application-microservices-monitoring  
tags: kubernetes, beginner 
status: Draft 
authors: Brandon Neo 
Feedback Link: mailto:APAC-SE-Central@dynatrace.com 
Analytics Account: UA-175467274-1

```

<!-- name of your lab -->
# My first tutorial

<!-- heading starting with ## will create a new section. Also include the estimated duration for this section to provide some guidance for the user -->
## Welcome
Duration: 10:00

Take a look at the metadata above.
- Summary: Short summary of your tutorial.
- ID: The ID will define the folder that the tutorial contents will be generated and put into
- Categories: The first item in the categories list defines the "icon" that you will attached to your tutorial in the tutorial overview screen. The other items in this list are used along with the first item to allow for filtering the tutorials.
- Tags: please set the Keptn version that your tutorial is written for, e.g., keptn07x (for Keptn 0.7.x releases)
- Status: set it to Draft or Published (doesn't have any effect right now)
- Authors: Please list yourself as an author
- Feedback link: either the tutorial repostory or any other link you want to provide 

In this tutorial we are going to ...

<!-- subheadline -->
### What you'll learn

- item 1
- item 2
- item 3

Positive
: We can also make use of highlight sections in green or orange.

Negative
: We can also make use of highlight sections in green or orange.

### Some other headline

- with your bullet points
- and more ...

Ok, enough.

1. you can also have numbered lists
1. pretty cool
1. yeah

You can also include source code

```
hello world
```

Images should go in the `assets/` folder.
Be sure to upload the images within a folder directory under `assets/`
Reference them as you would normally in markdown.
![Dynatrace logo](assets/keptn-logo.png)


<!-- include snippets here -->
{{ snippets/07/install/cluster-gke.md }}


Format of MD file

