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
  xcode-select -p                              # verificar si está intalado
        /Library/Developer/CommandLineTools    # SI está instalado
        gcc --version                          # versión instalada

  xcode-select --install                 # instalar
  ```

 3. MongoDB [https://www.mongodb.com/docs/v4.4/tutorial/install-mongodb-on-os-x/](https://www.mongodb.com/docs/v4.4/tutorial/install-mongodb-on-os-x/)
  ```console
  brew tap mongodb/brew                       # Formula de Homebrew para MongoDB y Herramientas de Base de Datos
  brew update                                 # actualizar Homebrew
  brew install mongodb-community@4.4          # instalar
        
        brew services list                          # muestra los servicios de brew

  brew services start mongodb-community@4.4.  # enciende MongoDB
  brew services stop mongodb-community@4.4.   # apaga MongoDB

                                              # agrega path para que reconozca el comando "mongo"
  export PATH=$PATH:/usr/local/opt/mongodb-community@3.6/bin

        mongo        # entrar a MongoDB
        show dbs     # mostrar las bases existentes en MongoDB

  ```

## Instalación MongoDB




```console
mkdir server1       # crea carpeta
cd server1          # entrar a carpeta
npm init -y         # iniciar servidor

code .              # abrir VSC con el directorio

npm install express --save
npm i body-parser
npm install mongoose --save
npm i nodemon

```

