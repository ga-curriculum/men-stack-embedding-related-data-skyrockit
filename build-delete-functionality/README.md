# ![MEN Stack Embedding Related Data - Skyrockit - Build Delete Functionality](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to implement delete functionality in a MEN stack application. 

Next, we'll tackle the following user story: 

> AAU, when viewing the details of an application, I should be able to click a button and delete the application.

## Conceptualizing the Route

Referencing [RESTful Routing Conventions](https://git.generalassemb.ly/modular-curriculum-all-courses/restful-routing/blob/main/routing-conventions/README.md) again, we find that to delete an application, the proper route will be:

```plaintext
DELETE /users/:userId/applications/:applicationsId
```

## Adding the UI to issue the request

Next, we'll need to create the UI that will issue the result to the route. In `show.ejs`, add the following `<form>`: 

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

  <!-- We'll use method-override to allow us to hit a delete route: -->
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

Next, in `controllers/applications.js`, update the delete route from:  

```js
// controllers/applications.js

router.delete('/:applicationId', (req, res) => {
  res.send(`You have reached the DELETE route for req.params: ${req.params.applicationId}`);
});
```

to: 

```js
// controllers/applications.js

router.delete('/:applicationId', async (req, res) => {
  try {
    // Look up the user from req.session
    const currentUser = await User.findById(req.session.user._id);
    // Use the Mongoose .deleteOne() method to delete 
    // an application using the id supplied from req.params
    currentUser.applications.id(req.params.applicationId).deleteOne();
    // Save changes to the user
    await currentUser.save();
    // Redirect back to the applications index view
    res.redirect(`/users/${currentUser._id}/applications`);
  } catch (error) {
    // If any errors, log them and redirect back home
    console.log(error);
    res.redirect('/')
  }
});
```

We don't need to create a new view, as we'll just be redirecting back to the existing index page! 