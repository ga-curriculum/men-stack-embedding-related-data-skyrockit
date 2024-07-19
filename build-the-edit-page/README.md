# ![MEN Stack Embedding Related Data - Skyrockit - Build the Edit Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to build and implement the `edit` view and route for updating a job application.

For our final page, we'll consult the following user story:

> As a user, when I'm looking at all the details of a job application, I want to be able to change any of the information. There should be an easy-to-find link that takes me to a different page where I can make these changes and then save them.

## Conceptualizing the Route

As always, we start by conceptualizing the route:

```plaintext
GET /users/:userId/applications/edit
```

Next, we'll create the UI that will issue the request to that route. Let's return to `show.ejs` one last time:

```html
<!-- views/applications/show.ejs -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title><%= application.title %></title>
</head>
<body>
  <%- include('../partials/_navbar.ejs') %>
  <h1><%= application.title %> at <%= application.company %></h1>
  <% if (application.notes) { %>
    <p>Notes: <%= application.notes %></p>
  <% } %>
  <% if (application.postingLink) { %>
    <a href="<%= application.postingLink %>">Visit this job posting</a>
  <% } %>
  <p>The status of this application is: <%= application.status %></p>

  <!-- Add the following link -->
  <a href="/users/<%= user._id %>/applications/<%= application._id %>/edit">
    Edit this application
  </a>
  <!--  -->

  <form
    action="/users/<%= user._id %>/applications/<%= application._id %>?_method=DELETE"
    method="POST"
  >
    <button type="submit">Delete this application</button>
  </form>
</body>
</html>
```

## Defining and building the route

Next, in `controllers/applications.js`, we want to look up an application by its `id`, and then render a view containing a form to update it.

```js
// controllers/applications.js

router.get('/:applicationId/edit', async (req, res) => {
  try {
    const currentUser = await User.findById(req.session.user._id);
    const application = currentUser.applications.id(req.params.applicationId);
    res.render('applications/edit.ejs', {
      application: application,
    });
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

## Building the UI

We don't currently have a `applications/edit.ejs` to render, so we'll need to create that file next:

```bash
touch views/applications/edit.ejs
```

The edit form will look very similar to the new form - we want to allow the user to change any of the application data. Since these forms will be so similar it's easiest to copy the code from `views/applications/new.ejs` into the `views/applications/edit.ejs` file we just created. Do this now.

Start our modifications to this by changing the page title from:

```html
  <!-- views/applications/edit.ejs -->

  <title>Add a New App</title>
```

to

```html
  <!-- views/applications/edit.ejs -->

  <title>Editing <%= application.title %></title>
```

This will make the page title change depending on which application is being edited.

Next change the header on the page to read:

```html
  <!-- views/applications/edit.ejs -->

  <h1>Edit <%= application.title %> at <%= application.company %></h1>
```

### Shaping the data

When revising the form for editing job applications, we'll largely use the same structure as our 'new' form. However, the key difference in the `edit.ejs` file is that we already have existing data for the application.

The goal is to pre-fill the form with the current data so the user only has to modify the parts they want to update. This approach significantly improves the user experience, as it prevents the need to re-enter all the information for minor changes.

To achieve this, we set the `value` attribute for each input field to reflect the data currently stored in the database. For instance, the input for the company name would look like this:

```html
    <!-- views/applications/edit.ejs -->

    <input
      type="text"
      name="company"
      id="company"
      value="<%= application.company %>"
    >
```

In this example, `<%= application.company %>` dynamically inserts the existing company name from the application data into the input field.

For the `<select>` dropdown that handles the 'status', additional logic is required. We need to ensure that the dropdown shows the current status as the selected option. This involves adding a conditional statement to each `<option>` to check whether it should be marked as 'selected' based on the current data:

```html
    <!-- views/applications/edit.ejs -->

    <select id="status" name="status">
      <option 
        value="interested" 
        <%= application.status === 'interested' ? 'selected' : '' %>
      >
        Interested
      </option>
      <option 
        value="applied"
        <%= application.status === 'applied' ? 'selected' : '' %>
      >
        Applied
      </option>
      <!-- ...more options with similar conditional logic -->
    </select>
```

Here, the expression `<%= application.status === 'interested' ? 'selected' : '' %>` checks if the current status is `'interested'`. If it is, the `'selected'` attribute is added to the option, making it the default selected option in the dropdown. This same logic is applied to all other status options.

Our form action will match our future `update` route:

```html
  <!-- views/applications/edit.ejs -->

  <!-- We'll use method-override to allow us to hit a put route: -->
  <form 
    action="/users/<%= user._id %>/applications/<%= application._id %>?_method=PUT"
    method="POST"
  >
```

Let's make all of these changes and finalize our form:

```html
<!-- views/applications/edit.ejs -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Editing <%= application.title %></title>
</head>
<body>
  <%- include('../partials/_navbar.ejs') %>
  <h1>Edit <%= application.title %> at <%= application.company %></h1>
  <form 
    action="/users/<%= user._id %>/applications/<%= application._id %>?_method=PUT"
    method="POST"
  >
    <label for="company">Company:</label>
    <input 
      type="text" 
      name="company" 
      id="company" 
      value="<%= application.company %>"
    >
    <label for="title">Title:</label>
    <input type="text" name="title" id="title" value="<%= application.title %>">
    <label for="notes">Notes:</label>
    <textarea name="notes" id="notes"><%= application.notes %></textarea>
    <label for="postingLink">Posting Link:</label>
    <input 
      type="text" 
      name="postingLink" 
      id="postingLink" 
      value="<%= application.postingLink %>"
    >
    <label for="status">Status:</label>
    <select id="status" name="status">
      <option 
        value="interested" 
        <%= application.status === "interested" ? "selected" : "" %>
      >
        Interested
      </option>
      <option 
        value="applied" 
        <%= application.status === "applied" ? "selected" : "" %>
      >
        Applied
      </option>
      <option 
        value="interviewing" 
        <%= application.status === "interviewing" ? "selected" : "" %> 
      >
        Interviewing
      </option>
      <option 
        value="rejected" 
        <%= application.status === "rejected" ? "selected" : "" %>
      >
        Rejected
      </option>
      <option
        value="accepted"
        <%= application.status === "accepted" ? "selected" : "" %>
      >
        Accepted
      </option>
    </select>
    <button type="submit">Update Application</button>
  </form>
</body>
</html>
```

The last step to to update our data in the database.
