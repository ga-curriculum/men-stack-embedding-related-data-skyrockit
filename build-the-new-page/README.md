# ![[tktk Module Name] - Build the New Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

tktk Notes: Draw a connection between the schema and the details a user can enter on this page and reference back to the user story for creating a resource - while this doesn't fulfill that entire user story it is a necessary step on the path there. This is the first time they're seeing a dropdown implementation so it would be good to discuss that. 

Next, it's time to build the UI that will hit our create route. 
Now that we're at this stage, let's recall one of our user stories: 

> AAU I should be able to create job application for jobs that I anticipating applying for or have applied for. I should be able to keep track of the company name, the job title, and the status of the job application. Optionally I can add any notes I have about the job, and the URL to the job posting.

In order to submit user input to the database, we'll need to create a new view that has a form element. We know that this form will need to have inputs for the company name, job title, and status. We also know it should have a notes section, along with a URL. 

First, create a `new.ejs` file in `views/applications`, and add HTML boilerplate (!+tab).  

Change the title to "Add a New Application", and add the navbar partial to the top of the `<body>`. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Add a New Application</title>
</head>
  <body>
    <%- include('../partials/_navbar.ejs') %>
    
  </body>
</html>
```




As always when working with forms, we should first reference the schema:

```js
const applicationSchema = mongoose.Schema({
  company: {
    type: String,
    required: true,
  },
  title: {
    type: String,
    required: true,
  },
  notes: {
    type: String,
  },
  postingLink: {
    type: String,
  },
  status: {
    type: String,
    enum: ['interested', 'applied', 'interviewing', 'rejected', 'accepted'],
  },
});
```

