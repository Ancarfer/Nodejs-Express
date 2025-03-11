# Node.js + Express

## 1. ¿Qué es Node.js?
Node.js es un entorno de ejecución de código abierto que permite ejecutar código JavaScript en el servidor. 
Utiliza un modelo de I/O no bloqueante, lo que lo hace ideal para aplicaciones de alta concurrencia, como servidores web y aplicaciones en tiempo real. Es muy utilizado para desarrollar aplicaciones backend y APIs. Su arquitectura basada en eventos lo hace eficiente para manejar múltiples conexiones simultáneamente.

Casos de uso comunes:
- LinkedIn
- Netflix
- Uber
- PayPal

## 2. ¿Qué es Express?
Express.js es un framework ligero para Node.js que facilita la construcción de aplicaciones web y APIs. Proporciona herramientas para manejar rutas, peticiones HTTP, y middleware, haciendo que el desarrollo sea más sencillo y rápido.

## 3. Instalación de Node.js y Express
### - Instalación de Node.js
Para instalar Node.js, visita la [página oficial](https://nodejs.org/) y descarga la versión recomendada para la mayoría de los usuarios. Luego, sigue las instrucciones según tu sistema operativo.

### - Instalación de Express
Una vez tengas Node.js instalado, puedes crear un proyecto y añadir Express mediante npm (Node Package Manager):
```bash
npm init -y
npm install express
```

### Estructura de carpetas

``` bash
my-app/
  ├── app.js
  ├── package.json
  ├── routes/
  │   └── index.js
  ├── public/
  │   └── styles.css
  └── views/
      └── index.ejs
```
### Uso de Nodemon
#### 1. ¿Qué es Nodemon? 
Nodemon es una herramienta que reinicia automáticamente tu servidor cuando detecta cambios en el código:
- **Instalación de Nodemon:**
```bash
npm install -g nodemon
```

- **Uso de Nodemon:**
Para iniciar tu aplicación con Nodemon, ejecuta:
```bash
nodemon app.js
```
## 4. Middleware
#### 1. ¿Qué es Middlewares?

Los middlewares en Express son funciones intermedias que se ejecutan entre la solicitud del cliente y la respuesta del servidor.

```javascript
const express = require('express');
const app = express();

// Middleware que se ejecuta en cada petición(te muestra todas las peticiones que se hagan)
app.use((req, res, next) => {
    console.log(`Nueva petición: ${req.method} ${req.url}`);
    next(); // Llama a la siguiente función (sino, la petición se queda bloqueada)
});
```
#### 2. ¿Para qué sirven los middlewares?

Los middlewares se utilizan para modificar, analizar o controlar las peticiones antes de que lleguen a las rutas finales.

1. Registro de peticiones:
    - Permiten registrar y analizar todas las solicitudes que llegan al servidor. Esto es útil para depuración o para estadísticas.
2. Autenticación y autorización:
    - Permiten verificar si un usuario está autenticado y si tiene permisos para acceder a una ruta específica.
3. Procesamiento de datos:
    - Pueden ser utilizados para analizar o modificar datos. Por ejemplo,
4. Manejo de errores:
    - Permiten centralizar y gestionar los errores, haciendo que sea más fácil capturar errores y responder de manera consistente.

#### 3. Tipos de middlewares en Express

**Middleware de aplicación**
- Ejemplo: Registrar todas las solicitudes que entran al servidor, independientemente de la ruta.

``` javascript
 app.use((req, res, next) => {
    console.log(`Solicitud recibida: ${req.method} ${req.url}`);
    next();
});
```
**Middleware de ruta**
- Ejemplo: Registrar cuando un usuario accede a la ruta /user.
``` javascript
app.get('/user', (req, res, next) => {
    console.log('Acceso a la ruta /user');
    next();
}, (req, res) => {
    res.send('Bienvenido al perfil de usuario');
});
```
**Middleware de error**
- Ejemplo: Capturar errores en la aplicación y enviar una respuesta adecuada.
``` javascript
app.use((err, req, res, next) => {
    console.error(`Error detectado: ${err.message}`);
    res.status(500).send('Algo salió mal en el servidor.');
});
```
**Middleware de terceros**
- Ejemplo: Registrar el uso de middleware de terceros como cors y body-parser.
``` javascript
const cors = require('cors');
const bodyParser = require('body-parser');

app.use(cors());  // Habilitar CORS para solicitudes de otros dominios
app.use(bodyParser.json());  // Analizar cuerpos JSON
```
**Middleware de autenticación y autorización**
- Ejemplo: Registrar intentos de acceso y validar autenticación.
``` javascript
const checkAuth = (req, res, next) => {
  if (!req.isAuthenticated()) {
    return res.status(401).send('No autorizado');
  }
console.log('Usuario autenticado, acceso permitido');
next();
};
```
------------------
## ¿QUÉ ES CRUD?
CRUD es un acrónimo que representa las cuatro operaciones básicas que se pueden realizar en una base de datos o en una aplicación de almacenamiento de datos:

- Create (Crear): Añadir nuevos registros o datos.
- Read (Leer): Recuperar o consultar datos existentes.
- Update (Actualizar): Modificar datos existentes.
- Delete (Eliminar): Eliminar datos existentes.

Estas operaciones son fundamentales en el desarrollo de aplicaciones que gestionan información.
------------------
## 4. Implementación de CRUD con `Express y PostgreSQL`
#### - Instalación de `pg`

``` bash
npm install pg-promise
```

#### - Configuración de la conexión a PostgreSQL

Para poder hacer la connexion a la base de datos, tendremos que crear un archivo el cual pondremos la configuracion de la base de datos:

#### Archivo `pool.js`
``` javascript
import pkg from 'pg';
const { Pool } = pkg;

const pool = new Pool({
  user: 'postgres',
  host: 'localhost',
  database: 'supermercado',
  password: 'admin',
  port: 5432,
});

export default pool;

```

#### - Creación de las rutas y CRUD en Express

**Archivo `route.js`**
``` javascript
import express from "express";
import pool from "./pool.js"; // Importa la conexión a PostgreSQL

const app = express();
const PORT = 3000;

```

**method `LISTEN`**
``` javascript
app.listen(PORT, () => {
  console.log(`Prueba en http://localhost:${PORT}/productos`);
});
```
**method `GET`**
``` javascript
//Para obtener los datos dinamicamente deberemos recuperar el valor del elemento con value
app.get("/:tabla/:id", async (req, res) => {
  try {
    const id = parseInt(req.params.id);
    const tabla = req.params.tabla;
    const result = await pool.query(`SELECT * FROM ${tabla} WHERE id = ${id}`);
    res.json(result.rows);
  } catch (e) {
    console.error(e);
    res.status(500).json({ error: "Error al obtener productos" });
  }
});
app.get("/:tabla", async (req, res) => {
  try {
    const tabla = req.params.tabla;
    const result = await pool.query(`SELECT * FROM ${tabla}`);
    res.json(result.rows);
  } catch (e) {
    console.error(e);
    res.status(500).json({ error: "Error al obtener productos" });
  }
});
```
**method `DELETE`**
``` javascript 
app.delete(`/:tabla/:id`, async (req, res) => {
  try {
    const id = parseInt(req.params.id);
    const tabla = req.params.tabla;
    await pool.query(
      `DELETE FROM ${tabla} WHERE id = ${id}`
    );
    const result = await pool.query("SELECT * FROM productos");
    res.json(result.rows);
  } catch (e) {
    console.error(e);
  }
});
```

**method `POST`**
``` javascript 
app.post('/postData',(req,res)=>{
    const {name,id} = req.body

    const insert_query = 'INSERT INTO demotable (name,id) VALUES ($1, $2)'
    
    con.query(insert_query, [name,id], (err, result)=>{
        if(err){
            res.send(err)
        }else{
            console.log(result)
            res.send("POSTED DATA")
        }
    })
});
```
**method `PUT`**
``` javascript 
app.put('/update/:id',(req,res)=>{
    const id=req.params.id;
    const name=req.body.name;
    const update_query="UPDATE demotable SET name=$1 WHERE id=$2"
    con.query(update_query,[name,id],(err,result)=>{
        if(err){
            res.send(err)
        }else{
            res.send("SUCCESSFULLY UPDATED")
        }
    })
});
```

**En caso de experimentar algun error tendremos que editar el archivo `package.js`**
```javascript
"type": "module"
```

-------------------------

## 📝 Prueba  Práctica

**Necesitaremos instalar la extensión `ThunderClient`**

**Debemos asegurarnos de ejecutar `nodemon ./route.js` previamente**

![ThunderClient](thunder.PNG)

**Crearemos una nueva `Request`**

![Request](request.PNG)

**Resultado de la ruta `/productos` con `GET`**

**Tipos de `method`**

![Method](method.PNG)


## 📝 Ejercicio para la clase

- Clona el repositorio en el cual se encuentra la base de datos de ejemplo
  + [Base de datos Sakila](https://github.com/jOOQ/sakila)
- Configura la conexión con el servidor y BDD
- Identifica el funcionamiento de las rutas y haz pruebas
  + http://localhost:3000/film
  + http://localhost:3000/film
  + http://localhost:3000/film/2
  + http://localhost:3000/film/2
- Crea una estructura para tu web donde mostrar la información
- Una vez elegido, comprueba que las rutas funcionan de manera correcta 


#### Contribuciones y aplausos 
[Adrian](https://github.com/danadiplas/AJAXGrupo1/blob/main/docs/NodeExpress.md) | 
[Àngel](https://github.com/Tailosrx/grup5/blob/main/docs/ancarfer-nodejs.md) | 
[Arnau](https://gitlab.com/pr-ctiques/grup2-chinook/-/blob/ctrlalt3-main-patch-48403/docs/express.md?ref_type=heads) | 
[Iker](https://github.com/simonquiceno/grupo3/blob/main/docs/Node%2BExpress.md) | 
[Xavier](https://github.com/Xavier545/M06UF4Grupo4/blob/main/docs/nodejs%2Bexpressjs.md)


