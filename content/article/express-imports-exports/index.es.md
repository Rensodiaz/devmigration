---
title: "Imports (part 2)"
date: 2020-08-22T23:02:54-04:00
author: "Renso Diaz"
draft: false

tags: ['Express', 'Nodejs', ‘Exports’, ‘Imports’, ‘Javascript’]

noSummary: false

resizeImages: false
---
When it comes to developing a web application with Node.js **Imports** play a very important role in the structure of our application. They help us to structure and restructure the application any way that we want. But also gave us the ability to use modules written by other people and integrate them into our application. In Fact you already did create one and use it, in our **Part 2** lesson when we created environment variables, If you don’t remember you can always go back to read the article. 

One thing to get clear is that imports are strictly read only, no modification is allowed within the file that is importing it, if you need to modify the module that you are importing you will have to go inside that specific file and make the changes.


Now let’s see an import using **Javascript** and then we will see how does it change when using a framework like **Express**:

``` javascript
import myModule from "path/to/module-name";
```



Let’s see an **import** in Express and define each of the parts:
``` javascript
const config = require('./config');
```
We have been talking about imports but there is another important part that comes hand in hand when creating modules and that is **Exports**

To start with **Exports** let’s use the module we created in **Part 2** called **Config.js** 
``` javascript
module.exports = {
   ENV: process.env.NODE_ENV || 'development',
   PORT: process.env.PORT || 4040
}
```
A one thing to noticed from the code above is **module.exports**



