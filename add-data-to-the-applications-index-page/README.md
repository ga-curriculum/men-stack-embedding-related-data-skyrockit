# ![MEN Stack Embedding Related Data - Skyrockit - Add Data to the Applications Index Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to implement an index route and view in a MEN stack application.

Currently, when we finish creating a new application, our controller redirects to `/applications`. Now that there is data in the database for this resource, we want `/applications` to be a page that indexes all of the applications we've created. 

Let's get started! 

## Conceptualizing the route

We've already done this! The route will be: 

```plaintext
GET /users/:userId/applications
```

## Add the UI

Submitting a new application will automatically redirect us here, and we already have a link in our nav partial! 

## Define the route and build the controller

In order to list all of the applications created by the user, we'll first need to look up the current user in the database. When rendering the index view, the `applications` array of the current user will get passed to the view in the context object. Let's update our controller function: 

```js
// controllers/applications.js

router.get('/', async (req, res) => {
  try {
    // Look up the user from req.session
    const currentUser = await User.findById(req.session.user._id);
    // Render index.ejs, passing in all of the current user's 
    // applications as data in the context object. 
    res.render('applications/index.ejs', {
      applications: currentUser.applications,
    });
  } catch (error) {
    // If any errors, log them and redirect back home
    console.log(error)
    res.redirect('/')
  }
});
```

## Building the view

Now that we have a controller for our index route, let's update our `applications/index.ejs` file to display the applications data: 

```html
<!-- applications/index.ejs -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Your Applications</title>
</head>
  <body>
    <%- include('../partials/_navbar.ejs') %>
    <h1>Welcome to your application tracker</h1>
    <!-- Add the following: -->
    <a href="/users/<%=user._id%>/applications/new">Add a New Application</a>
    <ul>
      <% applications.forEach((application)=>{ %>
        <li>
            <%= application.title %> at <%= application.company %>
        </li>
      <% }) %>
    </ul>
  </body>
</html>
```

Inside of a `<ul>` element, we loop over the `applications` array we receive from the context object, and create an `<li>` for each application. For now, we'll just display the title and company. Try adding another application - you should be able to see a list of all of the applications you've created on this page! 





