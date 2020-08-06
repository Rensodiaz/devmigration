---
title: "Express for biginners (part 1)"
date: 2020-07-30T23:02:54-04:00
author: "Renso Diaz"
draft: false

tags: ['express', 'Nodejs', 'Pug', 'Ejs', 'JWT', 'Mongodb', 'postgreSQL']

noSummary: false

resizeImages: false
---


Welcome to the first part of our journey, this is going to be a small series with multiple posts to help you dive right into [**Express**](https://expressjs.com/) framework and [**Node.js**](https://nodejs.org/en/). Don’t worry if you don’t have any previous experience with [**Express**](https://expressjs.com/) or [**Node.js**](https://nodejs.org/en/). This tutorial is designed with that in mind and will try to be as clear as possible. We will cover the backend logic only in this series and in the future we may also create another small tutorial on how to integrate a view engine like [**Pug**](https://pugjs.org/api/getting-started.html), [**Ejs**](https://ejs.co/), etc.

Some of the topics that we are going to cover in this series are the following:
```
   - Express file structure.
   - Configuration of multiple environments.
   - Imports.
   - Npm Modules.
   - Middlewares.
   - Routes.
   - Database implementation MongoDB and PostgreSQL
   - JWT authentication.
```

For our first lesson we will see the requirements needed, followed by our very first commands to create a hello world.


**About Express**

[**Express**](https://expressjs.com/) is a light-weight web application framework to help organize your web application into a **MVC**(Model View Controller) architecture on the server side and at the same time make the process of developing a web application very fast and less time consuming.

It also give us the ability to integrate databases like [**PostgresSQL**](https://www.postgresql.org/) and [**MongoDB**](https://www.mongodb.com/)
to provide a backend for your Node.js application. Express helps you manage everything, from routes to handling requests and views.


**Requirement**

Before we can start creating our very first [**Express**](https://expressjs.com/) application there is one thing that needs to be installed in our system.

Install [`Nodejs`](https://nodejs.org/en/).


**First application**

Create a folder with the name of your application and navigate to it:
```
mkdir mywebapp
cd mywebapp
```
Create your entry file, you can call this file whatever you like:
```
app.js or index.js
```
Initialize your application using the npm(node package manager), on the entry file option select the name of the file from the step above.
```
npm init
```

**Install express**
```
npm install express --save
```

**Using Express in our application**

Import express module, this import should be in the entry file we created a few steps back:
```
const express = require(‘express’);
```
After the [**Express**](https://expressjs.com/) module is import instantiate it by creating your application server variable:
```
const app = express();
```
Create the port to where the application is going to communicate with:
```
const port = 4040;
```
Let’s now create our first route, we will go deeper into this subject later on in this series:
```
app.get(‘/’, (req, res)=>{
   res.send(“Welcome to the world!”);
});
```
A couple of things to notice here are the parameters **req**(request), **res**(response) and arrow function (**=>**)

And now let’s start listening for get request in our application:
```
app.listen(port, ()=>{
   console.log(`Listening in port: ${port}`)
});
```

Run our application server:
```
npm start
```

Now you can open your browser or use another application like [**Postman**](https://www.postman.com/) to do our get request at:
```
http://localhost:4040
```
**Conclusion**

This is the bare minimum to run a [**Node.js**](https://nodejs.org/en/) application with[**Express**](https://expressjs.com/). In the next lesson we will talk about the file structure and setting multiple environments.


