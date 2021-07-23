# MD file format

## Use this format below in the beginning of the MD file

- ID: The ID will define the folder that the tutorial contents will be generated and put into
  - Add `-jp` or language as suffix for translated lab. Eg. **kubernetes-jp.md**
- Summary: Short summary of your tutorial. Text is used within overview page
- Categories: The first item in the categories list defines the "icon" that you will attached to your lab in the tutorial overview screen. 
  - You can add up to 3 entries / icons. Icons will have to added within /app/view/categories.scss
  - Ending entries should include categories which lab will appear in. 
  - Options are: 
    - infrastructure-monitoring
    - application-microservices-monitoring 
    - cloud-automation
    - digital-experience
    - business-analytics
- Tags: Other searchable elements from the lab. 
  - End the list with level -
  - Options are: 
    - Beginner
    - Intermediate
    - Advanced
- Status: set it to Draft or Published (doesn't have any effect right now)
- Authors: Please list yourself as an author
- Feedback link: either the team email or defaults to SE central email 

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
# My first lab

<!-- heading starting with ## will create a new section. Also include the estimated duration for this section to provide some guidance for the user -->
## Welcome
Duration: 10:00

- ID: The ID will define the folder that the tutorial contents will be generated and put into
  - Add `-jp` or language as suffix for translated lab. Eg. **kubernetes-jp.md**
- Summary: Short summary of your tutorial. Text is used within overview page
- Categories: The first item in the categories list defines the "icon" that you will attached to your lab in the tutorial overview screen. 
  - You can add up to 3 entries / icons. Icons will have to added within /app/view/categories.scss
  - Ending entries should include categories which lab will appear in. 
  - Options are: 
    - infrastructure-monitoring
    - application-microservices-monitoring 
    - cloud-automation
    - digital-experience
    - business-analytics
- Tags: Other searchable elements from the lab. 
  - End the list with level -
  - Options are: 
    - Beginner
    - Intermediate
    - Advanced
- Status: set it to Draft or Published (doesn't have any effect right now)
- Authors: Please list yourself as an author
- Feedback link: either the team email or defaults to SE central email 

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
