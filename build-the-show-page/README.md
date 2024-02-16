# ![MEN Stack Embedding Related Data - Skyrockit - Build the Show Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will have successfully built a 'Show' route and view as part of their web application, enabling them to perform a detailed 'Read' operation on individual items.

## Conceptualizing the route

Now that we have an index view, let's look at another User Story: 

> As a user, I need to be able to click on any job in my list and see all the details about it on a new page. This includes everything I've recorded about that job application.

Our current index page provides a snapshot of each application, displaying only its `title` and `company`. However, there's more data associated with each application that isn't yet visible. To address this, we'll create a 'show' view. This view will present all the details of a specific job application, allowing users to gain a complete understanding of each application's status and information.

The route is defined as follows:

```text
GET /users/:userId/applications/:applicationId
```

This route is structured to fetch a particular application, identified by its unique `:applicationId`, associated with a specific user, by `:userId`.


## Link to the `show` page

To start, let's update our `applications/index.ejs` file. We will modify it to include links that lead to the detailed `show` view of each application:

```html
<!-- views/applications/index.ejs -->

<ul>
  <% applications.forEach((application)=>{ %>
    <li>
      <!-- Add a link around each title and company, directing to the 'show' page -->
      <a href="/users/<%= user._id %>/applications/<%= application._id %>">
          <%= application.title %> at <%= application.company %>
      </a>
    </li>
  <% }) %>
</ul>
```

## Build the route

Next, we'll build a simple controller function:

```js
// controllers/applications.js

router.get('/:applicationId', (req, res) => {
  res.send(`here is your request param: ${req.params.applicationId}`);
});
```

Click one of the new application links to see your `applicationId`. It works! 


With this `id` we can now look up specific applications in our database. Modify the controller function to look up an application by `id` and render the `show` page:

```js
// controllers/applications.js

router.get('/:applicationId', async (req, res) => {
  try {
    // Look up the user from req.session
    const currentUser = await User.findById(req.session.user._id);
    // Find the application by the applicationId supplied from req.params
    const application = currentUser.applications.id(req.params.applicationId);
    // Render the show view, passing the application data in the context object
    res.render('applications/show.ejs', {
      application: application,
    });
  } catch (error) {
    // If any errors, log them and redirect back home
    console.log(error);
    res.redirect('/')
  }
});
```

## Building the UI

Next, we'll need to add the view we rendered above:

```bash
touch views/applications/show.ejs
```

To our boilerplate, we'll add some EJS to display the `application` data:

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
</body>
</html>
```

Note that some of the information we collected from the user was optional. We want to make sure we account for any optional data using conditional logic so that the user is only shown information they opted to include. We can do this by using the `<% if(data) %>` convention.