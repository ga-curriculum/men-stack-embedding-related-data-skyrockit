# ![Job Application Tracker App - Build and Use the Applicaitons Controller](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will have added the "application" schema to the app and thus set the stage for building CRUD functionality into the job tracker.

## Use the ERD as our guide.

We have established an ERD for our data and also decided to embed our application in the user model. Deciding to create an embedded relationship means that we will not need to create another file and import or export it's logic to other portions of our application.  Instead, we will define our schema logic in the same file that our user schema is located in and create the connection there.

Let us begin that process by defining our schema and also associate it with the user.

In the `models/users.js`, we'll add the following logic:

```javascript
const applicationSchema = new mongoose.Schema({
  // key/value pairs to be added 
});

const userSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
  },
  password: {
    type: String,
    required: true,
  },
  applications: [applicationSchema], // this is where we reference the applicationSchema
});
```

Take note of the `applications` key located in our `userSchema`.  The value there is a reference to the `userSchema` we are defining just above it.  In pointing to the `applicationSchema` and nesting it in a array literal, we are ensuring that a user can create and track multiple applications.  

We have also opted to only define the application schema and make sure we reference it prior to defining the shape of our applications.

### Define the shape of our Applications

Now that we have defined our application schema let us think back to our first user story and our ERD.

Our first user story is the following:

```
As a user (AAU), I should be able to create job application for jobs that I anticipating applying for or have applied for. I should be able to keep track of the company name, the job title, and the status of the job application. Optionally I can add any notes I have about the job, and the URL to the job posting.
```

The following story let's us know that an application created by a user will have at least a "company name", "job title", "status", and "notes".  These are values that user can at least create and are something we note in our ERD.

Speaking of which, let's revisit that ERD:

![Job Applications ERD](./assets/tktk-erd-placeholder.png)

Our ERD let's us know that our "company" and "title" are the only value require while everything else will be optional.  Also, while an `_id` is listed in the ERD we do not need to write this logic as Mongo will create one automatically when the document is created.

With this knowledge in hand, we can return to the application schema and add the fields and types needed:

`models/users.js`
```javascript
const applicationSchema = new mongoose.Schema({
  company: {
    type: String,
    required: true,
  },
  title: {
    type: String,
    required: true,
  },
  notes: {
    type: String,
  },
  postingLink: {
    type: String,
  },
  status: {
    type: String,
    enum: ['interested', 'applied', 'interviewing', 'rejected', 'accepted'],
  },
});
```

We have now added the key/value pairs needed for us to allow the user to create "applications" to track.