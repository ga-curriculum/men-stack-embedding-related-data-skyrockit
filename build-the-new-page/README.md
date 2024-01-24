# ![[tktk Module Name] - Build the New Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to construct a route and view for a user to enter form data for a new resource. 

## Conceptualizing the Route

At this stage, let's recall one of our user stories: 

> AAU I should be able to create job application for jobs that I anticipating applying for or have applied for. I should be able to keep track of the company name, the job title, and the status of the job application. Optionally I can add any notes I have about the job, and the URL to the job posting.

Using [RESTful Routing Conventions](https://git.generalassemb.ly/modular-curriculum-all-courses/restful-routing/blob/main/routing-conventions/README.md), we find that to display a `new.ejs` view with a form for entering a new application, the proper route will be:

```plaintext
GET /users/:userId/applications/new
```

## Defining the route on the server

In `controllers/applications.js`, update the `new` route. Instead of a `res.send()`, we want to `res.render()` our `new.ejs` view:  

```js
router.get('/new', async (req, res) => {
  res.render('applications/new.ejs');
});
```

## Building the UI

Next, it's time to build the UI that will hit our new route. In order to submit user input to the database, we'll need to create a new view that has a form element.

In `views/applications`, create a `new.ejs` file and add HTML boilerplate (!+tab).  

Change the title to "Add a New Application", and add the navbar partial to the top of the `<body>`: 

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

Next, we're ready to add our form. Recalling our user story, we know that this form will need to have inputs for the company name, job title, and status. We also know it should have a notes section, along with a URL.

As always when working with forms, we should first reference our  schema:

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

- `notes` should probably be a `<textarea>`, since there is the possibility that the user will want more space to type in extensive notes. 

- Finally, `status` has enumerable options, meaning that only the strings included in the `enum` array will be considered valid. Forcing users to guess which inputs are allowed would lead to a pretty terrible user experience, so we should make this input a `<select>` dropdown instead, letting them select only from valid options.

### Adding the form

In `new.ejs`, create a new form element. For now, we will leave the `action` and `method` attributes empty:

```html
  <form action="" method="">
  </form>
```

Next, add input fields for our user data. Don't forget to add labels for accessibility! 

```html
  <form action="" method="">
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
  </form>
```

A quick reminder - when we submit the form, the `name` attribute for each input is used as the key in a key:value pair, where the value is the user's input. As a result, the `name` attribute for each input must *exactly* match the corresponding property in our schema.

So, where we have: 

```js
company: {
    type: String,
    required: true,
  }
```

The corresponding input should look like: 

```html
<label for="company">Company:</label>
<input type="text" name="company" id="company">
```

Also, note that the `value` attribute for each option in our `<select>` dropdown exactly matches one of the elements in the `enum` array on our schema: 

```js
status: {
    type: String,
    enum: ['interested', 'applied', 'interviewing', 'rejected', 'accepted'],
  }
```

```html
<select id="status" name="status">
      <option value="interested">Interested</option>
      <option value="applied">Applied</option>
      <option value="interviewing">Interviewing</option>
      <option value="rejected">Rejected</option>
      <option value="accepted">Accepted</option>
    </select>
```

Similar to how the rest of the `input`s work, when the form is submitted the `name` of the `<select>` input will be used as a key, and the `value` of the selected option will be submitted as the value. So, if the user selects "Applied" from the dropdown, the resulting key:value pair would look like this: 

```json
{ "status": "applied" }
```

This is why the `value` must match an `enum` element exactly.


### Submitting the form

Finally, we need a way to submit the form, so let's add a button to the bottom of our `<form>` element and give it a `type` attribute of "submit": 

```html
<form action="" method="">
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
  <!-- add the following: -->
  <button type="submit">Add Application</button>
</form>
```

Fantastic - next, we're ready to build the create functionality and give this form somewhere to submit to! We'll be coming back to update the `action` and `method` on this form, but otherwise the view for a new route is finished. 