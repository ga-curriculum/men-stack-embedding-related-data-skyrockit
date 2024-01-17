# ![[tktk Module Name] - Build the Show Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

tktk Notes: Just a show page here, nothing too crazy! Might be good to draw attention to the conditional rendering used here since some of the fields are optional on creation.

Now that we have an index view, let's look at another User Story: 

> AAU, I should be able to view all the details of a job application on a new page by following a link from that index page.

Currently, our index only shows the title and company, but each application in our database actually contains a lot more data than that! In order to see all of the data, and fulfil our user story, let's build a `show` view that we can link to from our index. 

To start, let's go to `views/applications/index.ejs` and set up a link using the proper routing convention: 

```html
<ul>
    <% applications.forEach((application)=>{ %>
      <li>
        <!-- wrap the title and company in a link -->
        <a href="/users/<%= user._id %>/applications/<%= application._id %>">
            <%= application.title %> at <%= application.company %>
        </a>
      </li>
    <% }) %>
  </ul>
```

