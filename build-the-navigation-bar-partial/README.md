# ![MEN Stack Embedding Related Data - Skyrockit - Build the Navigation Bar Partial](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to understand and implement EJS partials in their web applications by creating a reusable nav bar component.

## What is a Partial

When creating an application, you might find that certain pieces of UI code are repeated on multiple pages. To avoid copy and pasting the same code (which goes against best practice of writing D.R.Y. code), we can use a feature in EJS called "partials."

A partial is essentially a small EJS template designed to be included or embedded within other EJS templates. A common use case for partials is for components like navigation bars, which are typically consistent across different pages of an app. The code for the navbar of an app is likely to remain the same regardless of what page the user is navigating to, and so being able to write the code once and reuse it on each page is very handy! To demonstrate the power and utility of partials, we will create and render a navbar partial for our application.

## Creating a partial template

Our partial will be used by different views in our application, so for code clarity we will place this partial in it's own subdirectory:

```bash
mkdir views/partials
touch views/partials/_navbar.ejs
```

**Note:** In our application, the navbar partial is named with an underscore at the beginning -  `_navbar.` This underscore is a common naming convention used to indicate that the file is a partial. It's a helpful reminder that this piece of code is meant to be a reusable component, embedded within other templates, rather than a standalone page.

Next, add the following code to our new nav partial:

```html
<!-- views/partials/_navbar.ejs -->

<nav>
  <% if(user) { %>
    <a href="/users/<%=user._id%>/applications">View Your Applications</a>
    <a href="/auth/sign-out">Sign Out</a>
  <% } else { %>
    <a href="/auth/sign-in">Sign In</a>
    <a href="/auth/sign-up">Sign Up</a>
  <% } %>
</nav>
```

The above code applies conditional rendering based on the existence of a user. When setting up our middleware we either passed a user into each view or null. If there is no signed in user, only the sign in and sign up links will be visible, otherwise the user will see a link to go view all of their applications. 

We'll add our `user` back into the mix momentarily! 

## Using partials

The process for including a partial in other templates is relatively straightforward. We will be including our newly created partial in four templates:

- `sign-up.ejs`
- `sign-in.ejs`
- `index.ejs`
- `applications/index.ejs`

Let's go ahead and include this partial in our `sign-up` and `sign-in` templates:

```html
<!-- views/auth/sign-up.ejs -->

<body>
  <%- include('../partials/_navbar.ejs') %> // add your nav here
  <h1>Create a new account!</h1>
  <form action="/auth/sign-up" method="POST">
    <label for="username">Username:</label>
    <input type="text" name="username" id="username" />
    <label for="password">Password:</label>
    <input type="password" name="password" id="password" />
    <label for="confirmPassword">Confirm Password:</label>
    <input type="password" name="confirmPassword" id="confirmPassword" />
    <button type="submit">Sign up</button>
  </form>
</body>
```

```html
<!-- views/auth/sign-in.ejs -->

<body>
  <%- include('../partials/_navbar.ejs') %> // add your nav here
  <h1>Sign in</h1>
  <form action="/auth/sign-in" method="POST">
    <label for="username">Username:</label>
    <input type="text" name="username" id="username" />
    <label for="password">Password:</label>
    <input type="password" name="password" id="password" />
    <button type="submit">Sign in</button>
  </form>
</body>
```

If all is working well, you should now see the links above your forms and know that your partial is rendering!

Now that we have the partial working for our `sign-up` and `sign-in` pages we can render it onto our site index page and the site landing page as well.

First, let's add the partial to our site's homepage:

```html
<!-- views/index.ejs -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Home Page</title>
</head>
<body>
  <%- include('./partials/_navbar.ejs') %>
  <h1>Welcome to Skyrockit, guest.</h1>
</body>
</html>
```

**Note** The addition of this partial now makes all of the previous links on this page redundant. You can remove the all previous starter code markup and let the partial do the work for you! Test it out and you can see the partial rendered in your application landing page now.

### Including the `nav` partial on the `applications` landing page

Let's also add our `navbar` partial to our `applications` landing page:

```html
<!-- views/applications/index.ejs -->

<body>
  <%- include('../partials/_navbar.ejs') %>
  <h1>Welcome to your application tracker</h1>
</body>
```

Refresh the page and you should see the navbar rendered above your h1.

## Adding `userId` for future routes

With our `isSignIn` middleware temporarily disabled, we were able to build and test our first `applications` controller route. However, all of our future routes require a `userId` for their functionality, which can only come from having a signed in user. 

Its time to add the userId as a route parameter in the path for our `applications` controller and reinstate our `isSigned` middleware. 

```js
// server.js

app.use('/auth', authController);
app.use(isSignedIn);
app.use('/users/:userId/applications', applicationsController);
```

Next, we begin our CRUD routes. 