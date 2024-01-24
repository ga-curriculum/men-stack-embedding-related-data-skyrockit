# ![[tktk Module Name] - Wrap Up](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will have a big picture understanding of the Skyrockit app. 

## In summary

Now that we have a finished app, let's take a step back and look at what we built! 

We started with User Stories that essentially required CRUD functionality for our app: users should be able to create a job application, view both an index of all applications and also read all of the application info, update an application, and delete an application. 

With those requirements established, we then had to make a decision about the architecture of our data. We knew that our app would have users, and that all of the applications created by a user would only be visible to them. In other words, a `User` can have many `Applications`, but an `Application` belongs to a single `User`. Since this is a classic example of a one to many (1:n) relationship, embedding the applications inside the user made sense - the parent document (the User) can hold all of the associated sub-documents (Applications), and we only need to make a single read operation to view all of the Applications on a User! This also means that a user can only view applications they created - each `User` in our app is essentially 'siloed' off on their own. 

With those decisions made, we could start working on our user stories by following a multi-step process for implementing CRUD functionality: 

- Determine the proper route (using RESTful routing conventions)
- Add the UI that triggers the HTTP request that matches the route
- Define the route in the router module that matches the HTTP request
- Add the controller action/method that will handle the request, and perform the necessary CRUD action
- In the case of a `GET` request, `render` a view template and pass it data. If it doesn't exist yet, code the view template. 
- If data has been mutated (via a `POST`, `PUT`, or `DELETE`) then `redirect` to another route. 

Some CRUD operations require a single request, such as a `GET` request for an index route. Others, such as Create or Update, will be a two step process: 

1. The first request displays a form for the user to enter the data
2. A second request submits the form to the server where the data is created or updated


## req.session.user


Review `req.session.user` now that they've had a chance to see it in action.


