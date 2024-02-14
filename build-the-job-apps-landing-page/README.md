# ![MEN Stack Embedding Related Data - Skyrockit - Build the Job Apps Landing Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to build the `applications` index route and view.

## Building the Landing page

Now that we have most of the key components in place, we can start to connect our controller to pages a user interacts with.

### Conceptualizing the route.

Before we program this route let's refresh our memories on the action and route combination needed for a landing page or "index" route:

```
GET /users/:userId/applications
```

If this route looks a little different from what we defined in our server, thats by design. We'll be adding a user id to our route parameters as soon as we have our basic route functionality in place.

### Create the applications `index` route

First we'll create the route that will eventually `GET` all of the users applications:

```javascript
// controllers/applications.js

router.get('/', (req, res) => {
  res.send('hello applications index route');
});
```

Since we have already wired up this controller to the server and associated it with a route, we can test it in our browser. Navigate to `localhost:3000/users/applications`.  If you have successfully connected the route, you should see your "hello applications index route" response.


### Render the applications `Index` template

Let's create a folder to house all of the views for our `applications` controller. Inside it create an `index.ejs` file: 

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

Test it out - `localhost:3000/users/applications`. If you see the rendered page then congratulations, you are ready to move forward!
