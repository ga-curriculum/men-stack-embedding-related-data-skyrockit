<h1>
  <span class="headline">Skyrockit</span>
  <span class="subhead">Add Route Middleware</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to add custom middleware to check for a logged-in user and pass user data to views.

## Adding custom middleware

At the start of this project, we cloned a pre-built repository that included established authentication routes and logic. Now, we need to use this feature to make sure only users who are logged in can see certain parts of our app. This is known as _Authorization_.

We also need a way to share the user's data with the multiple views in the application. The [Express documentation](https://expressjs.com/en/api.html#res) provides a feature called `res.locals` for this purpose. It allows us to store data that can be easily accessed by our pages when they are rendered to the user.

Both of these tasks can be accomplished by creating some custom middleware that intercepts requests to our `applications` controller.

### Creating the directory and middleware

Let's begin by creating a middleware directory in the root of our project:

```bash
mkdir middleware
```

Create two new files here called `is-signed-in.js` and `pass-user-to-view.js`:

```bash
touch middleware/is-signed-in.js middleware/pass-user-to-view.js
```

### `isSignedIn`

When writing a custom middleware function, recall that we want three parameters instead of the usual two parameters our route handlers have been using:

- `req` is the request object.
- `res` is the response object.
- `next` is the third parameter, representing the `next` function in the long line of middleware and route handlers that a request is processed through.

This function's purpose is to check if a user is signed in and authorized to access certain routes or resources

Let's add the following to `is-signed-in.js`:

```javascript
// middleware/is-signed-in.js

const isSignedIn = (req, res, next) => {
  if (req.session.user) return next();
  res.redirect('/auth/sign-in');
};

module.exports = isSignedIn;
```

The function checks if there's a user object in the session (provided by `req.session.user`). This is typically used to confirm that a user is logged in.

If the user is logged in, `next()` is called, allowing the request to proceed to the next middleware or route handler. If this check fails, however, it moves to redirect the user to the sign-in page, strongly suggesting to the user that, to get where they want to go, they'll have to sign-in.

### `passUserToView`

Inside of the `pass-user-to-view.js` file, we will add the following logic to add the user to the `res.locals` object.

```javascript
// middleware/pass-user-to-view.js

const passUserToView = (req, res, next) => {
  res.locals.user = req.session.user ? req.session.user : null;
  next();
};

module.exports = passUserToView;
```

In the above code, we use a ternary operator specifying that if a user exists (in `req.session.user`) then set the value of `res.locals.user` to that user. Otherwise, we will set the value of `res.locals.user` to null. `next()` then calls the next function in our route handling sequence.

This middleware provides us a shortcut to always pass the information of the logged in user to our requests final destination.

## Import and mount custom middleware

Now that we have our middleware created, we need to mount and use it in our server. Import the middleware just below our other dependencies at the top of `server.js`:

```javascript
// server.js

const morgan = require('morgan');
const session = require('express-session');

// require our new middleware!
const isSignedIn = require('./middleware/is-signed-in.js');
const passUserToView = require('./middleware/pass-user-to-view.js');
```

For this application, users must be signed in to view any of the routes associated with their applications. Therefore, `isSignedIn` should be placed above the `applications` controller, but not before auth.

The `passUserToView` middleware should be included before all our routes, including our homepage, just in case we want to include conditional rendering with a user's details. If there is no signed in user the locals object will be set to null.

```javascript
// server.js

app.use(express.urlencoded({ extended: false }));
app.use(methodOverride('_method'));
// app.use(morgan('dev'));
app.use(
  session({
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: true,
  })
);

app.use(passUserToView); // use new passUserToView middleware here

app.get('/', (req, res) => {
  res.render('index.ejs', {
    user: req.session.user,
  });
});

app.use('/auth', authController);
app.use(isSignedIn); // use new isSignedIn middleware here
```

Sign into your app then navigate to the home page. If you are signed in, you should see a log in your console containing all of the information for the signed in user!
