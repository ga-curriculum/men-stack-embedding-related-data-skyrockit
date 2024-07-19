# ![MEN Stack Embedding Related Data - Skyrockit - Build and Use the Applications Controller](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create and integrate an `applications` controller in their project.

## Creating our Application Controller

With our application schema set up, our next step is to create the logic that enables users to perform CRUD operations on their data. We're going to take a step-by-step approach, working on one part of the app at a time. This includes setting up the routes, creating the views (pages), and building CRUD functionality one page at a time. This structured approach allows us to implement and test each step before moving on to the next.

### Creating and connecting our applications controller

Start by generating a new file in the `controllers` directory. This file will contain the logic for CRUD operations on `applications`:

```bash
touch controllers/applications.js
```

Open the new `applications.js` file.

1. First, import Express and create a router.
2. Then, import the `User` model so we will have access to it in our CRUD routes.
3. Finally, make sure to export the router so that we can use it in our main server file.

```javascript
// controllers/applications.js

const express = require('express');
const router = express.Router();

const User = require('../models/user.js');

// we will build out our router logic here

module.exports = router;
```

Next, we will import the `applications` controller into `server.js`. Place this import near the top, before the `port` variable is defined.

```javascript
// server.js

const applicationsController = require('./controllers/applications.js');
```

Finally, link your controller to a specific route in `server.js`. This tells the server that any incoming requests to `/users/:userId/applications` will be handled by your `applications` controller.

Place it just above our `app.listen` logic in the server:

```javascript
// server.js

app.use('/auth', authController);
app.use(isSignedIn);
app.use('/users/:userId/applications', applicationsController); // New!

app.listen(port, () => {
  console.log(`The express app is ready on port ${port}!`);
});
```

With the controller connected, we are ready to start building out the `applications` pages.
