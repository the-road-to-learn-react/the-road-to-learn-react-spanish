# Introducción a React

Este capítulo es una introducción a React, una librería de JavaScript para renderizar interfaces en una página única y aplicaciones móviles, donde explico por qué los desarrolladores deberían considerar agregar la librería React a sus herramientas. Nos sumergiremos en el ecosistema de React para crear una aplicación desde cero sin configuración. En el camino introduciremos **JSX**, la sintaxis para React y **ReactDOM**, para que tengas entendimiento de los usos prácticos de React en aplicaciones web modernas.

## Hola, mi nombre es React.

En los últimos años las Single Page Applications o Aplicaciones de Página Única ([SPA por sus siglas en Inglés](https://es.wikipedia.org/wiki/Single-page_application)) se han vuelto populares. Frameworks como Angular, Ember y Backbone permiten a los desarrolladores JavaScript a construir aplicaciones web modernas más allá de lo que se podría lograr usando JavaScript puro (Vanilla JavaScript) y jQuery. Los tres frameworks mencionados están entre los primeros SPAs, apareciendo entre 2010 y 2011, pero hay muchas más opciones para el desarrollo de una sola página. La primera generación de frameworks SPA llegó en un nivel empresarial, por lo que eran menos flexibles. React, por otro lado, permanece como una librería innovadora que ha sido adoptada por líderes tecnológicos como ([Airbnb, Netflix y Facebook](https://github.com/facebook/react/wiki/Sites-Using-React)).

La versión inicial de React fue lanzada en 2013 por el equipo de desarrollo web de Facebook en 2013 como una librería para la capa de vistas, que representa la V en el Modelo Vista Controlador [(MVC por sus siglas en Inglés)](https://es.wikipedia.org/wiki/Modelo%E2%80%93vista%E2%80%93controlador). Esta librería permite renderizar componentes como elementos visibles en un navegador, mientras que con todo el ecosistema React, es posible crear aplicaciones de una sola página. Mientras que los primeros frameworks trataron de resolver muchos problemas a la vez, React solo es usado para construir tu capa de vista. Específicamente es una librería donde la vista es una jerarquía de componentes ensamblables(composable). 

En React, el enfoque permanece en la capa de Vistas hasta que se agreguen más aspectos a la aplicación. Estos son bloques de construcción para una SPA, que son esenciales para construir una aplicación madura, y tienen dos ventajas:

 *Puedes aprender a usar los bloques de construcción uno a la vez, sin preocuparte por entenderlos totalmente desde el principio. Esto es muy diferente a un Framework que incluye todo el set de bloques de construcción desde que inicias el proyecto. Este libro presenta a React como el primer bloque de construcción. Más bloques de construcción vendrán luego.

 *Todos los bloques de construcción son intercambiables. Lo que hace del ecosistema React algo innovador donde múltiples soluciones compiten entre sí para solucionar un problema, así que puedes elegir la solución más atractiva para ti y tu caso de uso.

Hoy en día, React es probablemente una de las mejores opciones para la construcción de sitios web modernos. De nuevo, React sólo se encarga de la capa de vista, [pero gracias a su amplio ecosistema, representa un framework completo, flexible e intercambiable](https://www.robinwieruch.de/essential-react-libraries-framework/). Además, cuenta con una API ligera, un ecosistema impresionante y una gran comunidad. 

### Ejercicios

Si quieres saber más sobre por qué elegí React o información más profunda sobre los temas mencionados arriba, éstos artículos pueden darte una mayor perspectiva:

* lee acerca de [por qué me cambié de Angular a React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* lee acerca de [el flexible ecosistema de React](https://www.robinwieruch.de/essential-react-libraries-framework/)
* lee acerca de [cómo aprender a usar un framework](https://www.robinwieruch.de/how-to-learn-framework/)

## Requerimientos

Antes de seguir avanzando debes estar familiarizado con los fundamentos del desarrollo web. Se espera que sepas trabajar con HTML, CSS y JavaScript, además de lo que significa el término [API](https://www.robinwieruch.de/what-is-an-api-javascript/), estarás usando esto durante todo el libro.

Te animo a que te unas al canal [Slack](https://slack-the-road-to-learn-react.wieruch.com/) oficial del libro para obtener ayuda o ayudar a otros.

### Editor y Terminal

Para las lecciones necesitarás un editor de texto plano o IDE y la terminal (línea de comandos). Puedes leer [mi guía de configuración](https://www.robinwieruch.de/developer-setup/) para ayudarte a organizar tus herramientas, esta guía está pensada para usuarios de Mac, sin embargo todas las herramientas que necesitarás están disponibles para varios sistemas operativos. Opcionalmente, puedes utilizar Git y GitHub por tu cuenta mientras estés realizando los ejercicios del libro, para mantener tus proyectos en repositorios de GitHub. Aquí dispones de una [pequeña guía](https://www.robinwieruch.de/git-essential-commands/) sobre cómo usar estas herramientas. Sin embargo, no es obligatorio su uso, podría resultar extenuante intentar aprenderlo todo al mismo tiempo.


### Node y npm

Por último, necesitarás instalar [Node y npm](https://nodejs.org/es/). Ambos se utilizan para administrar las bibliotecas que necesitarás en El Camino para aprender React. Instalarás paquetes externos Node a través del Gestor de Paquetes Node (npm, por sus siglas en Inglés). Estos paquetes pueden constituir bibliotecas o frameworks completos.

Puedes verificar la versiones instaladas Node y npm, respectivamente dentro de la terminal. Si no obtienes ninguna salida en la terminal, así que debes verificar tu instalación Node y npm. Estas son mis versiones al momento de escribir este libro:

~~~~~~~~
node --version
*v10.11.0
npm --version
*v6.4.1
~~~~~~~~

A continuación, un pequeño curso intensivo Node y npm. No es exhaustivo, pero obtendrás todas las herramientas necesarias. Si ya estás familiarizado con estas puedes omitir este capítulo.

El gestor **npm** permite instalar **paquetes externos** desde la terminal. Estos paquetes pueden ser un conjunto de funciones de utilidad, bibliotecas o frameworks enteros. Estos representan dependencias de tu aplicación, y puedes instalarlos en tu carpeta global de paquetes Node o bien en la carpeta local de tu proyecto.

Los paquetes globales Node son accesibles desde cualquier lugar de la terminal y sólo hay que instalarlos una vez en el directorio global. Puedes instalar un paquete a nivel global escribiendo en la terminal:


~~~~~~~~
npm install -g <paquete>
~~~~~~~~

La  etiqueta `-g` indica a npm que debe instalar el paquete a nivel global. Los paquetes locales son utilizados en tu aplicación. Por ejemplo, React como una librería será un paquete local, y puede ser requerido en tu aplicación para su uso. Puedes instalarlo a través de la terminal escribiendo:


~~~~~~~~
npm install react
~~~~~~~~

El paquete instalado aparecerá automáticamente en una carpeta llamada *node_modules/* y será agregado al archivo *package.json* junto a las otras dependencias de tu aplicación.

Ahora, ¿cómo inicializar la carpeta *node_modules/* y el archivo *package.json* en tu proyecto? Para ello existe un comando que inicia un proyecto npm que incluye automáticamente un archivo *package.json*. Sólo cuando tu proyecto posee este archivo, puedes instalar nuevos paquetes locales vía npm.


~~~~~~~~
npm init -y
~~~~~~~~

La etiqueta `-y` es un acceso directo para inicializar todos los valores predeterminados en el archivo *package.json* correspondiente. Inmediatamente, con npm inicializado en tu carpeta de proyecto, estás listo para instalar nuevos paquetes vía `npm install <paquete>`.

Un dato extra sobre el archivo *package.json*. Este archivo permite que compartas tu proyecto con otros desarrolladores sin compartir todos los paquetes Node. Contiene todas las referencias de los paquetes Node utilizados en tu proyecto. Y estos paquetes son llamados dependencias. Cualquiera puede copiar tu proyecto sin las dependencias, pues, estas son referenciadas en el archivo *package.json*. Así, cuando alguien copia tu proyecto, puede instalar todos las dependencias usando `npm install` en la terminal.

Hay otro comando npm que quiero mencionar, para prevenir confusiones:


~~~~~~~~
npm install --save-dev <paquete>
~~~~~~~~

La etiqueta `--save-dev` indica que el paquete Node es sólo usado en el entorno de desarrollo, esto quiere decir que no será usado en producción cuando cuelgues tu aplicación en un servidor. Esto es útil para realizar pruebas a una aplicación usando un paquete Node, quieres excluirlo de tu entorno de producción.

Podrías querer usar otros gestores para trabajar con los paquetes Node. **Yarn** es un gestor de dependencias que trabaja de forma similar a **npm**. Tiene su propia lista de instrucciones, aún así tienes acceso al mismo registro de npm. Yarn fue creado para resolver problemas que npm no pudo, pero ambas herramientas han evolucionado hasta el punto de que cualquiera de las dos sería suficiente.

### Ejercicios:

* configura un proyecto npm
  * crea una nueva carpeta con `mkdir <nombre_de_la_carpeta>`
  * navega hasta la carpeta con `cd <nombre_de_la_carpeta>`
  * ejecuta `npm init -y`
  * instala un paquete local, como React, con `npm install --save react`
  * échale un vistazo al archivo *package.json* y a la carpeta *node_modules/*
  * descubre cómo desinstalar el paquete Node *react*
* lee más sobre [npm](https://docs.npmjs.com/)

## Instalación

Existen varias maneras de comenzar una aplicación React. La primera es utilizar una [Content Delivery Network](https://es.wikipedia.org/wiki/Red_de_entrega_de_contenidos), o Red de Entrega de Contenidos (CDN por sus siglas en Inglés). Esto puede sonar complicado, pero no lo es. Muchas compañías usan CDNs que almacenan públicamente archivos para sus usuarios. Algunos de estos archivos que pueden ser una librería como React, ya que la librería empaquetada de React es únicamente un archivo JavaScript *react.js*.

Para utilizar React usando una CDN, puedes hacerlo insertando la etiqueta `<script>` en tu código HTML, con la URL de la CDN que desees utilizar. Para React necesitas dos archivos (librerías): *react* y *react-dom*.


~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

También puedes incluir React en tu aplicación inicializándola como un proyecto Node. Cuando tu aplicación tiene un archivo *package.json* es posible instalar *react* y *react-dom* desde la terminal. El único requisito es que la carpeta esté inicializada como  un proyecto npm, lo puedes hacer utilizando `npm init -y` tal como se mostró anteriormente. Es posible instalar múltiples paquetes en una misma línea a la vez con npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

El enfoque anterior se utiliza a menudo para añadir React a una aplicación existente administrada con npm.

Puede que tengas que lidiar con [Babel](http://babeljs.io/) también para hacer tu aplicación compatible con JSX (la sintáxis de React) y JavaScript ES6. Babel transpila tu código--es decir, lo convierte a vanilla JavaScript-- para que los navegadores puedan interpretar ES6 y JSX. Debido a la dificultad de esta tarea, Facebook desarrolló *create-react-app*, como una solución *cero-configuraciones* de React. La siguiente sección muestra cómo iniciar con el desarrollo de tu aplicación con esta herramienta.

### Ejercicios:

* lee más acerca de [cómo instalar React](https://facebook.github.io/react/docs/installation.html)

## Cero Configuraciones

En El Camino para aprender React usarás [create-react-app](https://github.com/facebookincubator/create-react-app) para iniciar el desarrollo tu aplicación. Este kit lanzado por Facebook en 2016, permite rápidamente empezar a trabajar en tu aplicación sin preocuparte por configuraciones. La gente [lo recomienda a principiantes en un 96%](https://twitter.com/dan_abramov/status/806985854099062785). Cuando utilizas *create-react-app* las herramientas y configuración evolucionan en segundo plano, mientras que el foco se mantiene en la implementación de la aplicación.

Para empezar, deberás agregar el *create-react-app* a tus paquetes globales Node. A partir de ahora estará disponible en la terminal para inicializar nuevas aplicaciones React.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

En la terminal puedes comprobar la versión de *create-react-app* para verificar que la instalación fue exitosa:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v2.0.2
~~~~~~~~

Ahora, puedes comenzar con el desarrollo de tu primera aplicación React. La llamaremos *hackernews*, puedes escoger un nombre distinto. Iniciar tu aplicación tomará unos pocos segundos. Después de esto, simplemente navega hasta el nuevo directorio con tu terminal:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

Ya puedes abrir la aplicación en tu editor de texto. La siguiente estructura de carpetas o variación de esta depende de la versión de *create-react-app* que tengas instalada, deberías poder ver algo cómo esto:

{title="Folder Structure",lang="text"}
~~~~~~~~
hackernews/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    favicon.ico
    index.html
    manifest.json
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
    registerServiceWorker.js
~~~~~~~~

A continuación, una pequeña descripción de cada una de las carpetas y archivos que encontrarás dentro del directorio recién creado. Está bien si no entiendes todo al principio.

* **README.md:** La extensión .md indica que el archivo está en formato "markdown". Markdown es un lenguaje de marcado ligero con sintaxis de texto plano. Muchos códigos fuente de proyectos incluyen un archivo *README.md* con instrucciones acerca del proyecto. Cuando estés subiendo tu proyecto a una plataforma como GitHub, eventualmente el archivo *README.md* mostrará promisoriamente su contenido cuando alguien acceda al repositorio. Como utilizaste *create-react-app*, tu archivo *README.md* debería ser igual al que se puede observar en el [repositorio GitHub de create-react-app](https://github.com/facebookincubator/create-react-app) oficial.

* **node_modules/:** Contiene todos los paquetes Node que han sido instalados vía npm. Como utilizaste *create-react-app* para inicializar tu aplicación, debes de poder ver un par de módulos Node ya instalados. Usualmente nunca tocarás esta carpeta, pues, con npm puedes instalar y desinstalar paquetes Node desde la terminal.

* **package.json:** Muestra una lista de las dependencias de paquetes Node y un conjunto de otras configuraciones referentes al proyecto.

* **.gitignore:** Especifica los archivos y carpetas que no serán añadidos a tu repositorio de git. Archivos que sólo vivirán en el proyecto local.

* **public/:** Almacena todos los archivos raíz de desarrollo, como por ejemplo *public/index.html*, este índice es el que se muestra en la dirección localhost:3000 cuando estás desarrollando tu aplicación. *create-react-app* viene ya configurado para relacionar este archivo índice con todos los scripts en la carpeta *src/*.

* **build/:** Esta carpeta será creada cuando el proyecto se esté preparando para la etapa de producción y contendrá todos los archivos relacionados a esta etapa de desarrollo. Todo el código escrito en las carpetas *src/* y *public/* serán empaquetados en un par de archivos y posicionados en la carpeta *build/* cuando el proyecto se esté compilando.

* **manifest.json** y **registerServiceWorker.js:** Por el momento no te preocupes acerca de lo que estos archivos hacen, no los necesitaremos en este proyecto. 

No necesitas tocar los archivos que acabamos de mencionar. Por ahora todo lo que necesitas está localizado en la carpeta *src/* y la mayor atención recae sobre el archivo *src/App.js*, que utilizaremos para implementar componentes React.

Más adelante, podrás dividir los componentes React en múltiples archivos, con uno o más componentes dentro de cada archivo.

Adicionalmente, encontrarás un archivo *src/App.test.js* para pruebas y uno *src/index.js* como punto de entrada al mundo React. Explorarás ambos archivos en capítulos próximos. También existe un archivo *src/index.css* y un archivo *src/App.css* que sirven para darle estilos a tu aplicación y sus componentes. Estos ya incluyen algunos estilos por defecto.

La aplicación *create-react-app*  es un proyecto npm, puedes utilizar npm para instalar y desinstalar paquetes Node en tu proyecto, adicionalmente, incluye algunos scripts que puedes ejecutar desde la terminal:

{title="Command Line",lang="text"}
~~~~~~~~
# Ejecuta la aplicación en http://localhost:3000
npm start

# Ejecuta las pruebas
npm test

# Prepara la aplicación para la etapa de construcción
npm run build
~~~~~~~~

Estos scripts se encuentran definidos en el archivo *package.json*. Tu aplicación React se encuentra ya inicializada, y lo mejor está por venir, cuándo ejecutes tu aplicación en el navegador.

### Ejercicios:

* ejecuta en la terminal, el comando `npm start` y visita la aplicación en tu navegador (puedes cancelar la ejecución de este comando presionando Control + C)
* ejecuta el comando interactivo `npm test`
* ejecuta el comando `npm run build` y verifica que la carpeta *build/* sea añadida a tu proyecto (puedes removerla después; nota que la carpeta build puede ser usada más tarde para [colocar tu aplicación en línea](https://www.robinwieruch.de/deploy-applications-digital-ocean/))
* familiarízate con la estructura de carpetas
* familiarízate con el contenido de los archivos
* lee más sobre [los comandos npm y create-react-app](https://github.com/facebookincubator/create-react-app)

## Introducción a JSX

Ahora conocerás JSX, la sintaxis empleada por React. Como fue mencionado antes, *create-react-app* ya ha iniciado una aplicación estándar. Todos los archivos incluyen implementaciones predeterminadas. Es tiempo de sumergirnos en el código fuente.

El único archivo que manipularás por ahora es: *src/App.js*.


~~~~~~~~
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}

export default App;
~~~~~~~~

No te dejes intimidar por las instrucciones import/export y la declaración de clases. Estas características pertenecen a JavaScript ES6, volveremos a ellas más adelante.

Dentro del archivo *src/App.js* se encuentra un **React ES6 class component o componente de clase React ES6** con el nombre `App`. Esto es una declaración de componente.

Básicamente, después de haber declarado un componente puedes utilizarlo como elemento en cualquier parte de tu aplicación produciendo una **instancia** de tu **componente**, o mejor dicho: Instanciando el componente.

El **elemento** que es devuelto se especifica dentro del método `render()`. Los elementos son de lo que están hechos los componentes. Es útil que entiendas las diferencias entre los términos "componente", "instancia" y "elemento".

En un momento verás donde se instancia el componente `App` y cómo se renderiza en el navegador.

El componente `App` es sólo la declaración y por si solo no hará nada. Para utilizar dicho componente podrías instanciarlo en algún lugar de tu JSX con la etiqueta `<App />`.

El contenido dentro del bloque de código perteneciente al método `render()` parece ser HTML, pero en realidad es JSX, y te permite mezclar HTML y JavaScript. Es potente, pero confuso cuando estás acostumbrado a HTML simple. Por eso que un buen punto de partida es comenzar utilizando HTML básico en tu JSX. Las expresiones JavaScript las puedes utilizar insertándolas entre corchetes.

Primero, vamos a eliminar todo el contenido por defecto que es retornado por `render` y agregar nuestro propio HTML.


~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>Bienvenido al Camino para aprender React</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Ahora, el método `render()` sólo devuelve HTML sin JavaScript. Vamos a definir "Bienvenido al Camino para aprender React" como el valor de una variable, que puede ser utilizad en JSX usando corchetes.


~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {

    var helloWorld = 'Bienvenido al Camino para aprender React';

    return (
      <div className="App">

        <h2>{helloWorld}</h2>

      </div>
    );
  }
}

export default App;
~~~~~~~~

Cuando inicies tu aplicación desde la terminal con `npm start` todo debería funcionar.

Además, probablemente notaste el atributo `className`. Este es similar al atributo estándar `class` en HTML. Debido a razones técnicas, JSX tuvo que reemplazar varios atributos internos de HTML. Puedes ver todos los [atributos HTML compatibles con JSX en la documentación de React](https://facebook.github.io/react/docs/dom-elements.html) aquí. Más adelante conoceremos nuevos atributos JSX.

### Ejercicios:

* define más variables y renderízalas en tu JSX
  * utiliza un complex object u objeto complejo en español, para representar a un usuario con nombre y apellido
  * renderiza las propiedades del usuario utilizando JSX
* lee más sobre [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)
* lee más sobre [componentes, elementos e instancias en React](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)

## ES6 const y let

Probablemente notaste que declaramos la variable `helloWorld` con `var`. JavaScript ES6 incluye otras dos formas de declarar variables: `const` y `let`. En JavaScript ES6 rara vez utilizarás `var`. Una variable declarada con `const` no se puede volver a asignar o volver a declarar. No puede ser mutada (cambiada o modificada), es decir, la estructura de datos se vuelve inmutable. Una vez que se define la estructura de datos, no se puede cambiar.


~~~~~~~~
// no permitido
const helloWorld = 'Bienvenido al Camino para aprender React';
helloWorld = 'Bye Bye React';
~~~~~~~~

Por otra parte, una variable declarada con `let` puede mutar.


~~~~~~~~
// permitido
let helloWorld = 'Bienvenido al Camino para aprender React';
helloWorld = 'Hasta luego, React';
~~~~~~~~

**TIP** Declara variables usando `let` cuando pienses reasignarlas.

Nota que la variable declarada con `const` no se puede modificar. Pero cuando el valor de la variable es un arreglo o un objeto, sus valores pueden ser alterados. Es decir, los valores dentro del objeto o arreglo no son inmutables.


~~~~~~~~
// permitido
const helloWorld = {
  text: 'Bienvenido al Camino para aprender React'
};
helloWorld.text = 'Hasta luego, React';
~~~~~~~~

Hay diferentes opiniones sobre cuándo usar *const* y *let*. Sugiero usar `const` siempre que sea posible, indicando así que se desea mantener la estructura de datos inmutable aunque los valores en objetos y arreglos se puedan modificar. La inmutabilidad es ampliamente adoptada en React y su ecosistema. Es por eso que `const` debe ser la opción por defecto cuando se define una variable, aunque no se trata de inmutabilidad realmente, sino de asignar las variables una sola vez. Esto muestra la intención de no cambiar(reasignar) la variable incluso cuando el su contenido puede ser modificado.

En tu aplicación utiliza `const` en vez de `var`.


~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {

    const helloWorld = 'Bienvenido al Camino para aprender React';

    return (
      <div className="App">
        <h2>{helloWorld}</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

### Ejercicios:

* lee más sobre [ES6 const](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Sentencias/const)
* lee más sobre [ES6 let](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Sentencias/let)
* investiga más acerca de las estructuras de datos inmutables
  * ¿Por qué se utilizan en la programación en general?
  * ¿Por qué se utilizan en React?

## ReactDOM

Antes de volver a trabajar en el componente `App` es posible que quieras ver dónde es utilizado. Este se encuentra declarado dentro del archivo *src/index.js*.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
~~~~~~~~

Básicamente `ReactDOM.render()` usa un nodo DOM perteneciente al HTML de tu aplicación para reemplazarlo con JSX. Así es como puedes integrar fácilmente React a cualquier aplicación externa. No está prohibido utilizar `ReactDOM.render()` varias veces en una aplicación. Puedes utilizarlo en varios lugares para iniciar la sintaxis JSX simple, un componente React, múltiples componentes React o una aplicación completa.

`ReactDOM.render()` por defecto requiere dos argumentos:

El primero es JSX que será renderizado.

Y el segundo especifica el lugar en el que la aplicación React será insertada dentro del código HTML de tu aplicación. En este ejemplo, se selecciona un elemento con `id='root'`. Puedes abrir el archivo *public/index.html* y encontrar allí dicho elemento.

En el ejercicio, `ReactDOM.render()` ya incluye el componente `App` como primer parámetro. Sin embargo, en el siguiente ejemplo utilizaremos código JSX simple y no la instancia de un componente. Todo esto para demostrar que al utilizar JSX simple no tienes es necesario instanciar un componente.

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>Hola Mundo de React</h1>,
  document.getElementById('root')
);
~~~~~~~~

### Ejercicios:

* abre *public/index.html* para ver dónde se conectan las aplicaciones React con el HTML de tu aplicación
* lee más sobre cómo [renderizar elementos](https://facebook.github.io/react/docs/rendering-elements.html)

## Reemplazo de Módulos en Caliente

Hot Module Replacement o Reemplazo de Módulos en Caliente (HMR por sus siglas en Inglés) es una interesante característica que puedes implementar en el archivo *src/index.js* para mejorar tu experiencia como desarrollador. Pero es opcional y puede que resulte abrumador al principio.

En *create-react-app* ya es una ventaja que el navegador recargue automáticamente la página tan pronto el código fuente cambia. Inténtalo, cambia la variable `helloWorld` en tu archivo *src/App.js*. El navegador debe recargar la página automáticamente. Sin embargo, esto puede hacerse de una mejor manera.

El Reemplazo de Módulo Caliente es una herramienta para actualizar tu aplicación dentro del navegador. Es decir, el navegador no actualiza la página entera. En *create-react-app* esta herramienta puede ser fácilmente activada, sólo necesitas agregar unas pocas líneas de código en tu *src/index.js* - tu punto de entrada de React -.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);


if (module.hot) {
  module.hot.accept();
}

~~~~~~~~

Eso es todo. Ahora, intenta cambiar de nuevo la variable `hellowWorld` en tu archivo *src/App.js*. El navegador no recargará la página, sin embargo, notarás que la aplicación vuelve a cargar y muestra la salida correcta.

HMR ofrece múltiples ventajas:

Imagina que estás depurando tu código con declaraciones `console.log()`. Estas declaraciones permanecerán en la consola de desarrolladores, incluso si el código cambia, pues el navegador ya no actualiza la página. Lo que puede resultar conveniente al momento de depurar.

En una aplicación en crecimiento, una actualización de página retrasa la productividad. Tienes que esperar hasta que la página se cargue, y una recarga de página puede tardar varios segundos en una aplicación grande. HMR soluciona este inconveniente.

Sin embargo, el mayor beneficio que ofrece HMR es que puede mantener el estado de la aplicación. Imagina que tienes un diálogo en tu aplicación que consta de varios pasos y estás en el paso 3. Sin HMR el código fuente cambiaría y el navegador actualizaría la página, después tendrías que abrir el cuadro de diálogo nuevamente y navegar desde el paso 1 hasta el paso 3. 

Con HMR el diálogo permanece abierto en el paso 3. Es decir, mantiene el estado de la aplicación, aunque el código fuente cambie. La aplicación misma vuelve a cargar, más no la página.

### Ejercicios:

* cambia el código fuente de *src/App.js* varias veces para ver a HMR en acción
* mira los primeros 10 minutos de [Live React: Hot Reloading with Time Travel](https://www.youtube.com/watch?v=xsSnOQynTHs) por Dan Abramov

## JavaScript avanzado en JSX

Volvamos a trabajar en el componente `App`. Hasta ahora logramos renderizar algunas variables primitivas con JSX. Ahora, comenzarás a renderizar una lista de artículos. La lista en principio contendrá datos artificiales, pero más adelante recibirá los datos desde una API externa, lo que será mucho más emocionante.

Primero, define la lista de elementos.


~~~~~~~~
import React, { Component } from 'react';
import './App.css';


const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  {
    title: 'Redux',
    url: 'https://github.com/reactjs/redux',
    author: 'Dan Abramov, Andrew Clark',
    num_comments: 2,
    points: 5,
    objectID: 1,
  },
];


class App extends Component {
  ...
}
~~~~~~~~

Los datos artificiales representan los datos que más adelante serán extraídos de la API. Cada elemento dentro de la lista tiene un título, una URL y un autor. Además incluye un identificador, puntos (que indican la popularidad de un artículo) y un recuento de comentarios.

Ahora, puedes utilizar el [método `map` de JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/map) incorporándolo dentro del código JSX. El método `map` permite iterar sobre la lista de elementos para poder mostrarlos. Recuerda que dentro del código JSX debes encerrar entre corchetes la expresión JavaScript:


~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">

        {list.map(function(item) {
          return <div>{item.title}</div>;
        })}

      </div>
    );
  }
}

export default App;
~~~~~~~~

Usar JavaScript junto con HTML en JSX es muy poderoso. Es posible que antes hayas utilizado el método `map` para convertir una lista de elementos a otra, pero esta vez lo utilizas para convertir una lista de elementos a elementos HTML.


~~~~~~~~
const array = [1, 4, 9, 16];

// pasa una función a map
const newArray = array.map(function (x) { return x * 2; });

console.log(newArray);
// resultado esperado: Array [2, 8, 18, 32]
~~~~~~~~

Por ahora sólo se muestra el valor de la propiedad `title` correspondiente a cada elemento de la lista. A continuación, vamos a mostrar otras propiedades del artículo.


~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">

        {list.map(function(item) {
          return (
            <div>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
            </div>
          );
        })}

      </div>
    );
  }
}

export default App;
~~~~~~~~

Puedes ver como el método `map` está sangrado dentro del código JSX. Cada propiedad de elemento se muestra en una etiqueta `<span>`. Además, la propiedad URL del elemento se utiliza en el atributo `href` de la etiqueta de anclaje.

React hará todo el trabajo por ti y mostrará cada elemento. Aunque puedes hacer algo para ayudar a que React alcance su máximo potencial y tenga un mejor rendimiento. Al asignar un atributo clave a cada elemento de lista, React será capaz de identificar cada elemento de la lista, React puede identificar elementos cambiados y eliminados cuando la lista cambie. Esta lista de ejemplo tiene un identificador:


~~~~~~~~
{list.map(function(item) {
  return (

    <div key={item.objectID}>

      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

Asegúrate de que el atributo clave tiene un valor estable. No cometas el error de usar el índice asignado para el elemento dentro del arreglo. El índice del arreglo no es del todo estable. Por ejemplo, cuando el orden de la lista cambie, React tendrá dificultades para identificar los elementos correctamente, pues cada elemento dentro de la lista tendrá ahora un orden y un índice distinto.


~~~~~~~~
// No hagas esto
{list.map(function(item, key) {
  return (
    <div key={key}>
      ...
    </div>
  );
})}
~~~~~~~~

Ahora puedes iniciar tu aplicación desde la terminal, abrir tu navegador y ver los dos elementos desplegados correspondientes a la lista que acabas de agregar al código de tu componente.

### Ejercicios:

* lee más sobre [llaves y listas de elementos en React](https://facebook.github.io/react/docs/lists-and-keys.html)
* repasar [funcionalidades estándar para Arreglos en JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/map)
* utiliza más expresiones JavaScript por tu cuenta en JSX

## Funciones Flecha en ES6 (Arrow Functions)

JavaScript ES6 introdujo las funciones flecha, que resultan más cortas que expresiones de función tradicionales.


~~~~~~~~
// declaración de una función
function () { ... }

// declaración de una función flecha
() => { ... }
~~~~~~~~

Puedes quitarlos cuando la función sólo recibe un argumento, pero tienes que conservarlos cuando hay múltiples argumentos.


~~~~~~~~
// permitido
item => { ... }

// permitido
(item) => { ... }

// no permitido
item, key => { ... }

// permitido
(item, key) => { ... }
~~~~~~~~

Ahora, revisemos nuevamente el método `map`. Puedes declararlo de manera más concisa con una función flecha ES6.


~~~~~~~~

{list.map(item => {

  return (
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

Además, es válido eliminar el *cuerpo del bloque* de la función flecha ES6. En un *cuerpo conciso* un `return` implícito se adjunta, de modo que se puede quitar la declaración `return`. Esto sucederá a menudo en el libro, así que asegúrate de entender la diferencia entre un cuerpo de bloque y un cuerpo conciso dentro de las funciones flecha ES6.


~~~~~~~~

{list.map(item =>
  <div key={item.objectID}>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>

)}

~~~~~~~~

Ahora tu código JSX es más conciso y legible. Al declarar `map` de esta manera, se omite la sentencia "function", los corchetes y la declaración `return`.

### Ejercicios:

* lee más sobre [funciones flecha ES6](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Funciones/Arrow_functions)

## Clases ES6 (ES6 Classes)

JavaScript ES6 introdujo el uso clases, que comúnmente son utilizadas en lenguajes de programación orientados a objetos. JavaScript fue y sigue siendo muy flexible en sus paradigmas de programación. Funciona bien tanto con programación funcional como con programación orientada a objetos, lado a lado, para sus casos de uso particulares.

 React adopta el paradigma programación funcional, sin embargo, al crear estructuras de datos inmutables las clases se utilizan para declarar componentes y se les llama componentes de clase ES6. React aprovecha las mejores cualidades de ambos paradigmas de programación.

Considera la siguiente clase llamada `Developer` para examinar una clase de JavaScript ES6 sin pensar en componentes React.


~~~~~~~~
class Developer {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  getName() {
    return this.firstname + ' ' + this.lastname;
  }
}
~~~~~~~~

Una clase tiene un constructor que permite instanciarla. Este constructor puede tomar argumentos y asignarlos a la instancia de la clase. Además, una clase puede definir funciones, y dado que la función está asociada a una clase, se le llama método de clase.

En el ejemplo anterior, `class Developer` es sólo la declaración de la clase. Es posible crear varias instancias de una clase por medio de la invocación. Lo que resulta similar al componente de clase ES6, que tiene una declaración pero debe ser usado en otro lugar para instanciarlo:


~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
~~~~~~~~

React utiliza clases JavaScript ES6 en los componentes de clase ES6. Ya utilizaste un componente de clase ES6 anteriormente.



~~~~~~~~
import React, { Component } from 'react';

...

class App extends Component {
  render() {
    ...
  }
}
~~~~~~~~

La clase `App` se extiende desde otro `Component`. En la programación orientada a objetos se emplea el principio de herencia, que hace posible pasar funcionalidades de una clase a otra clase.

La clase `App` extiende la funcionalidad de la clase `Component`. Para ser más específicos, `App` hereda funcionalidades de la clase Component. Este componente se utiliza para extender una clase ES6 básica a una clase de componente ES6. Tiene todas las funcionalidades que un componente necesita. Y una de estas funcionalidades es un método que ya conoces, el método `render()` del que conocerás más funcionalidades más adelante.

La clase `Component` encapsula todas las funcionalidades de React que un desarrollador no necesita ver. Permite a los desarrolladores utilizar las clases como componentes en React.

Los métodos encapsulados dentro de la clase `Component` representan la interfaz pública. Uno de estos métodos debe ser sobrescrito, para los demás no es necesario. Aprenderás acerca de estos últimos cuando el libro llegue a los métodos del ciclo de vida en un capítulo próximo. El método `render()` tiene que ser sobrescrito, porque define la salida de React `Component`.

Ahora que ya conoces los conceptos básicos de las clases JavaScript ES6 y cómo se utilizan en React para extenderlas a componentes, aprenderás más sobre los métodos de Componente cuando el libro describa los Métodos de Ciclo de Vida de React.

### Ejercicios:

* lee sobre [Fundamentos de JavaScript antes de aprender React](https://www.robinwieruch.de/javascript-fundamentals-react-requirements/)
* lee más sobre [Clases en ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)


¡Ahora sabes cómo arrancar tu propia aplicación React! Repasemos brevemente los visto en los últimos capítulos:

* React
  * *create-react-app* arranca una aplicación React
  * JSX mezcla HTML y JavaScript para definir los componentes React
  * Componentes, instancias y elementos son cosas diferentes
  * `ReactDOM.render()` es un punto de entrada para renderizar componentes React en el DOM
  * funciones JavaScript incorporadas pueden ser utilizadas en JSX
  * El método `map` puede utilizarse para renderizar una lista de elementos como elementos HTML
* ES6
  * Declaraciones de variables con `const` y `let` pueden ser utilizadas en casos de uso particulares
  * las funciones flecha pueden utilizarse para acortar las declaraciones de funciones
  * las clases se utilizan para definir componentes en React

Es recomendable que te detengas en este punto. Internaliza lo aprendido y aplícalo por cuenta propia. Puedes experimentar con el código fuente que has escrito hasta ahora.

El código fuente está disponible en el [repositorio oficial](https://github.com/rwieruch/hackernews-client/tree/0c5a701170dcc72fe68bdd594df3a6522f58fbb3).
