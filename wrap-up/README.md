# ![MEN Stack Embedding Related Data - Skyrockit - Wrap Up](./assets/hero.png)

Learning Objective: Students will gain a comprehensive understanding of the Skyrockit app and its functionalities.

## Overview of Skyrockit

As we wrap up the Skyrockit project, let's reflect on our journey and the architecture of the application we've developed.

### Understanding CRUD and user stories

Our starting point was the user stories, which outlined the need for CRUD functionalities in the app. These included creating, viewing, updating, and deleting job applications - all essential actions for a user-centered job application tracker.

### Data architecture and user-application relationship

Our app was designed to have individual users, each with their own set of job applications. These applications are unique to each user, emphasizing a one-to-many relationship: a single User is linked to multiple Applications, but each Application is specifically tied to one User. To efficiently manage this relationship, we chose to embed the Application data directly within the User data.

This embedding means that each User document in our database contains its own collection of Application sub-documents. The advantage of this approach is clear: we can retrieve all of a User's Applications with just one database query, enhancing efficiency and performance. Moreover, this structure ensures data privacy and integrity, as users can only access and interact with the Applications they have created, maintaining a distinct separation or 'silo' for each user within the application.

### Navigating with `req.session.user`

In a user-centric application, where data (like job applications) is closely tied to individual user accounts, it's essential to constantly identify the active user. This ensures that the data being accessed, modified, or deleted is correct and specific to the logged-in user. The use of `req.session.user` effectively maintains user state across different requests, which is a fundamental aspect of personalized web services.

The custom middleware that transfers `req.session.user` to `req.locals.user` simplifies the process of passing the current user's data to views. This design pattern enhances security and user experience by ensuring that actions and data are relevant to the currently logged-in user, thus preventing unauthorized access or modification of other users' data.

### Implementing CRUD functionality in 5 repeatable steps

1. We began by determining the appropriate RESTful route for each functionality.
2. Then, we created the UI elements to initiate the corresponding HTTP requests.
3. Each route was defined in our controller module to handle these requests.
4. Controller actions were then implemented to carry out the necessary CRUD operations.
5. For `GET` requests, we rendered view templates, passing along the necessary data. This involved creating and coding view templates where needed. For operations that altered data (`POST`, `PUT`, `DELETE`), we implemented redirects to relevant routes, ensuring a seamless user experience.

This approach allowed us to systematically build each feature of Skyrockit, ensuring it met both our initial user stories and the functionality required for an effective job application tracking system.
