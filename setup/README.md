# ![Job Application Tracker App - Setup](./assets/herotktk.png)

## Cloning our Auth repository

We will begin our Job Application build by cloning down the repository that holds our previously built Auth logic.  Doing so will allow us to have a connection established to our MongoDB Atlas, add functioning auth for our user model, and install some of the packages we will need for our app build.

First, we will want to move into your `lectures` directory on your computer

```bash
cd ~/code/ga/lectures/
```

Navigate to [this link](https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-session-auth-template) and clone the repository to your machine:

```bash
git clone git@git.generalassemb.ly:modular-curriculum-all-courses/men-stack-session-auth-template.git
```

Once we have the repository on our machines, we can change the name of the directory to better reflect a job tracking app meant to help us ascend to the highest stratospheres of our career:  Skyrockit!

```bash
mv men-stack-session-auth-template skyrockit
```

Change into the directory we cloned down and open VSCode from there:

```bash
cd skyrockit
code .
```

Next, you will want to install all of the packages listed in `package.json`

```bash
npm i
```

After succesfully install the packages, let us create a `.env` file and `.gitignore` so we can safely house our sensitive connection data and have it not tracked by git.

```bash
touch .gitignore .env
```

Once these files are created, add `.env`, `package-lock.json`, and `node_modules` to your `.gitignore` file.  Doing so will prevent those files and directories from being tracked and we can be confident that any data we add there will not be pushed up to GitHub.

Lastly, we want to create `MONGODB_URI` and `SESSION_SECRET` to hold values used in our auth logic.  `MONGODB_URI` will connect to your MongoDB Atlas connection string so you will need to establish one for this application.  `SESSION_SECRET` will aid in your auth session logic.

Let's ensure that all of our logic is correct and we can start our server.

```bash
nodemon start
```

Assuming all things are working you should see no errors and output that looks like the following:

```bash
[nodemon] 3.0.3
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node start server.js`
The express app is ready on port 3000!
Connected to MongoDB final-mern-app.
```

Excellent work!!  We are ready to set the stage for how to move forward with creating this application.
