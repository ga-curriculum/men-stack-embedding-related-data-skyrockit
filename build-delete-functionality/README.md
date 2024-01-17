# ![[tktk Module Name] - Build Delete Functionality](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

Next, we'll tackle the following user story: 

> AAU, I should be able to delete an application from the show view. 

First, let's conceptualize the route: 

```plaintext
DELETE /users/:userId/applications/:applicationsId
```

Next, we'll need to create the UI that will issue the result to the route. In `views/applications/show.ejs`, add the following `<form>`: 

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

  <!-- We use method-override to allow us to hit a delete route: -->
  <form
    action="/users/<%= user._id %>/applications/<%= application._id %>?_method=DELETE"
    method="POST"
  >
    <button type="submit">Delete this application</button>
  </form>
  
</body>
</html>
```

[tktk method-override install]


Next, in `controllers/applications.js`, define the route: 

```js
router.delete('/:applicationId', async (req, res) => {

});
```

Then, code out the controller function: 

```js
router.delete('/:applicationId', async (req, res) => {
  try {
    // Find the user from req.session
    const currentUser = await User.findById(req.session.user._id);
    // Use the Mongoose .deleteOne() method to delete 
    // an application by the id supplied from req.params
    currentUser.applications.id(req.params.applicationId).deleteOne();
    // Save the user
    await currentUser.save();
    // Redirect back to the applications index view
    res.redirect(`/users/${currentUser._id}/applications`);
  } catch (error) {
    // If necessary, log any errors and redirect home
    console.log(error);
    res.redirect('/')
  }
});
```

We don't need to create a new view, as we'll just be redirecting back to the existing index page! 