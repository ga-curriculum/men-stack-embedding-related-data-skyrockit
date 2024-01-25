# ![Job Application Tracker App - Setting the Stage](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to organize their user stories and build process prior to writing code for our Job Board application.

## What we're building - SkyRockit: The Ultimate Job Tracking App

We will be building an application where a user can track the status of various job applications that they have submitted.  Prior to the actual building of the application we need to establish the form of our "job application" data ( the schema ) and how it relates to our user ( we will chose the embedded data route ).

### Establish our User Stories

It is always a good idea to organize your thoughts around what you want your users to be able to do in your application.  A great strategy for laying the foundation of your app is creating user stories for each behavior a user could accomplish.  These user stories do not have to be technical in nature.  They also do not need to even be written as code.

If you would like a refresher on how to write some user stories you can [go here](https://git.generalassemb.ly/modular-curriculum-all-courses/javascript-browser-game-rock-paper-scissors/blob/main/pseudocode/README.md).

The following are some examples of user stories we can write for our Jobs Application Tracker.

```
As a user (AAU), I should be able to create job application for jobs that I anticipating applying for or have applied for. I should be able to keep track of the company name, the job title, and the status of the job application. Optionally I can add any notes I have about the job, and the URL to the job posting.

AAU, I should be able to view all the jobs that I have applied for on a single page. Only the job title and the company for each application should be listed here to keep this view digestable.

AAU, I should be able to view all the details of a job application on a new page by following a link from that index page.

AAU, when viewing the details of an application, I should be able to follow a link to be taken to a page where I can edit any of the details of a job application and update it from there.

AAU, when viewing the details of an application, I should be able to click a button and delete the application.
```

You might notice that the above user stories give us a good idea of what CRUD functions a user might want to perform on our applications.  Also, take note of how our first user story gives us some context as to how we might structure our data ("company name", "job title", "status", and optional "notes")  While these categories may not be the final names we give for data in our schema, they give us a high level guide we can refer back to when making decisions about how we structure data.

Let's go ahead and establish the RESTful routing for app:

| Action   | Route                                              | HTTP Verb  | 
|----------|----------------------------------------------------|------------|
| Index    | '/users/:userId/applications'                      | GET        |
| New      | '/users/:userId/applications/new'                  | GET        |
| Create   | '/users/:userId/applications'                      | POST       |
| Show     | '/users/:userId/applications/:applicationId'       | GET        |
| Edit     | '/users/:userId/applications/:applicationId/edit'  | GET        |
| Update   | '/users/:userId/applications/:applicationId'       | PUT        |
| Delete   | '/users/:userId/applications/:applicationId'       | DELETE     |

**Note:** We are including the `userId` in the route since we are nesting our applications within the user object.


<!-- tktk if it makes more sense to replace the above table with an asset that makes sense to me -->

### Create an ERD

Once we have our user stories established, we want to focus on the structure of our data.  When focusing on this task, we can create an ERD (Entity Relationship Diagram) that outlines what names our data will go by, what kind of datatypes said data will comprise of, and how our models will relate to each other.

An ERD can be used as a reference throughout the build of a project and can also be adjusted as your applications grow and become more complex.  Note that while the ERD is not actual code, it is critical for organizing an understanding of what your data should look like for yourself and other developers.

Below is an example of the ERD we will be using for our Job Application Tracker:

tktk here's a demo ERD, this should be replaced when assets are created, but it's a start:

![Job Applications ERD](./assets/tktk-erd-placeholder.png)

tktk Hunter, can you fix up this ERD so that it's happy?


### Embedding data

As a part of our ERD, you will note that we have chosen to create a relationship where applications are _embedded_ in the user model.  We are making this choice because it will reduce the amount of read operations needed to get the "application". The user will already be signed in and thus we will not need to search for other documents as they will be "embedded" in our user.

If you would like a refresher of an embedded relationship in Mongo, you can [look here](https://git.generalassemb.ly/modular-curriculum-all-courses/mongoose-relationships/tree/main/embedding).

Lastly, if you are interested in diving into the differences of embedding data vs referencing data in Mongo, you can do so in [this informative article](https://www.mongodb.com/developer/products/mongodb/mongodb-schema-design-best-practices/).