# ![Job Application Tracker App - Add Route Middleware](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to add middleware to be used in conjuction with their routing logic.

## Adding Route Middleware for our Application Routes

To begin this project, we cloned a repository that already had auth routes and logic established.  This is a wonderful start but we are at the stage where we need to add middleware to be used with our application routes.  The middleware we are adding will protect these `application` routes from being accessed by visitors who are not logged in.  It will also add the `user` to a `res.locals` object that can be accessed by our view templates.

Per the [Express documentation](https://expressjs.com/en/api.html#res) on `res.locals`:  Use this property to set variables accessable in templates rendered with `res.render`.

### Creating the folder and middleware

Let's begin the process of adding middleware by creating a middleware folder in the root of our project and adding the file that will house our logic:

```bash
mkdir middleware
touch middleware/pass-user-to-view.js
```

Inside of the `pass-user-to-view.js` file, we will add the middleware logic to add the user to the `res.locals` object.

`middleware/pass-user-to-view.js`
```javascript
const passUserToView = (req, res, next) => {
  res.locals.user = req.session.user ? req.session.user : null
  next()
}

module.exports = passUserToView
```

In the above code, via a terinary operator, we are saying if a user exists ( in `req.session.user`) set the value of `res.locals.user` to that user.  Otherwise, we will set the value of `res.locals.user` to null.  `next()` calls the next function in our route handling sequence.

### Mount and use the middleware in `server.js`

Now that we have our middleware created we need to mount it and use it in `server.js`.  To do so, let us import the middleware immediately after we mount our dependencies:

`server.js`
```javascript
const passUserToView = require('./middleware/pass-user-to-view.js');
```

Once that is in place we can "use" the middleware before our route logic like so:

`server.js`
```javascript
app.use(passUserToView);
```

We now have the middleware in place to move forward!