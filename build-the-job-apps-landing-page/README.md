# ![MEN Stack Embedding Related Data - Skyrockit - Build the Job Apps Landing Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able build the jobs app landing page.

### Stub our controller routes

With all of our logic connected we can start to stub and test our routing. The focus for us is not to create all of our database and view logic, but rather, to make sure our controller routes can be accessed and send us a response.

In that spirit, let us stub the route that will eventually GET all of the applications created by a user:

```javascript
// controllers/applications.js

router.get('/', (req, res) => {
  res.send('hello applications index route');
});
```

Since we have already wired up this controller to the server and associated it with a route, we can test if this route works in our browser. Navigate to `localhost:3000/users/applications`.  If you have successfully wired up the route, you should see your "hello applications index route" response.


## Building the Landing Page

Now that we have most of the key components in place, we can start to connect our controller logic to pages a user interacts with.

### Conceptualizing the route.

Before we program this route let's refresh our memories on the action and route combination needed for a landing page or "index" route:

```
GET /users/:userId/applications
```

### Modifying our index route.

First, we want to create an index route that will direct a logged-in user towards an index of their applications. If the user is not logged in, we want to redirect them to a page where they can either register or log-in. The application that we cloned down already has an index route defined, so we just need to modify the logic a bit to accomplish this.

Change the index route in the server to look like the following:
```javascript
// server.js

app.get('/', (req, res) => {
  if (req.session.user) {
    res.redirect(`/users/${req.session.user._id}/applications`);
  } else {
    res.render('index.ejs');
  }
});
```

You will notice that the redirect path looks like the route we established for our "applications" controller, but has an added route parameter that we did not use while testing. Let's modify our logic to include this:

```javascript
// server.js

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

Note that our new route now includes a route parameter to include the `userId`. We are also checking to see if the session user id matches the params userId. If they do, we can proceed forward. If not, we will redirect to the index.

Test out the sign in process to see what happens.  If you are redirected to the response of "hello applications index route" you can trust that our session logic is working!

### Render the Applications Index template

Let's create a folder to house all of our "application" ejs templates, and also add our `index.ejs` view template as well: 

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
  <title>Applications Index</title>
</head>
<body>
  <h1>Application Index Page</h1>
</body>
</html>
```

Our last step will be to change the controller logic from using `res.send` as a response to using `res.render` to render this newly created template.

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

Note that in the above snippet we aren't passing any information to the template yet. We'll be adding more to this controller function later - for now, we are just interested in making sure that the template is rendering when the route is accessed from the server. 

Test it out. If you see the rendered page then congratulations, you are ready to move forward!
