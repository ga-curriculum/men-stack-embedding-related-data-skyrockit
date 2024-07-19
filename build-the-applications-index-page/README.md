# ![MEN Stack Embedding Related Data - Skyrockit - Build the Applications Index Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to build the `applications` index route and view.

## Building the applications index page

Now that we have most of the key components in place, we can start to connect our controller to pages a user interacts with.

### Conceptualizing the route

Before we program this route, let's refresh our memories on the action and route combination needed for a landing page or index route:

```text
GET /users/:userId/applications
```

### Create the applications index route

First we'll create the route that will eventually `GET` all of the user's applications:

```javascript
// controllers/applications.js

router.get('/', (req, res) => {
  res.send('Hello applications index route!');
});
```

We can't test this yet though, we don't have a way to get users to this page easily and the value of a user's `_id` will be different for each one. Let's create a way to test it that will also improve our user's experience at the same time!

## Redirect signed in users to the index page

For an improved user experience, it will be beneficial to guide signed-in users directly to a page where they can see an index of their applications. Conversely, users who are not signed in should be directed to the homepage, where they have the option to sign up or sign in. Our starter code already includes a basic index route, but we'll adjust its logic to cater to these different user scenarios.

Let's update the main index route in our `server.js` file:

```js
// server.js

app.get('/', (req, res) => {
  // Check if the user is signed in
  if (req.session.user) {
    // Redirect signed-in users to their applications index
    res.redirect(`/users/${req.session.user._id}/applications`);
  } else {
    // Show the homepage for users who are not signed in
    res.render('index.ejs');
  }
});
```

Now signed in users can bypass the homepage and go right to their applications. Test this now! Ensure you are signed in, and navigate to the home page. Instead of seeing the content in `index.ejs` you should be redirected to the new route we just created and see the **Hello applications index route!** response. Let's replace that simple response with a full page next!

### Render the applications index template

Let's create a directory to house all of the views for our `applications` controller. Inside it create an `index.ejs` file:

```bash
mkdir views/applications
touch views/applications/index.ejs
```

Once this file is created, let's add some boilerplate:

```html
<!-- views/applications/index.ejs -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Your Apps</title>
</head>
<body>
  <h1>Your Apps</h1>
</body>
</html>
```

Now change the controller logic from using `res.send` as a response to using `res.render` to render this newly created template.

```javascript
// controllers/applications.js

router.get('/', async (req, res) => {
  try {
    res.render('applications/index.ejs');
  } catch (error) {
    console.log(error)
    res.redirect('/')
  }
});
```

Note that in the above snippet we aren't passing any information to the template yet. We'll be adding more to this controller function later - for now, we are just interested in making sure that the template is rendering when the route is accessed from the server. Test it out now! Ensure you are signed in, and navigate to the home page.
