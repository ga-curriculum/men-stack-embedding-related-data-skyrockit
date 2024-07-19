# ![MEN Stack Embedding Related Data - Skyrockit - Setup](./assets/hero.png)

## Setup

Open your Terminal application and navigate to your `~/code/ga/lectures` directory:

```bash
cd ~/code/ga/lectures
```

## Cloning the Auth boilerplate

This lecture uses the [`MEN Stack Auth Template`](https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-session-auth-template) as starter code. Doing so allows us to have a connection established to our MongoDB Atlas, add functioning auth for our user model, and install some of the packages we will need for our app build.

Navigate to the [MEN Stack Auth Template](https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-session-auth-template) and clone the repository to your machine:

```bash
git clone https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-session-auth-template.git skyrockit
```

Note by adding the `skyrockit` argument we're cloning the specified repo into a directory called `skyrockit` on our machines.

Next, `cd` into the new directory:

```bash
cd skyrockit
```

Finally, remove the existing `.git` information from this template:

```bash
rm -rf .git
```

> 🚨 Removing the `.git` info is important as this is just a starter template provided by GA. You do not need the existing git history for this project.

## GitHub setup

To add this project to GitHub, initialize a new Git repository:

```bash
git init
git add .
git commit -m "init commit"
```

Make a new repository on [GitHub](https://github.com/) named `skyrockit`.

Link your local project to your remote GitHub repo:

```bash
git remote add origin https://github.com/<github-username>/skyrockit.git
git push origin main
```

> 🚨 Do not copy the above command. It will not work. Your GitHub username will replace `<github-username>` (including the `<` and `>`) in the URL above.

Open the project's directory in your code editor:

```bash
code .
```

## Install dependencies

Next, you will want to install all of the packages listed in `package.json`

```bash
npm i
```

## Create your `.gitignore`

Create a `.gitignore` file at the root of your project:

```bash
touch .gitignore
```

Once the file are created, add `.env`, `package-lock.json`, and `node_modules` to your `.gitignore` file. Doing so will prevent those files and directories from being tracked and we can be confident that any data we add there will not be pushed up to GitHub.

```plaintext
.env
node_modules
package-lock.json
```

## Create your `.env`

Lastly, we want to create `MONGODB_URI` and `SESSION_SECRET` to hold values used in our auth logic. `MONGODB_URI` will connect to your MongoDB Atlas connection string so you will need to establish one for this application. `SESSION_SECRET` will aid in your auth session logic.

Add a `.env` file to your application:

```bash
touch .env
```

Then add the following secret keys to the file:

```plaintext
MONGODB_URI=
SESSION_SECRET=
```

Provide appropriate values for each.

Start the server and you are ready for launch.

```bash
nodemon start
```

Happy coding!
