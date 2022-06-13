# nodeJS


## instalación

### Mongodb

1. Homebrew
  ```console
  which brew                  # verificar si está intalado

        /usr/local/bin/brew   # SI está instalado.
        brew not found        # NO está instalado.

                              # instalar
        /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```

2. Xcode
  ```console
  xcode-select -p                              # verificar si está intalado
        /Library/Developer/CommandLineTools    # SI está instalado
        gcc --version                          # versión instalada

        xcode-select --install                 # instalar
  ```

 3. Mongobd
  ```cosole
        brew tap mongodb/brew                  # Formula de Homebrew para MongoDB y Herramientas de Base de Datos
        brew update                            # actualizar Homebrew
        brew install mongodb-community@5.0.    # instalar
  ```





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

