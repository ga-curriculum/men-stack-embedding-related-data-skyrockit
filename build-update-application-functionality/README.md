# ![[tktk Module Name] - Build Update Functionality](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to implement update functionality in a MEN stack application.


Now that we have our edit view, we can handle the updating of the resource! 

## Conceptualizing the Route

As always, we'll start by conceptualizing our route: 

```plaintext
PUT /users/:userId/applications/:applicationId
```

This matches the route we gave the `action` attribute of our `<form>`. 

```html
/users/<%= user._id %>/applications/<%= application._id %>
```

## Defining the route and coding the controller

Next, in `controllers/applications.js`, define the route: 

```js
router.put('/:applicationId', async (req, res) => {

});
```

Then, code the controller function: 

```js
router.put('/:applicationId', async (req, res) => {
  try {
    // Find the user from req.session
    const currentUser = await User.findById(req.session.user._id);
    // Find the current application from the id supplied by req.params
    const application = currentUser.applications.id(req.params.applicationId);
    // Use the Mongoose .set() method, updating the current application to reflect the new form data on `req.body`
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