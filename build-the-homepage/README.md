# ![MEN Stack Embedding Related Data - Skyrockit - Build the Homepage](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will have modified the provided starter code to create a new, thematic homepage for the Skyrockit application.

## Updating homepage starter code

Initially, the **MEN Stack Auth Template** provides a generic homepage setup. This page uses conditional rendering to show different content based on whether a user is signed in or not. We will tailor this page to align with the Skyrockit app's theme, focusing on job application tracking.

Here's the original homepage code:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>homepage</title>
</head>
<body>
  <% if (user) { %>
    <h1>Welcome to the app, <%= user.username %>!</h1>
    <p>
      <a href="/auth/sign-out">Sign out</a>
    </p>
  <% } else { %>
    <h1>Welcome to the app, guest.</h1>
    <p>
      <a href="/auth/sign-up">Sign up</a> or
      <a href="/auth/sign-in">Sign in</a>.
    </p>
  <% } %>
  <p><a href="/vip-lounge">Get into the VIP Users Only lounge!</a></p>
</body>
</html>
```

To customize this for Skyrockit, we'll make some changes to reflect the app's purpose and branding:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Skyrockit</title> 
</head>
<body>
  <h1>Skyrockit</h1>
  <p>A job application tracker.</p>
  <a href="/auth/sign-up">Sign up</a>
  <a href="/auth/sign-in">Sign in</a>
</body>
</html>
```

## Update server starter code

In addition to updating the homepage, let's tidy up the server code in `server.js` by removing the route for the **VIP lounge**:

```js
// server.js

app.get('/', (req, res) => {
  res.render('index.ejs', {
    user: req.session.user,
  });
});

// We've removed the VIP lounge!

app.use('/auth', authController);

app.listen(port, () => {
  console.log(`The express app is ready on port ${port}!`);
});
```
