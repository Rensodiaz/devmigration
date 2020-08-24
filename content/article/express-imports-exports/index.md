---
title: "Imports (part 3)"
date: 2020-08-22T23:02:54-04:00
author: "Renso Diaz"
draft: true

tags: ['Express', 'Nodejs', ‘Exports’, ‘Imports’, ‘Javascript’]

noSummary: false

resizeImages: false
---
When it comes to developing a web application with Node.js **Imports** play a very important role in the creation of our application. They help us to structure and restructure the application any way that we want. But also gave us the ability to use modules written by other people and integrate them into our application. 

In Fact you already did create one and use it, in our [**Part 2**](https://www.devmigration.com/article/express-file-structure/) lesson when we created environment variables. If you don’t remember you can always go back to read the article.

One thing to get clear is that imports are strictly read only, no modification is allow out of the scope of the module that **exports** it. If you need to modify the module that you are importing you will have to go inside that specific file and make the changes.


Now let’s see an import using **Javascript** and then we will see how does it change when using a framework like **Express**:

``` javascript
import myModule from "path/to/module-name";
```
Beside the **import** and **from** statement in the above code block there are two things that can make our code more readable:
   - **myModule**: name of how you would like to identify your module .
   - **module-name**: the actual name and location of your module.


Imports in Express.js are a bit different in syntax but the core function on how they operate are very similar to the example above. 
``` javascript
const config = require('./config');
```

As you can see we use a variable type **const** if you been follow along you will remember what we said about import been strictly read only **const** are variables that won’t change after a declaration and value have been assigned to them.


We have been talking about imports but there is another important part that comes hand in hand when creating modules and that is **Exports**

To start with **Exports** let’s use the module we created in **Part 2** called **Config.js** 
``` javascript
module.exports = {
   ENV: process.env.NODE_ENV || 'development',
   PORT: process.env.PORT || 4040
}
```
A one thing to noticed from the code above is **module.exports**



