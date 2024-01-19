# ![[tktk Module Name] - Build the Edit Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to implement edit functionality in a MEN stack application.

Let's tackle our final user story!

> AAU, when viewing the details of an application, I should be able to follow a link to be taken to a page where I can edit any of the details of a job application and update it from there.

## Conceptualizing the Route

As always, we start by conceptualizing the route:

```plaintext
GET /users/:userId/applications/edit
```

Next, we'll create the UI that will issue the request to that route. Let's return to `views/applications/show.ejs` one last time:  

```html
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

## Defining the route and coding the controller

Next, in `controllers/applications.js`, we'll define the route: 

```js
router.get('/:applicationId/edit', async (req, res) => {

});
```

And then code out the controller function. We'll look up an application by it's `_id`, and then render a view containing a form to update it: 

```js
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

We don't currently have a `applications/edit.ejs` to render, so we'll need to create that file next: 

```bash
touch views/applications/edit.ejs
```

Most of the code for the `edit` form will be similar to that of the `new` form. The primary difference is that, in `edit.ejs`, we have `application` data that is being passed to the view. 

We want the user to have all of the current applications data filled into the forms, so that they only need to change whatever input needs updating. 
As you can imagine, it would be a very bad user experience to have to re-enter all of the data every time you edit a resource. 

To avoid this, we can set each input's `value` attribute equal to the current data in the database. For the `<select>` dropdown, we also need to do a bit of extra work to ensure that only the option that matches the current `status` is selected.

```html
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