# Introducción a React

Este capítulo te brinda una introducción a React. Quizá te preguntas: ¿Por qué debería aprender React, en primer lugar? Este capítulo puede darte la respuesta a esa pregunta. Después, te sumergiras en el ecosistema maquetando tu primera aplicación React. En el camino, conocerás además JSX y ReactDOM. Así que prepárate para tu primer componente React.

## Hola, mi nombre es React.

**¿Por qué deberías molestarte en aprender React?** En los últimos años las single page aplications ([SPA](https://es.wikipedia.org/wiki/Single-page_application)) se han vuelto populares. Frameworks como Angular, Ember y Backbone ayudaron a los desarrolladores JavaScript a construir aplicaciones web modernas más allá del uso de JavaScript puro (Vainilla JavaScript) y jQuery. La lista de estas populares soluciones no es exhaustiva. Existe una amplia gama de frameworks SPA. Y cuando consideras las fechas de lanzamiento, la mayoría de ellos están en la primera generación de SPAs: Angular 2010, Backbone 2010, Ember 2011.

La versión inicial de React fue lanzada en 2013 por Facebook. React no es un framework SPA, pero sí una librería para la capa vista. Es la V en [MVC](https://es.wikipedia.org/wiki/Modelo%E2%80%93vista%E2%80%93controlador) (modelo vista controlador). Esta librería sólo te permite renderizar componentes como elementos visibles en un navegador. Sin embargo, todo el ecosistema alrededor de React hace que sea posible crear aplicaciones de una sola página.

¿Pero, por qué deberías considerar usar React en vez de la primera generación de frameworks SPA? Mientras que la primera generación de frameworks trató de resolver muchas cosas a la vez, React solo te ayuda a construir tu capa de vista. Es una biblioteca y no un framework. La idea detrás de ella: La capa vista es una jerarquía de componentes ensamblables.

En React, puedes centrarte en la capa vista antes de agregar más aspectos a tu aplicación. Cada otro aspecto es otro componente de la SPA. Estos bloques de construcción son esenciales para construir una aplicación madura, y aportan dos grandes beneficios.

Primero, puedes aprender a usar los bloques de construcción paso a paso. No tienes que preocuparte por entenderlos totalmente. Esto es muy diferente a un framework que te da cada bloque de construcción desde el principio. Este libro se centra en React como el primer bloque de construcción. Más bloques de construcción vendrán eventualmente.

En segundo lugar, todos los bloques de construcción son intercambiables. Esto hace que el ecosistema alrededor de React un sitio muy innovador. Múltiples soluciones compiten entre sí, así que puedes elegir la solución que resulte más atractiva para ti y tu caso de uso.

La primera generación de Frameworks SPA llegó a nivel empresarial, y son más rígidos. React permanece innovador y es adoptado por varias empresas líderes en tecnología cómo [Airbnb, Netflix y por supuesto Facebook](https://github.com/facebook/react/wiki/Sites-Using-React). Todos ellos invierten en el futuro de React y están contentos con la librería y el ecosistema que lo rodea.

Hoy en día, React es probablemente una de las mejores opciones para la construcción de aplicaciones de una sola página. Sólo se encarga de la capa de vista, [pero en conjunto con su ecosistema conforma un framework completo, flexible e intercambiable](https://www.robinwieruch.de/essential-react-libraries-framework/). React cuenta con una API ligera, un ecosistema impresionante y una gran comunidad. Puedes leer acerca de mis experiencias sobre [por qué me mudé de Angular a React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/). Recomiendo encarecidamente entender claramente por qué elegirías React sobre otro framework o biblioteca. Después de todo, todo el mundo está interesado en saber a donde nos llevará React en el 2017 y los años por venir.

### Ejercicios

* Lee acerca de [por qué me cambié de Angular a React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* Lee acerca de [el flexible ecosistema de React](https://www.robinwieruch.de/essential-react-libraries-framework/)
* Lee acerca de [cómo aprender a usar un framework](https://www.robinwieruch.de/how-to-learn-framework/)

## Requerimientos

Antes de que empieces a leer el libro, debes estar familiarizado con los fundamentos del desarrollo web. Se espera que sepas utilizar HTML, CSS y JavaScript. Debes saber también lo que el término [API](https://www.robinwieruch.de/what-is-an-api-javascript/) significa, pues usarás APIs a lo largo del libro. Te animo a que te unas al canal [Slack](https://slack-the-road-to-learn-react.wieruch.com/) oficial del libro para obtener ayuda, ó ayudar a otros.

### Editor y Terminal

¿Y qué hay acerca del entorno de desarrollo? Necesitarás un editor ó IDE y la terminal (línea de comandos). Puedes leer [mi guía de configuración](https://www.robinwieruch.de/developer-setup/) para organizar tus herramientas, está pensada para los usuarios de Mac, sin embargo, todas las herramientas que necesitarás están disponibles para varios sistemas operativos.

Opcionalmente, puedes utilizar Git y GitHub por tu cuenta mientras estes realizando los ejercicios en el libro, para mantener tus projectos en repositorios de GitHub. Aquí dispones de una [pequeña guía]() sobre cómo usar estas herramientas. Sin embargo, no es obligatorio su uso, podría resultar extenuante intentar aprenderlo todo al mismo tiempo.

### Node y npm

Por último, pero no menos importante, necesitarás instalar [Node y npm](https://nodejs.org/es/). Ambos se utilizan para administrar las bibliotecas que necesitarás en el camino para aprender React. Se instalarán paquetes de node externos a través de npm (gestor de paquetes de node por sus siglas en Inglés). Estos paquetes de node pueden ser bibliotecas ó entornos completos.

Puedes verificar la versiones instaladas de Node y npm, respectivamente, en la línea de comandos. Si no obtienes ninguna salida en la terminal, significa que debes verificar tu instalación Node y npm. Estas son mis versiones al momento de escribir este libro:

{title="Command Line",lang="text"}
~~~~~~~~
node --version
*v8.9.4
npm --version
*v5.6.0
~~~~~~~~

## Node y npm

Este capítulo te brinda un pequeño curso intensivo sobre Node y npm. No es exhaustivo, pero obtendrás todas las herramientas necesarias. Si ya estás familiarizado con ellas, puedes omitir este capítulo.

El **manejador de paquetes de Node** (npm) permite instalar **paquetes externos** desde la línea de comandos. Estos paquetes pueden ser un conjunto de funciones de utilidad, bibliotecas o frameworks enteros. Ellos representan dependencias de tu aplicación, y puedes instalar, ya sea en tu carpeta global de paquetes de Node o en la carpeta local de tu proyecto.

Los paquetes globales de Node son accesibles desde cualquier lugar de la terminal y solo hay que instalarlos una vez en el directorio global. Puedes instalar un paquete globalmente escribiendo en la terminal:

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g <package>
~~~~~~~~

La  etiqueta `-g` indica a npm que debe instalar el paquete globalmente. Los paquetes locales son utilizados en tu aplicación. Por ejemplo, React como una librería será un paquete local, y puede ser requerido en tu aplicación para su uso. Puedes instalarlo a través de la terminal escribiendo:

{title="Command Line",lang="text"}
~~~~~~~~
npm install <package>
~~~~~~~~

En el caso de React, sería:

{title="Command Line",lang="text"}
~~~~~~~~
npm install react
~~~~~~~~

El paquete instalado aparecerá automáticamente en una carpeta llamada *node_modules/* y será agregado al archivo *package.json* junto a las otras dependencias de tu aplicación.

Ahora, ¿cómo inicializar la carpeta *node_modules/* y el archivo *package.json* en tu proyecto? Para ello existe un comando que inicia un proyecto npm que incluye automáticamente un archivo *package.json*. Solo cuando tu proyecto posee este archivo, puedes instalar nuevos paquetes locales vía npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

La etiqueta `-y` es un acceso directo para inicializar todos los valores predeterminados en el archivo *package.json* correspondiente. De no utilizarla, tienes que decidir cómo configurar el archivo.

Inmediatamente después de inicializar tu proyecto npm estás listo para instalar nuevos paquetes via `npm install <paquete>`.

Algo más sobre el archivo *package.json*. El archivo permite que compartas tu proyecto con otros desarrolladores sin compartir todos los paquetes node. El archivo contiene todas las referencias de los paquetes node utilizados en tu proyecto. Estos paquetes son llamados dependencias. Cualquiera puede copiar tu proyecto sin las dependencias. Las dependencias son referenciadas en el archivo *package.json*. Alguien que copie tu proyecto puede instalar todos los paquetes usando `npm install` en la línea de comandos.

Quiero cubrir un comando npm más, para prevenir confusiones:

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

La etiqueta `--save-dev` indica que el paquete node es solo usado en el entorno de desarrollo. No será usado en producción cuando cuelgues tu aplicación en un servidor. ¿Qué tipo de paquete sería ese? Imagina que quieres probar tu aplicación con la ayuda de un paquete Node. Necesitas instalar ese paquete a través npm, pero quieres excluirlo de tu entorno de producción. Las pruebas solo se realizan durante el proceso de desarrollo, más no cuando tu aplicación está ya funcionando en producción. En ese nivel debería estar ya probada, y funcionando para todos los usuarios. Este es solo un caso de uso donde querrás usar la etiqueta `--save-dev`. 

Por ahora estos son todos los comandos que necesitas, pero en el camino encontrarás más de ellos.

### Ejercicios:

* configura un proyecto npm
  * crea una nueva carpeta con `mkdir <nombre_de_la_carpeta>`
  * navega hasta la carpeta con `cd <nombre_de_la_carpeta>`
  * ejecuta `npm init -y`
  * instala un paquete local, como React, con `npm install --save react`
  * échale un vistazo al archivo *package.json* y a la carpeta *node_modules/*
  * descubre como desinstalar el paquete node *react*
* lee más sobre [npm](https://docs.npmjs.com/)

## Instalación

Existen varios enfoques a la hora de comenzar con una aplicación React.

El primero, es usar una [red de entrega de contenidos](https://es.wikipedia.org/wiki/Red_de_entrega_de_contenidos), ó mejor conocida cómo CDN (Content Delivery Network). Esto puede sonar más complicado de lo que es. Muchas compañías tienen CDNs que almacenan públicamente archivos para sus usuarios. Archivos que pueden ser una librería, cómo React, después de todo una librería puede ser solo un archivo JavaScript. Se alojan en algún lugar y puedes requerirlas en tu aplicación.

¿Cómo usar un CDN para comenzar con React? Puedes insertar la etiqueta `<script>` en tu código HTML apuntando a la url correspondiente al CDN que deseas utilizar. Para empezar en React necesitas dos archivos (librerías): *react* y *react-dom*.

{title="Code Playground",lang="javascript"}
~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

¿Pero por qué usar un CDN cuando tienes npm para instalar paquetes Node, cómo React?

Cuando tu aplicación tiene un archivo *package.json*, puedes instalar *react* y *react-dom* desde la línea de comandos. El único requisito es que la carpeta se inicialice como  un proyecto npm, utilizando `npm init -y` con un archivo *package.json*. Puedes instalar múltiples paquetes en una línea con npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

El enfoque expuesto anteriormente se utiliza a menudo para añadir React a una aplicación existente que es administrada con npm.

Desafortunadamente eso no es todo. También debes que lidiar con [Babel](http://babeljs.io/) para hacer tu aplicación compatible con JSX (la sintaxis de React) y JavaScript ES6. Babel transpila tu código para que los navegadores puedan interpretar ES6 y JSX. No todos los navegadores son capaces de interpretar la sintaxis.

Siguiendo este enfoque se deben incluir muchas herramientas y configuraciones manualmente, lo cuál puede resultar abrumador para los principiantes, pues toda la configuración de React debe realizarse manualmente.

Por esta razón, Facebook desarrolló *create-react-app*, como una solución que ahorra tiempo y esfuerzo al usuario, encargandose esta de la mayor parte del proceso de configuración.

El siguiente capítulo te mostrará cómo iniciar la construcción de tu aplicación con esta herramienta.

### Ejercicios:

* leer más sobre [instalación de React](https://facebook.github.io/react/docs/installation.html)

## Cero configuraciones

En El Camino para aprender React usarás [create-react-app](https://github.com/facebookincubator/create-react-app) para iniciar con el desarrollo tu aplicación, este kit fué Introducido por Facebook en 2016, te permite rápidamente empezar a trabajar en tu aplicación, sin que debas preocuparte por la configuración. La gente [lo recomienda a los principiantes en un 96%](https://twitter.com/dan_abramov/status/806985854099062785). Cuando utilizas *create-react-app* las herramientas y configuración evolucionan en segundo plano, mientras que el foco está en la implementación de la aplicación.

Para empezar, deberás instalar el paquete a tus paquetes globales node. Después de eso siempre estará disponible en la línea de comandos para inicializar nuevas aplicaciones React.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

En la línea de comandos puedes comprobar la versión de *create-react-app* para verificar que la instalación fue exitosa:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v1.5.1
~~~~~~~~

Ahora, puedes comenzar con el desarrollo de tu primera aplicación react. La llamaremos *hackernews*, sin embargo, puedes escoger un nombre distinto. Iniciar tu aplicación tomará unos pocos segundos. Después de esto, simplemente navega hasta dentro de la carpeta con tu línea de comandos:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

Ahora, puedes abrir la aplicación en tu editor. La siguiente estructura de carpetas o variación de esta depende de la versión de *reate-react-app* que tengas instalada, deberías poder ver algo cómo esto:

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

Una pequeña descripción de cada una de las carpetas y archivos que encontrarás dentro de tu recien creado directorio. Está bien si no entiendes todo al principio.

* **README.md:** La extensión .md indica que el archivo es de formato "markdown". Markdown es un lenguaje de marcado ligero con sintaxis de texto plano. Muchos códigos fuente de proyectos incluyen un archivo *README.md* que te dan instrucciones acerca del proyecto. Cuando estes subiendo tu proyecto a una plataforma cómo GitHub, eventualmente el archivo *README.md* mostrará promisoriamente su contenido cuando alguien accesa al repositorio. Cómo utilizaste *create-react-app*, tu archivo *README.md* debería ser igual al que se puede observar en el [repositorio de GitHub create-react-app](https://github.com/facebookincubator/create-react-app) oficial.

* **node_modules/:** Esta carpeta contiene todos los paquetes de node que han sido instalados via npm. Ya qué utilizaste *create-react-app* para inicializar tu aplicación, en tu nueva carpeta ya debes poder ver un par de módulos de node instalados para ti. Usualmente nunca tocarás esta carpeta, pués, con npm puedes instalar y desinstalar paquetes de node desde la línea de comandos.


* **package.json:** El archivo te muestra una lista de las dependencias de paquetes de node y un conjunto de otras configuraciones referentes al proyecto.

* **.gitignore:** El archivo indica todos los archivos y carpetas que no serán añadidos a tu repositorio de git. Archivos que solo vivirán en el proyecto local.

* **public/:** La carpeta almacena todos los archivos raíz de desarrollo, cómo por ejemplo *public/index.html*, este índice es el que se muestra en la dirección localhost:3000 cuando estás desarrollando tu aplicación. *create-react-app* viene ya configurado para relacionar este archivo índice con todos los scripts en la carpeta *src/*.

* **build/:** La carpeta será creada cuándo el proyecto se esté preparando para la etapa de producción y contiendrá todos tus archivos relacionados a esta étapa de desarrollo. Todo tu código escrito en las carpetas *src/* y *public/* serán empaquetados en un par de archivos y posicionados en la carpeta *build/* cuando tu proyecto se esté compilando.

* **manifest.json** y **registerServiceWorker.js:** Por los momentos no te preocupes acerca de lo que estos archivos hacen, no los necesitaremos en este proyecto. 

Después de todo, no necesitas tocar los archivos y carpetas mencionados antes. En el principio, todo lo que necesitas está localizado en la carpeta *src/*. La mayor atención recae en el archivo *src/App.js* que se utiliza para implementar componentes React. Más adelante podrás dividir tus componentes en múltiples archivos donde cada uno sirve para mantener uno o varios componentes por sí mismo.

Adicionalmente, encontrarás un archivo *src/App.test.js* para pruebas y uno *src/index.js* cómo punto de entrada al mundo de React. Conocerás ambos archivos en un capítulo posterior. Además, hay un archivo *src/index.css* y un archivo *src/App.css* que sirven para darle estilos a tu aplicación y sus componentes. Todos vienen con estilo por defecto cuándo se abren.

La aplicación *create-react-app*  es un proyecto npm. Puedes utilizar npm para instalar y desinstalar paquetes de node en tu proyecto. Y cuenta con los siguientes scripts npm, disponibles en la línea de comandos:

{title="Command Line",lang="text"}
~~~~~~~~
// Ejecuta la aplicacion en http://localhost:3000
npm start

// Ejecuta las pruebas
npm test

// Prepara la aplicación para el nivel de producción
npm run build
~~~~~~~~

Los scripts son definidos en el archivo *package.json*. Tu aplicación React está inicializada, y la mejor parte viene en los ejercicios a continuación, donde finalmente correrás tu aplicación en el navegador.

### Ejercicios:

* ejecuta en la terminal, el comando `npm start`, y visita la aplicación en tu navegador (puedes cancelar la ejecución de este comando presionando Control + C)
* ejecuta el comando interactivo `npm test`
* ejecuta el comando `npm run build` y verifica que la carpeta *build/* sea añadida a tu proyecto (puedes removerla después; nota que la carpeta build puede ser usada más tarde para [colocar tu aplicación en línea](https://www.robinwieruch.de/deploy-applications-digital-ocean/))
* familiarizate con la estrutura de carpetas
* familiarizate con el contenido de los archivos
* lee más sobre [los comandos npm y create-react-app](https://github.com/facebookincubator/create-react-app)

## Introducción a JSX

Ahora conocerás a JSX, la sintaxis empleada por React. Como fué mencionado antes, *create-react-app* ya ha iniciado una aplicación estándar. Todos los archivos vienen con implementaciones predeterminadas. Es tiempo de sumergirnos en el código fuente.

El único archivo que debes tocar al principio será *src/App.js*.

{title="src/App.js",lang=javascript}
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

No te dejes confundir por las instrucciones import/export y la declaración de clases. Estas características pertenecen a JavaScript ES6. Volveremos a ellas en un capítulo posterior.

En el archivo tienes un **componente React ES6 class** con el nombre App. Es una declaración de componente. Básicamente, después de haber declarado un componente, puedes utilizarlo como elemento en cualquier parte de tu aplicación produciendo una **instancia** de tu **componente**, o mejor dicho: Instanciando el componente.

El **elemento** que es devuelto se especifica en el método `render()`. Los elementos son de lo que están hechos los componentes. Es útil comprender las diferencias entre los términos componente, instancia y elemento.

Muy pronto, verás donde el componente App es instanciado. De lo contrario, no podrás ver la salida renderizada en el navegador, ¿verdad? El componente App es sólo la declaración, por si solo no puede ser utilizado. Para utilizar dicho componente podrías instanciar el componente en algún lugar de tu JSX con `<App />`.

El contenido en el bloque de código perteneciente al método `render()` se parece bastante a HTML, pero es JSX. JSX te permite mezclar HTML y JavaScript. Es potente pero confuso cuando estás acostumbrado a HTML simple. Es por eso que un buen punto de partida, es utilizar HTML básico en tu JSX. A continuación, puedes empezar a insertar expresiones JavaScript entre llaves.

Primero vamos a eliminar todo el contenido distractor en el archivo.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>Welcome to the Road to learn React</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Ahora, el método `render()` sólo devuelve HTML sin JavaScript. Vamos a definir "Bienvenido al camino para aprender React" cómo el valor de una variable, que puede ser utilizad en JSX usando corchetes.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    var helloWorld = 'Bienvenido al camino para aprender React';
# leanpub-end-insert
    return (
      <div className="App">
# leanpub-start-insert
        <h2>{helloWorld}</h2>
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

Cuándo inicies tu aplicación desde la linea de comandos con `npm start` todo debería funcionar.

Además, probablemente notaste el atributo `className`, este refleja el atributo estándar `class` en HTML. Debido a razones técnicas, JSX tuvo que reemplazar un puñado de atributos HTML internos. Puedes encontrar todos los [atributos HTML compatibles en la documentación de React](https://facebook.github.io/react/docs/dom-elements.html). En tu camino para aprender React te encontrarás con más atributos JSX.

### Ejercicios:

* define más variables y renderízalas en tu JSX
  * utiliza un objeto complejo para representar a un usuario con nombre y apellido
* lee más sobre [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)
* lee más sobre [componentes React, elementos e instancias](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)

## ES6 const y let

Supongo que notaste que declaramos la variable `helloWorld` con `var`. JavaScript ES6 viene con dos opciones más para declarar sus variables: `const` y `let`. En JavaScript ES6 rara vez encontrarás `var`. Daremos una explicación para `const` y `let`:

Una variable declarada con `const` no se puede volver a asignar o volver a declarar. No puede ser mutada (cambiada, modificada). Adoptas estructuras de datos inmutables al usarlo. Una vez que se define la estructura de datos, no se puede cambiar.

```js
// No permitido
const helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
```

Una variable declarada con `let` puede mutar.

```js
// Permitido
let helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
```

Lo usarás cuando necesites reasignar una variable.

Sin embargo, hay que tener cuidado con `const`. Una variable declarada con `const` no se puede modificar. Pero cuando la variable es un array o un objeto, el valor que tiene puede ser alterado. El valor que tiene no es inmutable.

```js
// allowed
const helloWorld = {
  text: 'Welcome to the Road to learn React'
};
helloWorld.text = 'Bye Bye React';
```

Pero ¿cuando usar cada una? Hay diferentes opiniones sobre su uso. Sugiero usar `const` cada que puedas. Indica que desea mantener su estructura de datos inmutable aunque los valores en objetos y arreglos se pueden modificar. Si desea modificar la variable, puede utilizar `let`.

La inmutabilidad es adoptada en React y su ecosistema. Es por eso que `const` debe ser tu opción por defecto cuando se define una variable. Sin embargo, en objetos complejos los valores internos pueden ser modificados. Ten cuidado con este comportamiento.

En su aplicación debe utilizar `const` sobre `var`.

```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    const helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
        <h2>{helloWorld}</h2>
      </div>
    );
  }
}

export default App;
```

### Ejercicios:

* lee más sobre ES6 [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
* lee más sobre ES6 [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
* investigue más sobre estructuras de datos inmutables
  * ¿Por qué tienen sentido en la programación en general?
  * ¿Por qué se utilizan en React y su ecosistema?

## ReactDOM

Antes de continuar con el componente App, es posible que desee ver dónde se utiliza. Se encuentra en tu punto de entrada al mundo de React: el archivo *src/index.js*.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

Básicamente `ReactDOM.render()` usa un nodo DOM en tu HTML para reemplazarlo con tu JSX. Así es como puedes integrar fácilmente React en cualquier aplicación externa. No está prohibido utilizar `ReactDOM.render()` varias veces en tu aplicación. Puedes utilizarlo en varios lugares para iniciar la sintaxis JSX simple, un componente React, múltiples componentes React o una aplicación completa.

`ReactDOM.render()` espera dos argumentos.

El primer argumento es JSX que se renderizara. El segundo argumento especifica el lugar en el que la aplicación React se insertará en tu código HTML. Espera un elemento con un `id='root'`. Puedes abrir tu archivo *public/index.html* para encontrar el atributo id.

En la implementación `ReactDOM.render()` ya tiene tu componente App. Sin embargo, sería bueno pasar JSX más simple, siempre y cuando sea JSX. No tiene que ser una instancia de un componente.

```js
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
```

### Ejercicios:

* abre el *public/index.html* para ver dónde se conectan las aplicaciones React en tu HTML
* lee más sobre [renderizando elementos en React](https://facebook.github.io/react/docs/rendering-elements.html)

## Módulo Hot Reloading

Hay una cosa que usted puede hacer en el archivo *src/index.js* para mejorar su experiencia como desarrollador.

En *create-react-app* ya es una ventaja que el navegador actualice automáticamente la página cuando cambia el código fuente. Inténtelo cambiando la variable `helloWorld` en tu archivo *src/App.js*. El navegador debe actualizar la página. Pero puedes hacerlo mejor.

Modulo Hot Reloading (HMR) es una herramienta para recargar su aplicación en el navegador. El navegador no actualiza la página. Puede activarlo fácilmente en *create-react-app*. En tu *src/index.js* - tu punto de entrada de React -  tienes que agregar una pequeña configuración.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
if (module.hot) {
  module.hot.accept()
}
```

Eso es todo. Intente cambiar de nuevo la variable `hellowWorld` en tu archivo *src/App.js*. El navegador no debe actualizar la página, pero la aplicación vuelve a cargar y muestra la salida correcta. HMR viene con múltiples ventajas:

Imagine que está depurando su código con declaraciones `console.log()`. Estas declaraciones permanecerán en la consola de desarrolladores, incluso si cambia su código, porque el navegador ya no actualiza la página. Eso puede ser conveniente para fines de depuración.

En una aplicación en crecimiento, una actualización de página demora su productividad. Tienes que esperar hasta que la página se cargue. Una recarga de página puede tardar varios segundos en una aplicación grande. HMR quita esta desventaja.

El mayor beneficio es que puede mantener el estado de la aplicación con HMR. Imagine que tiene un diálogo en su aplicación con varios pasos y que está en el paso 3. Básicamente es un mago. Sin HMR cambiaría el código fuente y su navegador actualizará la página. Usted tendría que abrir el cuadro de diálogo de nuevo y sin tener que navegar desde el paso 1 al paso 3. Con HMR su diálogo permanece abierto en el paso 3. Mantiene el estado de la aplicación aunque el código fuente cambie. La aplicación misma se vuelve a cargar, pero no la página.

### Ejercicios:

* cambia el código fuente de *src/App.js* algunas veces para ver HMR en acción
* ver los primeros 10 minutos de [Live React: Hot Reloading with Time Travel](https://www.youtube.com/watch?v=xsSnOQynTHs) po Dan Abramov

## JavaScript complejo en JSX

Volvamos al componente App. Hasta ahora se han renderizado algunas variables primitivas en su JSX. Ahora usted comenzará a renderizar una lista de artículos.La lista será datos artificiales en el principio, pero más tarde obtendrás los datos de una API externa. Eso será mucho más emocionante.

Primero tienes que definir la lista de elementos.

```js
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
```

Los datos artificiales reflejarán los datos que extraeremos más adelante de la API. Un elemento de la lista tiene un título, una url y un autor. Además viene con un identificador, puntos (que indican la popularidad de un artículo) y un recuento de comentarios.

Ahora puede utilizar la función de `map` JavaScript incorporada en su JSX. Te permite iterar sobre tu lista de elementos para mostrarlos. Como se mencionó, usará llaves para encapsular la expresión JavaScript en su JSX.

```js
class App extends Component {

  render() {
    return (
      <div className="App">
        { list.map(function(item) {
          return <div>{item.title}</div>;
        })}
      </div>
    );
  }
}

export default App;
```

Eso es bastante poderoso en JSX. Por lo general, es posible que haya utilizado `map` para convertir una lista de elementos a otra lista de elementos. Esta vez utiliza `map` para convertir una lista de elementos en elementos HTML.

Hasta ahora, sólo se mostrará el `título` de cada elemento. Pero vamos a mostrar algunas más de las propiedades del artículo.

```js
class App extends Component {

  render() {
    return (
      <div className="App">
        { list.map(function(item) {
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
```

Puede ver cómo la función map está simplemente en línea en su JSX. Cada propiedad de elemento se muestra en una etiqueta `<span>`. Además, la propiedad url del elemento se utiliza en el atributo `href` de la etiqueta de anclaje.

React hará todo el trabajo por usted y mostrara cada elemento. Pero debe agregar un helper para que React alcance su máximo potencial y mejore su rendimiento. Debe asignar un atributo key a cada elemento de lista. Así, React es capaz de identificar los elementos añadidos, cambiados y eliminados cuando la lista cambia. Los elementos artificiales de la lista ya vienen con un identificador.

```js
{
  list.map(function(item) {
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
  });
}
```

Debe asegurarse de que el atributo key es un identificador estable. No cometa el error de usar el índice de elementos en el array. El índice del array no es del todo estable. Por ejemplo, cuando la lista cambia su orden, React tendrá dificultades para identificar los elementos correctamente.

```js
// don't do this
{ list.map(function(item, key) {
  return (
    <div key={key}>
      ...
    </div>
  );
})}
```

Ahora mostrarás los dos elementos de la lista. Puedes iniciar tu aplicación, abrir tu navegador y ver los dos elementos de la lista desplegados.

### Ejercicios:

* lee más sobre [React lists and keys](https://facebook.github.io/react/docs/lists-and-keys.html)
* recapitular el [standard built-in Array functionalities in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
* Utilice más expresiones JavaScript por tu cuenta en JSX

## ES6 Arrow Functions

JavaScript ES6 introduce arrow functions. Una expresión arrow function es más corta que una expresión de funcion.

```js
// function expression
function () { ... }

// arrow function expression
() => { ... }
```

Pero tienes que ser consciente de sus funcionalidades. Uno de ellos es un comportamiento diferente con el objeto `this`. Una function expression siempre define su propio objeto `this`. Arrow function expressions aún tienen el objeto `this` del contexto cerrado. No se confunda al usar `this` en una arrow function.

Hay otro hecho valioso sobre las arrow function con respecto al paréntesis. Puedes quitar los paréntesis cuando la función sólo obtiene un argumento, pero tienen que mantenerlos cuando tengas múltiples argumentos.

```js
// allowed
item => { ... }

// allowed
(item) => { ... }

// not allowed
item, key => { ... }

// allowed
(item, key) => { ... }
```

Sin embargo, echemos un vistazo a la función `map`. Puedes escribirla más concisamente con una ES6 arrow function.

```js
{ list.map(item => {
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
```

Además, puede eliminar el *cuerpo del bloque* de la ES6 arrow function. En un *cuerpo conciso* un return implícito se adjunta así que usted puede quitar la declaración return. Eso sucederá más a menudo en el libro, así que asegúrese de entender la diferencia entre un cuerpo de bloque y un cuerpo conciso.

```js
{ list.map(item =>
  <div key={item.objectID}>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>
)}
```

Ahora su JSX se ve más conciso y legible. Omite la sentencia function, las llaves y la declaración return.

### Ejercicios:

* lee más sobre [ES6 arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## ES6 Classes

JavaScript ES6 introdujo clases. Una clase se utiliza comúnmente en lenguajes de programación orientados a objetos. JavaScript fue y es muy flexible en sus paradigmas de programación. Puedes hacer programación funcional y programación orientada a objetos lado a lado para sus casos de uso particulares.

Aunque React adopta la programación funcional, por ejemplo con estructuras de datos inmutables, las clases se utilizan para declarar componentes. Se les llama componentes clase ES6. React mezcla las partes buenas de ambos paradigmas de programación.

Consideremos la siguiente clase Developer para examinar una clase de JavaScript ES6 sin pensar en un componente

```js
class Developer {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  getName() {
    return this.firstname + ' ' + this.lastname;
  }
}
```

Una clase tiene un constructor para hacerla instanciable. El constructor puede tomar argumentos para asignarlos a la instancia de clase. Además, una clase puede definir funciones. Dado que la función está asociada a una clase, se llama método. A veces se referencia como un metodo de clase.

La clase Developer es sólo la declaración de clase. Puedes crear varias instancias de la clase invocándola. It is similar to the ES6 class component, que tiene una declaración, pero tienes que usarlo en otro lugar para instanciarlo.

Veamos cómo se puede instanciar la clase y cómo se pueden utilizar sus métodos.

```js
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
```

React utiliza clases de JavaScript ES6 para componentes de clase ES6. Ya has utilizado un componente de clase ES6.

```js
import React, { Component } from 'react';

...

class App extends Component {
  render() {
    ...
  }
}
```

La clase App se extiende de `Component`. Básicamente, se declara el componente App, pero se extiende desde otro componente.¿Qué significa extender? En la programación orientada a objetos tienes el principio de herencia. Se utiliza para pasar funcionalidades de una clase a otra clase.

La clase App extiende la funcionalidad de la clase Component. Para ser más específico, hereda funcionalidades de la clase Component. El componente se utiliza para extender una clase ES6 básica a una clase de componente ES6. Tiene todas las funcionalidades que un componente necesita tener. Una de estas funcionalidades, un método, que ya utilizaste: el metodo `render()`. Pero aprenderás más funcionalidades.

La clase `Component` encapsula todas las funcionalidades de React que un desarrollador no necesita ver. Permite a los desarrolladores utilizar clases como componentes en React.

Los métodos que un `Component` React expone es la interfaz pública. Uno de estos métodos tiene que sobrescribirse, los otros no necesitan ser sobrescritos. Aprenderá acerca de estos últimos cuando el libro llegue a los métodos del ciclo de vida en un capítulo posterior. El metodo `render()`iene que ser sobrescrito, porque define la salida de un React `Component`.

Ahora conoce los conceptos básicos de las clases de JavaScript ES6 y cómo se utilizan en React para extenderlas a componentes. Como dije, aprenderá más sobre los métodos de Componente cuando el libro describa los métodos del ciclo de vida React.

### Ejercicios:

* lee más sobre [ES6 classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)


Has aprendido a arrancar tu propia aplicación React! Repasemos los últimos capítulos:

* React
  * create-react-app arranca una aplicación React
  * JSX mezcla HTML y JavaScript para definir los componentes React
  * componentes, instancias y elementos son cosas diferentes
  * `ReactDOM.render()` es un punto de entrada para una aplicación React
  * funciones JavaScript incorporadas se pueden utilizar en JSX
    * map puede utilizarse para renderizar una lista de elementos como elementos HTML
* ES6
  * Declaraciones de variables con `const` y `let` para casos de uso particulares
  * arrow functions pueden utilizarse para acortar las declaraciones de funciones
  * las clases se utilizan para definir componentes en React

Tiene sentido hacer una pausa en este punto. Internalizar lo aprendido y aplicarlos por su cuenta. Puedes experimentar con el código fuente que ha escrito hasta ahora.

Puede encontrar el código fuente en el [repositorio oficial](https://github.com/rwieruch/hackernews-client/tree/0c5a701170dcc72fe68bdd594df3a6522f58fbb3).
