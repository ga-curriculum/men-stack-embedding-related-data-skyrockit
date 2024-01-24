# ![Job Application Tracker App - tktk Microlesson Name](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create a controller file, connect the file to `server.js`, and test out the associated logic.

## Creating our Application Controller

We have an application schema in place and want to move forward with creating the logic that allows users to actually CRUD our data.  However, we also want to make sure that we do not get lost in the process of building out our logic.  A good way to get started is to simply create a controller file and establish our HTTP verb logic.  We can focus on each element of CRUD functionality after ensuring we have correctly established our routes.  So let's get started!

### Creating and connecting our applications controller

We first want to create the file that will house our CRUD logic for applications.  We will create that file in the `controllers` directory:

```bash
touch controllers/applications.js
```

After we create the file, we will want to add a connection to the database and export logic so all of our functionality will be available to the `server.js`

```javascript
// controllers/applications.js

const express = require('express');
const router = express.Router();

const User = require('../models/user.js');

// we will build out our router logic after the import of the user model and above our module.exports line

module.exports = router;
```

Next, we will import the applications controller into `server.js` above our definition of `port`:

```javascript
// server.js

const applicationsController = require('./controllers/applications.js');
```

Finally, we will connect our controller to a route to test our stubbed routes. The following line can be placed just above our `app.listen` logic in the server:

```javascript
// server.js

app.use('/users/applications', applicationsController);
```

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

### Adding and testing the "New" route

Having established a base level format for our get route we can do the same for all other CRUD routes related to our "applications". Our next route to add and test will be a GET route that displays the new application form. Again, we really only care about connecting the request and sending a response so let's do that underneath our last route:

```javascript
// controllers/applications.js

router.get('/new', (req, res) => {
  res.send('you have made a request to the new application route');
});
```

You can test that this route works by navigating to `localhost:3000/users/applications/new`.  If you are greeted with the response you are ready to move on to the next route.

### A route to show an application

Like the last two routes we can follow the same pattern to establish a route that will eventually show a created application.  For this one, we want to use a route param to handle an application id.  To test this, we will define the route and send the value of that route param as the response as follows:


```javascript
// controllers/applications.js
router.get('/:applicationId', (req, res) => {
  res.send(`here is your request param: ${req.params.applicationId}`);
});
```

### The edit page route
We also want to create a route that allows a user to reach a form to edit an already existing application. To do so, we will create another GET route that will include the route param but also "/edit" appended to it. Add the following code to your controller:


```javascript
// controllers/application.js
router.get('/:applicationId/edit', (req, res) => {
  res.send(`You are at the edit page for this application:  ${req.params.applicationId}`);
});
```

### Our route for creating an application
All of our previous routes were get requests which allowed for us to be able to test them in the browser. For our remaining routes, we will need to use a tool like "Postman" to test our routes.  These routes will eventually send data to create or edit an application. Our delete route will remove an application from the database. Since these routes are not simply requesting information they will need to use a different HTTP verb per route to signify their intent. Our first route will be for creating applications. It follows the same structure as the previous ones but uses the HTTP ver `post` in place of `get`. Let's add the following code to your controller:


```javascript
// controllers/applications.js
router.post('/', (req, res) => {
  res.send('you have reached the POST route for applications');
});
```

To test this, we will run a POST request to this route using Postman:

![Postman](./assets/originals/postman.png)

We can test this route in a similar manner to the previously created routes. If you get the response you created, you can move forward with wiring up the last two routes.

### Edit and Delete functionality.

Our last two routes will be used for editing and deleting job applications. We will add them and test them in this one step as they have the same pattern but with different HTTP verbs:

```javascript
// controllers/applications.js

router.get(':/applicationId/edit', (req, res) => {
 res.send(`You have reached the EDIT route for req.params: ${req.params.applicationId}`);
});

router.put('/:applicationId', (req, res) => {
  res.send(`You have reached the PUT route for req.params: ${req.params.applicationId}`);
});

router.delete('/:applicationId', (req, res) => {
  res.send(`You have reached the DELETE route for req.params: ${req.params.applicationId}`);
});
```

Test these routes in Postman by changing the verb and clicking on "Send". If you get the responses you created all of your routes have been mounted and are ready for modification!
