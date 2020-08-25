---
title: "Imports (parte 3)"
date: 2020-08-23T23:02:54-04:00
author: "Renso Diaz"
draft: false

tags: [Express, Nodejs, Exports, Imports, Javascript, NPM]

noSummary: false

categories: ['Articulos']

resizeImages: false
---
Cuando hablamos sobre el desarrollo de aplicaciones web con Node.js los **Imports** juegan una parte bien importante al momento de crear nuestra aplicación. Los **Imports** nos ayudan a la estructuración y reestructuración de nuestra aplicación en cualquier forma que deseemos. Pero también nos ofrecen la facilidad de usar módulos creados por otras personas a la misma vez que los nuestros.

Para estar más claros, ya hemos creado y usado un **Import** que nosotros mismo en la lección pasada cuando creamos variables de entorno. [**Parte 2**](https://www.devmigration.com/article/express-file-structure/) puedes volver y leer el artículo todas las veces que desees.

Una cosa bien importante es que los **Import** son estrictamente de lectura, ningun tipo de modificación es permitido fuera del entorno del módulo que lo **Exporta**. Si necesitas modificar el módulo que quieres usar puedes abrir el archivo y modificarlo directamente.

Vamos a ver cómo se usan los imports con **Javascript** y luego como cambia al usar un framework como **Express**:

``` javascript
import myModule from "path/to/module-name";
```

Aparte de **import** and **from** en el ejemplo de arriba tenemos dos propiedades más que ayudan al entendimiento del código:
   - **myModule**: Nombre de como quieres identificar tu módulo en el entorno en que estas.
   - **module-name**: Localización y nombre donde está tu módulo.

Imports en Express.js son un poco diferente en la sintaxis pero sus funcionalidades son las misma:
``` javascript
const config = require('./config');
```

Como podemos observar en Express.js usamos una variable tipo **const**, este tipo de variables no se pueden modificar una vez que su declaración se ha completado. Por lo tanto nuestra restricción de que los **Imports** son de modo lectura solamente sigue en pie.


### Exports
Nosotros hemos hablado sobre **Imports** pero otro punto muy importante que va de la mano con los imports son los **Exports**. Estos son los que nos ayudan a exportar nuestros módulos en nuestra aplicación.

Para empezar con **Exports** usaremos el módulo que creamos en nuestra sesión anterior [**Parte 2**](https://www.devmigration.com/article/express-file-structure/) llamado **Config.js** 
``` javascript
module.exports = {
   ENV: process.env.NODE_ENV || 'development',
   PORT: process.env.PORT || 4040
}
```
También podemos usar variables y obtener el mismo resultado.
``` javascript
Const config = {
   ENV: process.env.NODE_ENV || 'development',
   PORT: process.env.PORT || 4040
}
module.exports = config
```

Una de las cosas que podemos notar en el código de arriba es **module.exports**, con esta propiedad le dejamos saber al entorno padre que queremos usar este módulo en otros archivos y así él mantiene una referencia de cuáles módulos se pueden usar.

En nuestra próxima lección hablaremos sobre módulos **NPM**, Donde veremos como se instalan y remueven módulos. También podremos ver como usar imports de los mismos módulos.




