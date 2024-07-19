# ![MEN Stack Embedding Related Data - Skyrockit - Build the Navigation Bar Partial](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to understand and implement EJS partials in their web applications by creating a reusable nav bar component.

## What is a partial

When creating an application, you might find that certain pieces of UI code are repeated on multiple pages. To avoid copying and pasting the same code (which goes against best practice of writing DRY code), we can use a feature in EJS called *partials*. A common use case for partials is for components like navigation bars, which are typically consistent across different pages of an app.

The code for the navbar of an app is likely to remain the same regardless of what page the user is navigating to, so being able to write the code once and reuse it on each page is very handy!

> 📚 A *partial* is an EJS template designed to be included or embedded within other EJS templates.

To demonstrate the power and utility of partials, we will create and render a navbar partial for our application.

## Creating a partial template

Our partial will be used by different views in our application, so for code clarity, we will place this partial in its own subdirectory:

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
    <a href="/users/<%= user._id %>/applications">Your Apps</a>
    <a href="/auth/sign-out">Sign Out</a>
  <% } else { %>
    <a href="/auth/sign-in">Sign In</a>
    <a href="/auth/sign-up">Sign Up</a>
  <% } %>
</nav>
```

The above code applies conditional rendering based on the existence of a user. When setting up our `passUserToView` middleware, we either passed a user object or null into the locals object of each page. The logic above reads, that if there is no signed-in user, only the sign-in and sign-up links will be visible, otherwise the user will see a link to view all of their applications or sign out.

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
    <input type="text" name="username" id="username" required />
    <label for="password">Password:</label>
    <input type="password" name="password" id="password" required />
    <label for="confirmPassword">Confirm Password:</label>
    <input
      type="password"
      name="confirmPassword"
      id="confirmPassword"
      required
    />
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
    <input type="text" name="username" id="username" required />
    <label for="password">Password:</label>
    <input type="password" name="password" id="password" required />
    <button type="submit">Sign in</button>
  </form>
</body>
```

If all is working well, you should now see the links above your forms and know that your partial is rendering!

Now that we have the partial working for our `sign-up` and `sign-in` pages we can render it onto our site index page and the site homepage as well.

First, let's add the partial to our site's homepage:

```html
<!-- views/index.ejs -->

<body>
  <%- include('./partials/_navbar.ejs') %>
  <h1>Skyrockit</h1>
  <p>A job application tracker.</p>
  <a href="/auth/sign-up">Sign up</a>
  <a href="/auth/sign-in">Sign in</a>
</body>
```

The addition of this partial now makes all of the previous links on this page redundant. You can remove these links and let the partial do the work for you!

> 💡 Test it out and you can see the partial rendered in your main homepage now. If you want to test the conditional rendering, just create a test user and sign in. Once signed in, the page receives a user object, and you'll see the link to the applications page.

### Including the `nav` partial on `index.ejs`

Let's also add our `navbar` partial to our `applications` landing page:

```html
<!-- views/applications/index.ejs -->

<body>
  <%- include('../partials/_navbar.ejs') %>
  <h1>Your Apps</h1>
</body>
```

Refresh the page and you should see the navbar rendered above the `<h1>` element.
