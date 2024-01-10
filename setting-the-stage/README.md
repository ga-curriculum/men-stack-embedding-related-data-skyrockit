# ![[tktk Module Name] - tktk Microlesson Name](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

tktk Notes: What're we building? Establish what this application is here. Discuss user stories (a quick reminder of user stories is necessary, we covered it previously in <https://git.generalassemb.ly/modular-curriculum-all-courses/javascript-browser-game-rock-paper-scissors/blob/main/pseudocode/README.md> but this is a the first time they've seen user stories in this unit). Go over an ERD here. We've already covered embedding a bit in <https://git.generalassemb.ly/modular-curriculum-all-courses/mongoose-relationships/tree/main/embedding>, but a quick reminder of the concepts covered there would be good. Could potentially reference patterns described in <https://www.mongodb.com/developer/products/mongodb/mongodb-schema-design-best-practices/>. Continue to reference user stories and the ERD throughout this lecture where appropriate.

tktk Here's some user stories to get going, feel free to flesh these out, they're just quick starting points:

tktk - As a user (AAU), I should be able to create job application for jobs that I anticipating applying for or have applied for. I should be able to keep track of the company name, the job title, and the status of the job application. Optionally I can add any notes I have about the job, and the URL to the job posting.

tktk - AAU, I should be able to view all the jobs that I have applied for on a single page. Only the job title and the company for each application should be listed here to keep this view digestable.

tktk - AAU, I should be able to view all the details of a job application on a new page by following a link from that index page.

tktk - AAU, when viewing the details of an application, I should be able to follow a link to be taken to a page where I can edit any of the details of a job application and update it from there.

tktk - AAU, when viewing the details of an application, I should be able to click a button and delete the application.

tktk it might also be good to draw student's attention to how with just these user stories we can get an idea of what the schema of the applications resource will look like, along with what CRUD actions users will be able to carry out on that resource.

tktk here's a demo ERD, this should be replaced when assets are created, but it's a start:

![Job Applications ERD](./assets/tktk-erd-placeholder.png)

tktk Hunter, can you fix up this ERD so that it's happy?

tktk Code: The final code for this lecture can be found at: <https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-embedding-related-data-job-applications-solution>
