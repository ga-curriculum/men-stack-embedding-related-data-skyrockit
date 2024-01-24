# ![[tktk Module Name] - Build the Show Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to implement a show route and page in a MEN stack application.

Now that we have an index view, let's look at another User Story: 

> AAU, I should be able to view all the details of a job application on a new page by following a link from that index page.

Currently, our index only shows the title and company, but each application in our database contains a lot more data than that! In order to see all of the data, and fulfil our user story, let's build a `show` view that we can link to from our index. 

To start, let's revisit `views/applications/index.ejs` and set up a link with the proper routing convention: 

```html
<ul>
    <% applications.forEach((application)=>{ %>
      <li>
        <!-- wrap the title and company in a link -->
        <a href="/users/<%= user._id %>/applications/<%= application._id %>">
            <%= application.title %> at <%= application.company %>
        </a>
      </li>
    <% }) %>
  </ul>
```

## Defining the route and coding the controller

Then, code out the body of the controller function. Update the current router function from:

```js
router.get('/:applicationId', (req, res) => {
  res.send(`here is your request param: ${req.params.applicationId}`);
});
```

to: 

```js
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

Note that some of the information we collected from the user was optional. We want to make sure we account for any optional data using conditional logic, so that the user is only shown information they opted to include. We can do this by using the `<% if(data) %>` convention.

