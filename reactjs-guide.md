# Guia React

## ¿Cómo funciona React?

![reactjs](./img/reactjs1.png "reactjs")

## Instalar node

instalar [https://nodejs.org/es/download/](https://nodejs.org/es/download/)

```console
node -v        # verificar node             
npm -v         # verificar npm              
```

## Instalar React

```console
npm install -g create-react-app      # instalar react de manera global
npm view react version               # verificar react
```

## Plugings VSCode

1. Prettier - Code formatter

2. ES7 React/Redux/GraphQL/React-Native snippets

3. Auto Import — ES6 & TS

4. styled-components-snippets - Jon Wheeler

5. MUI Snippets

extras [https://medium.com/canariasjs/tips-para-mejorar-tu-desarrollo-con-react-y-vscode-b4a09431d8eb](https://medium.com/canariasjs/tips-para-mejorar-tu-desarrollo-con-react-y-vscode-b4a09431d8eb)

---

## Crear Proyecto

```console
    create-react-app nombre-proyecto    # crear desde local
npx create-react-app nombre-proyecto    # crear desde internet
```

```console
npm start                               # levantar servidor
```

## Instalar Librerias (dependencias)

```console
npm i node-sass@4.13.0                                    # SASS
npm i react-router-dom@5                                  # Router DOM
npm install --save styled-components                      # StyledComponents  
npm install @mui/material @emotion/react @emotion/styled  # material ui
npm install @mui/icons-material                           # material icons
```

## Repositorio

React ya inició un repo

```console
git add .                       # captura los cambios
git commit -m 'create project'  # agrega los cambios
```

```console
brew install gh                                           # instalar Github CLI
gh repo create usuario/repo --public                      # crear repo en github

git remote add origin https://github.com/usuario/repo.git # conectar con repo
git remote -v                                             # verificar conexión
git branch --list                                         # ver ramas
git branch -m main                                        # cambiar nombre
git push -u origin main                                   # subir
```

---

## Estructura

```console
root
|
|──.git
|──.gitignore
|
|──package-lock.json
|──package.json
|
|──README.md
|
|──node_modules
|
|──public
|   |──favicon.ico
|   |──index.html
|   |──logo192.png
|   |──logo512.png
|   |──manifest.json
|   └──robots.txt
|
└──src
    |──App.css
    |──App.js
    |──App.test.js
    |
    |──index.css
    |──index.js
    |
    |──logo.png
    |──reportWebVitals.js
    |──setupTests.js
    |
    
    - - - - - - esto lo agregamos nosotros

    |
    |──assets
    |   |──data.js
    |   └──img
    |       |──logo.svg
    |       |──flower.jpg
    |       └──head.png
    |
    |──pages
    |   |
    |   |──home
    |   |   |──home.page.jsx
    |   |   └──home.styles.scss
    |   |
    |   |──about
    |   |   |──about.page.jsx
    |   |   └──about.styles.scss
    |   |
    |   └──contact
    |        |──contact.page.jsx
    |        └──contact.styles.scss
    |
    └──components
        |
        |──navbar
        |   |──navbar.page.jsx
        |   └──navbar.styles.scss
        |
        |──button
        |   |──button.page.jsx
        |   └──button.styles.scss
        |
        └──footer
            |──footer.page.jsx
            └──footer.styles.scss
```

## Sistema de Componentes

![reactjs](./img/reactjs2.png "reactjs")

## Tipos de Componentes

|  | Tipo | Atajo | Detalle |
| --- | --- | --- | --- |
| 1.1 | Arrow Function Component | **rafce** | React Arrow Function Export Component |
| 1.2 | Arrow Function Component | **rafc** | React Arrow Function Component |
| - | | | |
| 2.1 | Functional Component | **rfce** | React Functional Export Component |
| 2.2 | Functional Component | **rfc** | React Functional Component |
| - | | | |
| 3.1 | Class Component | **rcce** | React Class Export Component |
| 3.2 | Class Component | **rcc** | React Class Component |

```jsx
// 1.1
// rafce
// React Arrow Function Export Component

import React from "react";

const Name = () => {
  return <div>Name</div>;
};

export default Name;
```

```jsx
// 1.2
// rafc
// React Arrow Function Component

import React from "react";

export const Name = () => {
  return <div>Name</div>;
};
```

```jsx
// 2.1
// rfce
// React Functional Export Component

import React from "react";

function Name() {
  return <div>Name</div>;
}

export default Name;
```

```jsx
// 2.2
// rfc
// React Functional Component

import React from "react";

export default function Name() {
  return <div>Name</div>;
}
```

```jsx
// 3.1
// rcce
// React Class Export Component

import React, { Component } from "react";

export class Name extends Component {
  render() {
    return <div>Name</div>;
  }
}

export default Name;
```

```jsx
// 3.2
// rcc
// React Class Component

import React, { Component } from "react";

export default class Name extends Component {
  render() {
    return <div>Name</div>;
  }
}

```

## Rutas

En index.js

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter as Router } from 'react-router-dom';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <React.StrictMode>
      <Router>
        <App />
      </Router>
  </React.StrictMode>
);
```

En App.js

```jsx
import React from 'react';
import { Route, Switch, Redirect } from "react-router-dom";

import Home from "./pages/home/home.page";
import About from "./pages/about/about.page";
import Contact from "./pages/contact/contact.page";
import Error404 from "./pages/error404/error404.page";

import Navbar from './components/navbar/navbar.component';
import Footer from "./components/footer/footer.component";

import './App.css';

const App = () => {
  return (
    <>
        <Navbar />

        <Switch>
          <Route exact path="/" component={Home} />
          <Route exact path="/home">{<Redirect to="/" />}</Route>
          <Route exact path="/about" component={About} />
          <Route exact path="/contact" component={Contact} />
          <Route path="*" component={Error404} />
        </Switch>

        <Footer />
      
    </>);
};

export default App;
```

En home.page.jsx

```jsx
import React from "react";

const Home = () => {
  return <div>Home</div>;
};

export default Home;
```

En about.page.jsx

```jsx
import React from "react";

const About = () => {
  return <div>About</div>;
};

export default About;
```

En contact.page.jsx

```jsx
import React from "react";

const Contact = () => {
  return <div>Contact</div>;
};

export default Contact;
```

En error404.page.jsx

```jsx
import React from "react";

const Error404 = () => {
  return <div>Error404</div>;
};

export default Error404;
```

## Menu - navbar

Link

```jsx
import React from "react";
import { Link } from 'react-router-dom';

const Navbar = () => {
    return (
        <nav>
            <Link to="/">Home</Link>
            <Link to="/about">About</Link>
            <Link to="/contact">Contact</Link>
        </nav>
    );
};

export default Navbar;
```

NavLink

```jsx
import React from "react";
import { NavLink } from 'react-router-dom';

const Navbar = () => {
    return (
        <nav>
            <NavLink activeClassName="selected" to="/">Home</NavLink>
            <NavLink activeClassName="selected" to="/about">About</NavLink>
            <NavLink activeClassName="selected" to="/contact">Contact</NavLink>
        </nav>
    );
};

export default Navbar;
```

## Estilos CSS

```jsx
import React from "react";

import "./components/button/button.styles.css";

const Button = () => {
  return <div className="btn">Button</div>;
};

export default Button;
```

## Imágenes

```jsx
import React from "react";
import logo from "../../assets/img/logo.svg";
import "./components/logo/logo.styles.css";

const Logo = () => {
    return (
        <>
            <img className="logo" src={Logo} alt="logo"/> 
        </>
    );
};

export default Logo;
```

---

## HOOKS

### useState

Agregar un estado (1 dato)

```jsx
import React from "react";
import { useState } from "react";

const App = () => {

  const [color, setColor] = useState("red");

  return (
    <>
      <button
        type="button"
        onClick={() => setColor("blue")}>
          boton
      </button>
      <p>{color}</p>
    </>
  )

};

export default App;
```

Agregar un estado (1 dato con una función fuera del return )

```jsx
import React from "react";
import { useState } from "react";

const App = () => {

  const [color, setColor] = useState("red");

  const update = () => {
    setColor("blue")
  }

  return (
    <>
      <button
        type="button"
        onClick={update}>
          boton
      </button>
      <p>{color}</p>
    </>
  )

};

export default App;
```

Agregar un estado (1 dato con una función fuera del return + condición )

```jsx
import React from "react";
import { useState } from "react";

const App = () => {

  const [color, setColor] = useState("red");

  const updateColor = () => {
    if (color === "red") {
      setColor("blue")
    } else {
      setColor("red")
    }
  }

  return (
    <>
      <button
        type="button"
        onClick={updateColor}>
          boton
      </button>
      <p>{color}</p>
    </>
  )

};

export default App;










import React from "react";
import { useState } from "react";
import './App.css';
import Image from './animacion.gif';
```

Agregar un estado (1 dato dentro de un conjunto, con una función fuera del return + condición )

```jsx
const App = () => {

  // const [color, setColor] = useState("red");
  const [photo, setPhoto] = useState({
    height: "300px",
    width: "300px",
    display: "block"
  });

  const updateColor = () => {
    if (photo.display === "none") {
      setPhoto({
        ...photo,
        display: "block"
      })
    } else {
      setPhoto({
        ...photo,
        display: "none"
      })
    }
  }

  return (
    <>
      <button
        type="button"
        onClick={updateColor}
        className="red">
        btn
      </button>
      <p>{photo.height}</p>
      <p>{photo.width}</p>
      <p>{photo.display}</p>
      <img
        style={{
          height: photo.height,
          width: photo.width,
          display: photo.display
        }}
        src={Image}
        alt="pic"/>
    </>)
};

export default App;
```
