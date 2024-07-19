# ![MEN Stack Embedding Related Data - Skyrockit - Build Update Functionality](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will learn how to `update` specific data in a job application by implementing a `PUT` route.

## Conceptualizing the route

Now that we have our edit view, we can handle the updating of the resource in the database!

As always, we'll start by conceptualizing our route:

```plaintext
PUT /users/:userId/applications/:applicationId
```

This matches the route we gave the `action` attribute of our `<form>`.

```html
  <!-- views/applications/edit.ejs -->

  <!-- We'll use method-override to allow us to hit a put route: -->
  <form 
    action="/users/<%= user._id %>/applications/<%= application._id %>?_method=PUT"
    method="POST"
  >
```

## Defining the route and building the controller function

Next, in `controllers/applications.js`, let's build our `update` route:

```js
// controllers/applications.js`

router.put('/:applicationId', async (req, res) => {
  try {
    // Find the user from req.session
    const currentUser = await User.findById(req.session.user._id);
    // Find the current application from the id supplied by req.params
    const application = currentUser.applications.id(req.params.applicationId);
    // Use the Mongoose .set() method
    // this method updates the current application to reflect the new form
    // data on `req.body`
    application.set(req.body);
    // Save the current user
    await currentUser.save();
    // Redirect back to the show view of the current application
    res.redirect(
      `/users/${currentUser._id}/applications/${req.params.applicationId}`
    );
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

> 🧠 The application's existing data is updated with the new data submitted in the request body (`req.body`). This is done using Mongoose's `.set()` method, which is a convenient way to update subdocuments in an embedded schema.
