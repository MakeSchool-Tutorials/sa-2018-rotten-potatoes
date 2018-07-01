---
title: "MongoDB Setup"
slug: mongodb-setup
---

The database is where we'll store and organize all of our web application's data. Whenever a new movie review is written, it's information will be saved in the database. If another user wants to see that review, our web app retrieves the resource from the data to display it as a view.

Our web app will constantly add (write) and retrieve (read) data to and from our database. In our web app, we'll be using MongoDB.

# What is MongoDB?

MongoDB is a popular open-source database that stores your data as JSON-like data structures. You'll commonly hear this type of database referred to as a NoSQL database, which means that it's a non-SQL database.

> [info]
>
If you're not familiar with SQL databases, don't worry. SQL databases are another type of database, sometimes referred to as relational database, that store data in tables.

MongoDB is very popular within the JavaScript ecosystem for it's ease of use and ability to [quickly add or modify data in it](https://www.mongodb.com/blog/post/why-mongodb-popular). To setup our database, we'll use a service named [mLab](https://mlab.com/).

# Setting Up MongoDB with mLab

[mLab](https://mlab.com/) is a database-as-a-service that we'll use to setup our MongoDB database. To start, we'll need to create an account.

> [action]
>
1. Go to the [mLab website](https://mlab.com/) and create a new account by signing up. If you already have an account, you can skip this step and log in instead. ![Sign Up](assets/sign_up.jpg)
1. Fill out the form to create a new account. Make sure you provide an email that you can verify as you'll need to in the next step. ![Create Account](assets/create_account.jpg)
1. Verify your account by opening the verification email and clicking the link provided.
1. Create a new _MongoDB Deployment_ by clicking the _Create New_ button in the top-right corner. ![Create New Database](assets/create_new_db.jpg)
1. Set up your MongoDB configurations so that your _Cloud Provider_ is `Amazon Web Services` and your _Plan Type_ is `Sandbox`. When you're done, hit the _Continue_ button on the bottom right corner. ![Database Configurations](assets/db_configurations.jpg)
1. Set your AWS region to `US East (Virginia) (us-east-1)` and hit the _Continue_ button. ![AWS Region](assets/aws_region.jpg)
1. Name your database `rotten-potatoes` and hit _Continue_. ![Name Database](assets/name_db.jpg)
1. Finally, review and confirm your _order_ to setup your new database. ![Database Confirm](assets/db_confirm.jpg)

Great! You've created a new MongoDB database. We'll use this for our Rotten Potatoes web app.

![New Database](assets/new_db.jpg)

## Creating a Database User

To use and access our database, we'll need to do one more thing: create a database user.

> [action]
>
Click on our `rotten-potatoes` database. You should see the following: ![Database View](assets/db_view.jpg)

Notice that mLab even displays a warning that we'll need to create a user before we can use our new database.

![No User Warning](assets/no_user_warning.jpg)

Without further ado, let's create our _Database User_.

> [action]
>
Create a _Database User_ for your database:
>
1. Switch to the _Users_ tab of your database. ![Users Tab](assets/users_tab.jpg)
1. On the _Users_ tab, click the _Add database user_ button. ![Add DB User](assets/add_db_user.jpg)
1. Enter a username (i.e. `ms-user`) and password (i.e `makeschool`) of your choice. Make sure you write your credidentials down. You'll need this for later! When you're finished, hit _Create_. ![New User Form](assets/new_user_form.jpg)

When you're done, you should see our new database user and the previous warning will be gone.

![New User Success](assets/new_user_success.jpg)

With our new _database user_, we can use the credidentials to connect our _Express_ app.

# Connecting Express to MongoDB

Our database is set up on mLab's server, however we'll still need to do some setup to connect the wires between our web app and our database.

First, we'll start by installing some MongoDB-related packages with NPM. Specifically, we're going to need to install the MongoDB driver and Mongoose.

> [action]
>
Using NPM, install both the MongoDB and Mongoose packages:
>
1. Open terminal or your equivalent command-line tool.
1. Install `mongodb` with the following command `npm install mongodb --save`.
1. Install `mongoose` with the following command `npm install mongoose --save`.

## Configuring app.js

Almost done with our database setup! Last, we'll need to add some code to our `app.js` file that connects our web app with our mLab's database.

> [action]
>
In `app.js`, add the following code near the bottom of the file, right above the `module.exports` line:
>
```JavaScript
//...
>
const mongoose = require('mongoose');
const mongoURI = '(your mongodb URI)';

// connect our app with our database
mongoose.connect(mongoURI)
mongoose.Promise = global.Promise;
let db = mongoose.connection;
db.on('error', console.error.bind(console, 'MongoDB connection error:'));
>
module.exports = app;
```

To finish, we'll need to replace the placeholder string for our constant `mongoURI` with our database's actually URI. We'll need to go back to mLab to finish up.

Back in your mLab's `rotten-potatoes` database, you'll notice the following message (if you're not inside your `rotten-potatoes` database overview, navigate there now).

![Database URI](assets/database_uri.jpg)

We'll need to replace the `mongoURI` constant in our `app.js` file with the URI provided by mLab. In the example above, our URI is `mongodb://<dbuser>:<dbpassword>@ds253879.mlab.com:53879/rotten-potatoes`. However, you'll notice that URI contains placeholders for our credentials (`<dbuser>` and `<dbpassword>`).

We'll need to replace `<dbuser>` and `<dbpassword>` with our _database user's_ username and password respectively. For example, if your _database user_ had a password of `ms-user` and a password of `makeschool`, your database URI would be the following:

```
mongodb://ms-user:makeschool@ds253879.mlab.com:53879/rotten-potatoes
```

When you've got your database URI, replace the `mongoURI` placeholder.

[action]
> 
In `app.js`, replace your `mongoURI` with your mLab database URI that contains your credentials:
>
```
// ...
>
const mongoose = require('mongoose');
const mongoURI = 'mongodb://ms-user:makeschool@ds253879.mlab.com:53879/rotten-potatoes';
>
// ...
```

And with that last change, we've successfully setup our database and connected it to our web app. This prepares us to start implementing CRUD operations in our REST routes.

# Up Next

We've setup the database, but haven't used it. In the next section, we'll create our first resource and start writing data to our database.
