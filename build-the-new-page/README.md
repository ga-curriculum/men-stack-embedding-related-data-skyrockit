# ![[tktk Module Name] - Build the New Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

tktk Notes: Draw a connection between the schema and the details a user can enter on this page and reference back to the user story for creating a resource - while this doesn't fulfill that entire user story it is a necessary step on the path there. This is the first time they're seeing a dropdown implementation so it would be good to discuss that. 

## Conceptualizing the Route

At this stage, let's recall one of our user stories: 

> AAU I should be able to create job application for jobs that I anticipating applying for or have applied for. I should be able to keep track of the company name, the job title, and the status of the job application. Optionally I can add any notes I have about the job, and the URL to the job posting.

Using @RESTful/Resourceful Routing Conventions , we find that to display a new.ejs view with a form for entering a new application, the proper route will be:

```plaintext
GET /users/:userId/applications/new
```

[tktk is link added in nav partials?]

## Defining the route on the server

In `server.js`, add the following route that maps the HTTP request to the new action: 

```js
router.get('/new', async (req, res) => {
  res.render('applications/new.ejs');
});
```

## Building the UI

Next, it's time to build the UI that will hit our create route. In order to submit user input to the database, we'll need to create a new view that has a form element.

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

### Conceptualizing the form

Recalling our user story, we know that this form will need to have inputs for the company name, job title, and status. We also know it should have a notes section, along with a URL. 

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

From the schema, we can already infer a few things about how we can best design our form: 

- We can expect `company`, `title`, and `postingLink` to all be single line text inputs. 

- `notes` should probably be a `<textarea>`, since there is the possibility that we'll want more space to type in extensive notes. 

- Finally, `status` has enumerable options, meaning that only the strings included in the `enum` array will be considered valid. Forcing users to guess which inputs are allowed would lead to a pretty terrible user experience, so we will make this input a `<select>` dropdown instead, letting them select from only the valid options.

### Adding the form

```html
  <form action="/users/<%= user._id %>/applications" method="POST">
    <label for="company">Company:</label>
    <input type="text" name="company" id="company">
    <label for="title">Title:</label>
    <input type="text" name="title" id="title">
    <label for="notes">Notes:</label>
    <textarea name="notes" id="notes"></textarea>
    <label for="postingLink">Posting Link:</label>
    <input type="text" name="postingLink" id="postingLink">
    <label for="status">Status:</label>
    <select id="status" name="status">
      <option value="interested">Interested</option>
      <option value="applied">Applied</option>
      <option value="interviewing">Interviewing</option>
      <option value="rejected">Rejected</option>
      <option value="accepted">Accepted</option>
    </select>
    <button type="submit">Add Application</button>
  </form>
```

