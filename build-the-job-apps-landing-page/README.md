# ![Job Application Tracker App - Build the Job Apps Landing Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able build the jobs app landing page.

## Jobs Landing page Build

Now that we have most of the key components in place we can start to build out the logic to connect our controller logic to rendered pages a user interacts with.

### Establishing our index route.

We want to create an index route that will direct a logged in user towards an application index and a non-logged in visitor to an index page where they will need to register or logged in.  The application that we cloned down already has an index route defined, we just need to modify the logic to accomplish this:

Change the index route in the server to look like the following
`server.js`
```javascript
app.get('/', (req, res) => {
  if (req.session.user) {
    res.redirect(`/users/${req.session.user._id}/applications`);
  } else {
    res.render('index.ejs');
  }
});
```

You will notice that the redirect goes to something that looks like the route we established for our "applications" controller but has an added route parameter that we did not use while testing.  Let's modify our logic to include this:

`server.js`
```javascript
app.use(
  '/users/:userId/applications',
  (req, res, next) => {
    if (req.session.user && req.session.user._id === req.params.userId) {
      next();
    } else {
      res.redirect('/');
    }
  },
  applicationsController
);
```

Note that our new route includes a route parameter to include the `userId`.  We are also checking to see if the session user id matches the params userId.  If they do, we can proceed forward.  If not, we will redirect to the index.

Test out the sign in process to see what happens.  If you are redirected to your response of "hello applications index route" you can trust that our session logic is working with our "applications" logic and we can move forward with rendering a template.

### Render the Applications Index template

Let's create a folder to house all of our "application" ejs templates and also add our `index.ejs` view template as well

```bash
mkdir views/applications
touch views/applications/index.ejs
```

Once this file is created, let's user the boilerplate shortcut or you can fill the page out with this format:

`views/applications/index.ejs`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Applications Index</title>
</head>
<body>
  <h1>Application Index Page</h1>
</body>
</html>
```

Our last step here will be to change the controller logic from using `res.send` as a response to using `res.render` to render this newly created template.

`controllers/applications.js`
```javascript
router.get('/', async (req, res) => {
  try {
    res.render('applications/index.ejs');
  } catch (error) {
    console.log(error)
    res.redirect('/')
  }
});
```

Note that in the above snippet we aren't passing any information to the template yet.  We are interested in making sure that the template is rendering when the route is accessed from the server.

Test it out.  If you see the rendered page congratulations!!  You are ready to move forward!
