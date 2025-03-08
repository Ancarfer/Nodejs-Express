# Node.js + Express


## 1. ¿Qué es Node.js?
Node.js es un entorno de ejecución de código abierto que permite ejecutar código JavaScript en el servidor. 
Utiliza un modelo de I/O no bloqueante, lo que lo hace ideal para aplicaciones de alta concurrencia, como servidores web y aplicaciones en tiempo real. Es muy utilizado para desarrollar aplicaciones backend y APIs. Su arquitectura basada en eventos lo hace eficiente para manejar múltiples conexiones simultáneamente.

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


## 4. Uso de Nodemon + Middleware y estructura del proyecto
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

#### 2. ¿Qué es Middlewares?

Los middlewares en Express son funciones intermedias que se ejecutan entre la solicitud del cliente y la respuesta del servidor.

```javascript
const express = require('express');
const app = express();

// Middleware que se ejecuta en cada petición
app.use((req, res, next) => {
    console.log(`Nueva petición: ${req.method} ${req.url}`);
    next(); // Llama a la siguiente función (sino, la petición se queda bloqueada)
});

// Ruta de ejemplo
app.get('/', (req, res) => {
    res.send('¡Hola, mundo!');
});

// Levantar el servidor
app.listen(3000, () => {
    console.log('Servidor corriendo en http://localhost:3000');
});

```

#### 3. ¿Para qué sirven los middlewares?

Los middlewares se utilizan para modificar, analizar o controlar las peticiones antes de que lleguen a las rutas finales.

1. Registro de peticiones:
    - Permiten registrar y analizar todas las solicitudes que llegan al servidor. Esto es útil para depuración o para estadísticas.
    - Ejemplo: Registrar cada solicitud que entra con el método HTTP y la URL.
2. Autenticación y autorización:
    - Permiten verificar si un usuario está autenticado y si tiene permisos para acceder a una ruta específica.
    - Ejemplo: Verificar un token JWT en las cabeceras para validar si un usuario puede acceder a una ruta protegida.
3. Procesamiento de datos:
    - Pueden ser utilizados para analizar o modificar datos. Por ejemplo,
    - Ejemplo: Usar express.json() para parsear el cuerpo de las solicitudes con formato JSON.
4. Manejo de errores:
    - Permiten centralizar y gestionar los errores, haciendo que sea más fácil capturar errores y responder de manera consistente.
    - Ejemplo: Usar un middleware de error para devolver un mensaje de error personalizado si una ruta no existe.

#### 4. Tipos de middlewares en Express



## 4. Implementación de CRUD con Express y PostgreSQL
#### Instalación de `pg`
#### Configuración de la conexión a PostgreSQL
#### Creación de las rutas CRUD en Express
### ¿Como ver el CRUD? (Postman, curl)
## 📝 Prueba  Práctica
## 📝 Ejercicio para la clase

#### [Adrian](https://github.com/danadiplas/AJAXGrupo1/blob/main/docs/NodeExpress.md), [Àngel](https://github.com/Tailosrx/grup5/blob/main/docs/ancarfer-nodejs.md), [Arnau](https://gitlab.com/pr-ctiques/grup2-chinook/-/blob/ctrlalt3-main-patch-48403/docs/express.md?ref_type=heads), [Iker](https://github.com/simonquiceno/grupo3/blob/main/docs/Node%2BExpress.md) y [Xavier](https://github.com/Xavier545/M06UF4Grupo4/blob/main/docs/nodejs%2Bexpressjs.md)
