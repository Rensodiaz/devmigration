---
title: "Imports (part 3)"
date: 2020-08-23T23:02:54-04:00
author: "Renso Diaz"
draft: false

tags: [Express, Nodejs, Exports, Imports, Javascript, NPM]

noSummary: false

categories: ['Articles']

resizeImages: false
---
When it comes to developing a web application with Node.js **Imports** play a very important role in the creation of our application. They help us to structure and restructure the application any way that we want. But also gave us the ability to use modules written by other people and integrate them into our application. 

In Fact you already did create one and use it. In our [**Part 2**](https://www.devmigration.com/article/express-file-structure/) lesson when we created environment variables. If you don’t remember you can always go back to read the article.

One thing to get clear is that imports are strictly read only, no modification is allow out of the scope of the module that **exports** it. If you need to modify the module that you are importing you will have to go inside that specific file and make the changes.


Now let’s see an import using **Javascript** and then we will see how does it change when using a framework like **Express**:

``` javascript
import myModule from "path/to/module-name";
```
Beside the **import** and **from** statements in the above code block there are two things that can make our code more readable:
   - **myModule**: Name of how you would like to identify your module in the current scope.
   - **module-name**: The actual name and location of your module.


Imports in Express.js are a bit different in syntax but the core function on how they operate are very similar to the example above. 
``` javascript
const config = require('./config');
```

As you can see we use a variable type **const**, if you have been following along you will remember what we said about import been strictly read only **const** are variables that won’t change after a declaration and value have been assigned to them.


### Exports
We have been talking about **Imports** but there is another important part that comes hand in hand when are creating modules and using them anywhere else in your code, that is call **Exports**.

To start with **Exports** let’s use the module we created in [**Part 2**](https://www.devmigration.com/article/express-file-structure/) called **Config.js** 
``` javascript
module.exports = {
   ENV: process.env.NODE_ENV || 'development',
   PORT: process.env.PORT || 4040
}
```
You can also use variables and obtain the same result like this: 
``` javascript
Const config = {
   ENV: process.env.NODE_ENV || 'development',
   PORT: process.env.PORT || 4040
}
module.exports = config
```


A one thing to noticed from the code above is **module.exports** we are letting the parent scope of our application that we have a module and that we would like to use it somewhere else in our application.

In our next lesson we will talk about **NPM** modules, we will see how to install and delete them. also we will see how we can import them into our application.



