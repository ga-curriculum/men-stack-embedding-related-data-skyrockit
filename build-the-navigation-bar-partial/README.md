# ![Job Application Tracker App - Build the Navigation Bar Partial](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create a partial to be used on multiple pages and reduce code duplication.

## What is a Partial

When building an app you may notice that there are elements of user facing code that are repeated in order to render on multiple pages. We know that we want to practice D.R.Y programming, and copying and pasting these repetitive elements goes against that principle.

Luckily for us, EJS makes use of "partials". In short, a partial is an an EJS template that is made to be rendered inside of other templates. A great example of something that is rendered on multiple pages of an app is a navbar. The code for the navbar of an app is likely to remain the same regardless of what page the user is navigating to, and so being able to write the code once and reuse it on each page is very handy! To demonstrate the power and utility of partials, we will create and render a navbar partial for our application.

## Creating a partial template

Our partial will be used by different views in our application, so for code clarity we will place this partial in it's own subdirectory:

```bash
mkdir views/partials
touch views/partials/_navbar.ejs
```

**Note:** We are using an underscore as a naming convention for our partial. This is a naming convention that reinforces tha we are rendering a partial in our template.

Next, in this new template we'll add the following code:

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
  <%- include('../partials/_navbar.ejs') %> // this is the line that renders our partial
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
  <%- include('../partials/_navbar.ejs') %> // this is our partial
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

First, let's add the partial to our site's landing page:

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
  <h1>Welcome to application tracker 3000, guest.</h1>
</body>
</html>
```

**Note** The addition of this partial now makes all of the previous links on this page redundant.  You can remove the links you had and let the partial do the work for you! Test it out and you can see the partial rendered in your application landing page now.

### Including the partial on the "applications" landing page

We can now move on to adding the `navbar` partial to our "applications" landing page. **Note** that our partial logic requires a user so we will now need to pass user data in our controller function.  Luckily, we have already written middleware logic to handle passing a `user` to our template.

First, we'll add the partial to the page:

```html
<!-- views/applications/index.ejs -->

<body>
  <%- include('../partials/_navbar.ejs') %>
  <h1>Welcome to your application tracker</h1>
</body>
```

Refresh the page and you should see the navbar rendered above your h1. Next, we are ready to start building out the application.
