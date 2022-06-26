# nodeJS


## Instalación MongoDB

1. Homebrew [https://brew.sh/index_es](https://brew.sh/index_es)

  ```console
  which brew                  # verificar si está intalado

        /usr/local/bin/brew   # SI está instalado.
        brew not found        # NO está instalado.

                              # instalar
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```

2. Xcode [https://xcodereleases.com/](https://xcodereleases.com/)

  ```console
  xcode-select -p                             # verificar si está intalado
        /Library/Developer/CommandLineTools   # SI está instalado
        gcc --version                         # versión instalada

  xcode-select --install                      # instalar
  ```

 3. MongoDB [https://www.mongodb.com/docs/v4.4/tutorial/install-mongodb-on-os-x/](https://www.mongodb.com/docs/v4.4/tutorial/install-mongodb-on-os-x/)
  ```console
  brew tap mongodb/brew                       # Formula de Homebrew para MongoDB y Herramientas de Base de Datos
  brew update                                 # actualizar Homebrew
  brew install mongodb-community@4.4          # instalar
        
        brew services list                    # muestra los servicios de brew

  brew services start mongodb-community@4.4   # enciende MongoDB
  brew services stop mongodb-community@4.4    # apaga MongoDB

                                              # agrega path para que reconozca el comando "mongo"
  export PATH=$PATH:/usr/local/opt/mongodb-community@3.6/bin

        mongo                                 # entrar a MongoDB
        show dbs                              # mostrar las bases existentes en MongoDB

  ```

## Crear Servidor

```console
mkdir server1                 # crea carpeta
cd server1                    # entrar a carpeta
npm init -y                   # iniciar servidor

code .                        # abrir VSC con el directorio

npm install express --save    # renderizar del lado del servidor 
npm i body-parser             # permite a Express leer el crear un objeto Json
npm install mongoose --save   # conectar con mongoDB
npm i nodemon                 # actualización automática de navegador

```

app.js
- importar express
- meter todo express en app
- crear puerto 5500
- app.get (home, solicitud y respuesta)
- app.listen(escuchador)

```js
import express from "express"

const app = express()
const port = 5500


app.get("/", (req, res) => {
    res.send("Hello World")
})

app.listen(port, () => {
    console.log(`Server is running on port ${port}`)
})
```

package.json

- agregar "type": "module" para usar `import`
- agregar scripts para lenvantar el servidor con `npm run serve`  

```json
{
  "name": "api1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "serve": "nodemon app.js"
  },  "author": "",
  "license": "ISC",
  "type": "module",
  "dependencies": {
    "body-parser": "^1.20.0",
    "express": "^4.18.1",
    "mongoose": "^6.3.6",
    "nodemon": "^2.0.16"
  }
}
```


## Levantar Servidor

```console
npm run serve
```


## Crear Endopint

app.js
- configurar
  - usar `app.use(express.json())` para que lo lea como json
  - aumentar de peso al json, por defecto viene 100kb
- crear endpoint
  - crear console log
  - crear respuesta

```js
app.use(express.json({ limit: '50mb' }))

app.post("/api/clients", (req, res) => {
    console.log("dummy endpoint")
    res.send("you have posted to the dummy endpoint")
})
```

instalar [https://www.postman.com/](https://www.postman.com/) para probar endpoint enviando:

```web
POST : http://localhost:5500/api/clients/
```


