---
title: "Express para principiantes (parte 1)"
date: 2020-07-30T23:02:54-04:00
author: "Renso Diaz"
draft: false

tags: ['express', 'Nodejs', 'Pug', 'Ejs', 'JWT', 'Mongodb', 'postgreSQL']

noSummary: false

resizeImages: false
---


Bienvenido a la primera parte de nuestra jornada, este capítulo inicial es el primero de una serie, en la cual trataremos y abundaremos sobre [**Express**](https://expressjs.com/) framework y [**Node.js**](https://nodejs.org/en/).
Si no tienes mucho conocimiento sobre [**Express**](https://expressjs.com/) framework o [**Node.js**](https://nodejs.org/en/) no te preocupes esta serie se diseñó tomando en cuenta esas preocupaciones. En esta serie cubriremos muchos de los aspectos del backend solamente. En el futuro trataremos de crear otros tutoriales para enseñar cómo trabajar express y sus soportes para manejadores de vistas como, [**Pug**](https://pugjs.org/api/getting-started.html), [**Ejs**](https://ejs.co/), etc.

Algunos de los temas que estaremos cubriendo son los siguientes:
```
   - Estructura de archivos de Express.
   - Configuraciones de varios entornos de desarrollo.
   - Importaciones.
   - Modulos de NPM.
   - Mediadores.
   - Rutas.
   - Implementación de base de datos MongoDB y PostgreSQL
   - Autentificación usando JWT.
```

En esta primera parte abordaremos requerimientos, seguido por la creación de nuestra primera aplicación usando nuestro marco de referencia [**Express**](https://expressjs.com/).

**Sobre Express**

 [**Express**](https://expressjs.com/) es un marco de referencia para la creación de aplicaciones web, el cual nos ayuda a organizar nuestra aplicación haciendo uso del patrón de diseño MVC(Modelo Vista Controladores) del lado del servidor. Al mismo tiempo hace que el proceso de desarrollo sea bastante rápido y a la misma vez se invierta el mínimo de tiempo.

Otra fortaleza de utilizar  [**Express**](https://expressjs.com/) es la habilidad para integrar base de datos como [**PostgresSQL**](https://www.postgresql.org/) y [**MongoDB**](https://www.mongodb.com/) el cual proveer un backend para tus aplicaciones en [**Node.js**](https://nodejs.org/en/). [**Express**](https://expressjs.com/) nos ayuda a manejar todo desde las rutas, manejo de peticiones y vistas.


**Requerimientos**

Antes de que podamos empezar a crear nuestra aplicación tenemos que instalar algunas cosas en nuestro sistema:

Instalar [`Node.js`](https://nodejs.org/en/).


**Aplicación inicial**

Primero creamos un folder para nuestra aplicación y navegamos hacia el nuevo folder:
```
mkdir mywebapp
cd mywebapp
```
Luego necesitamos crear un archivo de entrada, podemos nombrarlo como deseemos:
```
app.js or index.js
```
Inicializamos nuestra aplicación usando NPM(Manejador de Paquetes para Node), en la opción archivo de entrada seleccionamos el nombre del archivo creado en el paso anterior:
```
npm init
```

**Instalacion de Express:**
```
npm install express --save
```

**Uso de Express en nuestra aplicación**

Importación del módulo de [**Express**](https://expressjs.com/), este módulo se importará en el archivo de entrada seleccionado con anterioridad, creando la siguiente variable:
```
const express = require(‘express’);
```
Creación de variables de servicio, instanciando el modulo exportado de  [**Express**](https://expressjs.com/):
```
const app = express();
```
Creación de variable de puerto por el cual se comunicará nuestra aplicación :
```
const port = 4040;
```
Creamos nuestra primera ruta. Trataremos este tema con más profundidad en las próximas partes de esta serie:
```
app.get(‘/’, (req, res)=>{
   res.send(“Welcome to the world!”);
});
```
Si prestamos un poco de atención al código de arriba podemos notar que se están usando varias variables **req**(solicitud), **res**(respuesta) y una función tipo flecha(**=>**).

Configuración para que nuestra aplicación escuche en el puerto que deseamos:
```
app.listen(port, ()=>{
   console.log(`Listening in port: ${port}`)
});
```

Inicializamos nuestra aplicación llamando:
```
npm start
```

Ahora que nuestra aplicación está corriendo y escuchando en el puerto adecuado podemos usar el browser or usar una aplicación para generar peticiones como [**Postman**](https://www.postman.com/) llamando:

```
http://localhost:4040
```

**Conclusión**

Esto es lo mínimo requerido para crear una aplicación web con [**Node.js**](https://nodejs.org/en/) y [**Express**](https://expressjs.com/). En la próxima parte tendremos la oportunidad de profundizar en la estructura de los archivos y configuración de otros entornos de desarrollo.




