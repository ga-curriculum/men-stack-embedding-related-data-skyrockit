<h1>
  <span class="headline">Skyrockit</span>
  <span class="subhead">Build Delete Functionality</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to build and implement a `DELETE` route and form in their Express application.

Next, we'll tackle the following user story:

> As a user, if I'm looking at the details of a job application, I want a simple way to delete it completely, like clicking a 'Delete' button.

## Conceptualizing the route

This route will be defined as:

```plaintext
DELETE /users/:userId/applications/:applicationsId
```

This allows us to delete a specific application by `id`.

## Adding the UI to issue the request

Next, we'll need to create the UI that will issue the request to the route. In `show.ejs`, add the following `<form>`:

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
    <% } %> <% if (application.postingLink) { %>
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

## Building the route

Just like we did in our `show` route, we will use an `id` to look up a specific application, only this time we will delete it.

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
    res.redirect('/');
  }
});
```

For this route, we don't need to create a new view, as we'll just be redirecting back to the existing index page!
