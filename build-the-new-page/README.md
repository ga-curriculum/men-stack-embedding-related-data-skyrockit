# ![MEN Stack Embedding Related Data - Skyrockit - Build the New Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create a form for adding new job applications, understand how to route this form using RESTful conventions and learn the principles of aligning form inputs with a data schema.

At this stage, we have an index page, but no applications to display. Before we can add application data to our database, we need a form to collect this information from our user. 

## Conceptualizing the route

Let's review our first user story: 

> As a user, I want to be able to add new job applications that I'm thinking about applying to or have already applied to. For each job, I should be able to note down important stuff like the company's name, the job title, what stage the application is at, and if I want, some personal notes and the link to the job posting.

To make this possible, we need to establish a `new` route. This route will lead users to a form where they can input all the necessary information about a new job application. According to [RESTful Routing](https://git.generalassemb.ly/modular-curriculum-all-courses/restful-routing/blob/main/routing-conventions/README.md) Conventions, the route for displaying a `new.ejs` view, containing a form for a new application, is defined as:

```plaintext
GET /users/:userId/applications/new
```

Before we move forward, a quick reminder that some CRUD operations only require a single request, such as a `GET` request for a `show` page. However, operations like creating or updating data typically involve two distinct steps:

1. *Initial request for form:* The first step involves a request that results in a page being displayed. This page contains a form designed to gather the required data from the user.
2. *Submitting the Form:* After the user fills out and submits the form, a second request takes place. This request sends the user's data to the server, where it's either added as new data to the database (Create) or used to update existing data (Update).

Below, we'll tackle the first half of the `New` + `Create` process (a `GET` request to a view displaying a form), and in the next lesson we'll handle what happens when that form is submitted.


## Defining the route on the server

In `controllers/applications.js`, create the `new` route. This route should `res.render()` a `new.ejs` view:

```js
// controllers/applications.js

router.get('/new', async (req, res) => {
  res.render('applications/new.ejs');
});
```

## Building the UI

Next, it's time to build our view. To submit user input, we'll need to create a page with a form element.

In `views/applications`, create a `new.ejs` file and add HTML boilerplate.  

Change the title to "Add a New Application", and give the page a matching header. Lastly, add the navbar partial at the top of the `<body>`: 

```html
<!-- views/applications/new.ejs -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Add a New App</title>
</head>
  <body>
    <%- include('../partials/_navbar.ejs') %>
    <h1>Add a New App</h1>
  </body>
</html>
```

Time to test, but how do we get to our new page? 

We need a link on our `applications` index page that will take us to our `new` view.

```html
<!-- views/applications/index.ejs -->

<body>
  <h1>Your Apps</h1>
  <a href="/users/<%=user._id%>/applications/new">Add an App</a>
</body>
</html>
```

The link for this route should match our defined route above. Sign in and test it out! 


## Designing the form

Now, let's focus on constructing our form, keeping our user story in mind. This form is intended to allow users to input details about job applications including the **company name**, **job title**, and the application's **status**. Additionally, it should provide space for **notes** and a **link** to the job posting.

It's essential to align our form design with the structure outlined in our data schema:

```js
// models/user.js

const applicationSchema = mongoose.Schema({
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

When designing the form based on this schema, here's what we should consider:

- **Single Line Text Inputs**: For `company`, `title`, and `postingLink`, use standard text inputs. These fields typically require short, concise text.

- **TextArea for Notes**: The notes field is likely to contain more detailed information. Therefore, use a `<textarea>` element, giving users ample space to write their notes.

- **Dropdown for Status:** The status field has pre-defined, *enumerable* options. To enhance user experience and prevent errors, use a `<select>` dropdown. This way, users can only choose from the valid options listed in the `enum` array. This approach eliminates guesswork for users and gives us predictable data.

## Building the form

In `new.ejs`, create a new form element. For now, we will leave the `action` and `method` attributes empty:

```html
<!-- views/applications/new.ejs -->

  <form action="" method="">
  </form>
```

Next, add input fields to match each item in our data schema. Each input should have a corresponding label for accessibility. 

```html
<!-- views/applications/new.ejs -->

<form action="" method="">
  <label for="company">Company:</label>
  <input type="text" name="company" id="company">
  
  <label for="title">Title:</label>
  <input type="text" name="title" id="title">
  
  <label for="notes">Notes:</label>
  <textarea name="notes" id="notes"></textarea>
  
  <label for="postingLink">Posting Link:</label>
  <input type="text" name="postingLink" id="postingLink">
  
  <label for="status">Status:</label>
  <select id="status" name="status">
    <option value="interested">Interested</option>
    <option value="applied">Applied</option>
    <option value="interviewing">Interviewing</option>
    <option value="rejected">Rejected</option>
    <option value="accepted">Accepted</option>
  </select>
</form>
```

### Shaping data

It's important to remember that when a form is submitted, each input's `name` attribute becomes the key in a key-value pair. The value for this key is what the user enters into the form. Therefore, the `name` attribute of each input field needs to match exactly with the corresponding property name in our data schema.

For example, in our schema, we have a company field defined like this:

```js
company: {
    type: String,
    required: true,
  }
```

To ensure proper data mapping when the form is submitted, the corresponding input in our HTML form should be set up as follows:

```html
<label for="company">Company:</label>
<input type="text" name="company" id="company">
```

> The `name` attribute of the input (`name="company"`) exactly matches the property name in the schema (`company:`).


Also, note that the `value` attribute for each `<option>` in our `<select>` dropdown exactly matches one of the items in the `enum` array on our schema: 

**Schema:**
```js
status: {
    type: String,
    enum: ['interested', 'applied', 'interviewing', 'rejected', 'accepted'],
  }
```

**Form:**
```html
<select id="status" name="status">
      <option value="interested">Interested</option>
      <option value="applied">Applied</option>
      <option value="interviewing">Interviewing</option>
      <option value="rejected">Rejected</option>
      <option value="accepted">Accepted</option>
    </select>
```

Similar to how the rest of the inputs work, when the form is submitted the `name` of the `<select>` input will be used as a key, and the `value` of the selected option will be submitted as the value. If a user were to select "Applied" from the dropdown, the resulting key:value pair would look like this: 

```json
{ "status": "applied" }
```

This is why the `value` must match an `enum` element exactly.


## Submitting the form

To complete our form, the last element we need is a submit button.

```html
<!-- views/applications/new.ejs -->

<form action="" method="">
  <label for="company">Company:</label>
  <input type="text" name="company" id="company">
  
  <label for="title">Title:</label>
  <input type="text" name="title" id="title">
  
  <label for="notes">Notes:</label>
  <textarea name="notes" id="notes"></textarea>
  
  <label for="postingLink">Posting Link:</label>
  <input type="text" name="postingLink" id="postingLink">
  
  <label for="status">Status:</label>
  <select id="status" name="status">
    <option value="interested">Interested</option>
    <option value="applied">Applied</option>
    <option value="interviewing">Interviewing</option>
    <option value="rejected">Rejected</option>
    <option value="accepted">Accepted</option>
  </select>

  <!-- Add a submit button -->
  <button type="submit">Add Application</button>
</form>
```

Fantastic - next, we're ready to build the `create` functionality and give this form somewhere to submit to! 
We'll be coming back to update the `action` and `method` on this form, but otherwise our `new` page is finished.