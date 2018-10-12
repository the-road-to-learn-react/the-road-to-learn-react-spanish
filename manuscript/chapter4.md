# Organización de Código y Pruebas

El capítulo se centrará en temas importantes para lograr que  el código sea mantenible en una aplicación escalable. Aprenderás sobre la organización del código para adoptar las mejores prácticas al estructurar las carpetas y archivos. Otro aspecto que aprenderás son las pruebas, lo cual es importante para mantener un código robusto.

## Módulos ES6: Importación y Exportación

En JavaScript ES6 puedes importar y exportar funcionalidades desde módulos. Estas funcionalidades pueden ser funciones, clases, componentes, constantes, etc. Básicamente todo lo que puede asignar a una variable. Los módulos pueden ser archivos individuales o carpetas enteras con un archivo de índice como punto de entrada.

Al principio del libro, después de haber iniciado la aplicación con *create-react-app* , existían varios `import` y `export` a través de los archivos iniciales. Ahora es el momento apropiado para explicar esto.

Las sentencias `import` y `export` ayudan a compartir código en varios archivos. Antes había varias soluciones para esto en el entorno JavaScript. Fue un desastre, porque no había una manera estándar para hacerlo, en la actualidad, es un comportamiento nativo de Javascript ES6.

Además, estas declaraciones adoptan la división de código. Se distribuye el código a través de varios archivos para mantenerlo reutilizable y mantenible. Lo primero es cierto porque puede importar el fragmento de código en varios archivos. Esto último es cierto porque usted tiene una sola fuente donde usted mantiene el pedazo de código.

Por último, pero no menos importante, le ayuda a pensar en encapsulación de código. No es necesario que todas las funcionalidades se exporten desde un archivo. Algunas de estas funcionalidades sólo deben utilizarse en el archivo donde se han definido. Las exportaciones de un archivo son básicamente la API pública del archivo. Sólo las funcionalidades exportadas están disponibles para ser reutilizadas en otro lugar. Sigue la mejor práctica de encapsulación.

Pero seamos prácticos. Como funcionan las declaraciones `import` y `export`? Los ejemplos siguientes muestran las declaraciones compartiendo una o varias variables entre dos archivos. Al final, el enfoque puede escalar a varios archivos y podría compartir más que simples variables.

Puede exportar una o varias variables. Se llama una exportación con nombre.

```js
const firstname = 'robin';
const lastname = 'wieruch';

export { firstname, lastname };
```

Y los importa en otro archivo con una ruta de acceso relativa al primer archivo.

```js
import { firstname, lastname } from './file1.js';

console.log(firstname);
// output: robin
```

También puede importar todas las variables exportadas de otro archivo como un objeto.

```js
import * as person from './file1.js';

console.log(person.firstname);
// output: robin
```

Las importaciones pueden tener un alias. Puede ocurrir que importe funcionalidades de varios archivos que tengan el mismo nombre de exportación. Es por eso que puedes usar un alias.

```js
import { firstname as foo } from './file1.js';

console.log(foo);
// output: robin
```

Por último pero no menos importante existe la declaración `default`. Se puede utilizar para algunos casos de uso:

* exportar e importar una sola funcionalidad
* para resaltar la funcionalidad principal de la API exportada de un módulo
* tener una funcionalidad de importación alternativa


```js
const robin = {
  firstname: 'robin',
  lastname: 'wieruch',
};

export default robin;
```

```js
import developer from './file1.js';

console.log(developer);
// output: { firstname: 'robin', lastname: 'wieruch' }
```

El nombre de importación puede diferir del nombre predeterminado exportado. También puede utilizarlo junto con las instrucciones de exportación e importación nombradas.

```js
const firstname = 'robin';
const lastname = 'wieruch';

const person = {
  firstname,
  lastname,
};

export {
  firstname,
  lastname,
};

export default person;
```

```js
import developer, { firstname, lastname } from './file1.js';

console.log(developer);
// output: { firstname: 'robin', lastname: 'wieruch' }
console.log(firstname, lastname);
// output: robin wieruch
```

En las exportaciones nombradas puede ahorrar líneas adicionales y exportar las variables directamente.

```js
export const firstname = 'robin';
export const lastname = 'wieruch';
```

Estas son las principales funcionalidades de los módulos ES6. Te ayudan a organizar tu código, a mantener tu código y a diseñar API de módulos reutilizables. También puedes exportar e importar funcionalidades para probarlas. Lo harás en uno de los siguientes capítulos.

### Ejercicios:

* leer mas sobre [ES6 import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
* leer mas sobre [ES6 export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

## Organización de código con módulos ES6

Podrias preguntarte: ¿Por qué no seguimos las mejores prácticas de división de código para el archivo *src/App.js*? En el fichero ya tenemos múltiples componentes que podrían definirse en sus propios ficheros/carpetas(módulos). Por el bien de aprender React, es práctico guardar estas cosas en un lugar. Pero una vez que su aplicación React crece, debes considerar dividir estos componentes en varios módulos. Sólo así escalara tu aplicación.

A continuación te propongo varias estructuras de módulos que  *podrías* aplicar. Yo recomendaría aplicarlos como un ejercicio al final del libro. Para mantener el libro en sí simple, no realizaré la división del código y seguiré los siguientes capítulos con el archivo *src/App.js*

Una posible estructura de módulo podría ser:

```
src/
  index.js
  index.css
  App.js
  App.test.js
  App.css
  Button.js
  Button.test.js
  Button.css
  Table.js
  Table.test.js
  Table.css
  Search.js
  Search.test.js
  Search.css
```

No parece muy prometedor. Puede ver un montón de nombres duplicados y sólo difiere la extensión de archivo. Otra estructura de módulo podría ser:


```
src/
  index.js
  index.css
  App/
    index.js
    test.js
    index.css
  Button/
    index.js
    test.js
    index.css
  Table/
    index.js
    test.js
    index.css
  Search/
    index.js
    test.js
    index.css
```

Parece más limpio que el anterior. Un componente se define por su declaración de componente en el archivo JavasScript, pero también por su estilo y pruebas.

Otro paso podría ser extraer las variables constantes del componente App. Estas constantes se utilizaron para componer el URL de la API de Hacker News.

```
src/
  index.js
  index.css
# leanpub-start-insert
  constants/
    index.js
  components/
# leanpub-end-insert
    App/
      index.js
      test.js
      index..css
    Button/
      index.js
      test.js
      index..css
    ...
```

Naturalmente, los módulos se dividirían en *src/constants/* y *src/components/*.

Ahora el archivo *src/constants/index.js* podría ser similar al siguiente:

```js
export const DEFAULT_QUERY = 'redux';
export const DEFAULT_PAGE = 0;
export const DEFAULT_HPP = '100';

export const PATH_BASE = 'https://hn.algolia.com/api/v1';
export const PATH_SEARCH = '/search';
export const PARAM_SEARCH = 'query=';
export const PARAM_PAGE = 'page=';
export const PARAM_HPP = 'hitsPerPage=';
```

El archivo *App/index.js* podría importar estas variables para poder usarlas.

```js
import {
  DEFAULT_QUERY,
  DEFAULT_PAGE,
  DEFAULT_HPP,

  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
} from '../constants/index.js';

...
```

Cuando utilices la convencion de nombrado de *index.js*,  puedes omitir el nombre de archivo de la ruta relativa.

```
import {
  DEFAULT_QUERY,
  DEFAULT_PAGE,
  DEFAULT_HPP,

  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
# leanpub-start-insert
} from '../constants';
# leanpub-end-insert

...
```

Pero ¿qué hay detrás del nombrado del archivo *index.js*? La convención se introdujo en el mundo node.js. El archivo de index es el punto de entrada a un módulo. Describe la API pública al módulo. Los módulos externos sólo pueden usar el archivo *index.js* para importar el código compartido del módulo. Considere la siguiente estructura de módulo compuesta para demostrarlo:

```
src/
  index.js
  App/
    index.js
  Buttons/
    index.js
    SubmitButton.js
    SaveButton.js
    CancelButton.js
```

La carpeta *Buttons/* tiene varios componentes botón definidos en archivos distintos. Cada archivo puede `export default`  el componente específico que lo hace disponible para *Buttons/index.js*. El archivo *Buttons/index.js* importa todas las representaciones de botones diferentes y las exporta como API de módulo público.

```js
import SubmitButton from './SubmitButton';
import SaveButton from './SaveButton';
import CancelButton from './CancelButton';

export {
  SubmitButton,
  SaveButton,
  CancelButton,
};
```

Ahora el *src/App/index.js*  puede importar los botones de la API del módulo público ubicada en el archivo *index.js*.

```
import {
  SubmitButton,
  SaveButton,
  CancelButton
} from '../Buttons';
```

Al ir con esta restricción, sería una mala práctica para llegar a otros archivos mas que el *index.js* en el modulo. Rompería las reglas de la encapsulación.

```
// bad practice, don't do it
import SubmitButton from '../Buttons/SubmitButton';
```

Ahora ya sabes cómo podrías refactorizar tu código fuente en módulos con las restricciones de la encapsulación. Como he dicho, por el bien de mantener el tutorial simple no voy a aplicar estos cambios. Pero tu debes hacer la refactorización al final del libro.

### Ejercicios:

* refactorizar tu archivo *src/App.js* en módulos de múltiples componentes al finalizar el libro

## Interfaz de componentes con PropTypes

Deberias conocer [TypeScript](https://www.typescriptlang.org/) o [Flow](https://flowtype.org/) para introducir una interfaz de tipo a JavaScript. Un lenguaje tipado es menos propenso a errores. Los editores y otras utilidades pueden detectar estos errores antes de que se ejecute el programa. Esto hace que su programa sea más robusto.

React viene con un comprobador de tipo incorporado para evitar errores. Puede utilizar PropTypes para describir la interfaz de componentes. Todas las props que se pasan de un componente principal a un componente secundario se validan basándose en la interfaz PropTypes asignada al componente secundario.

El capítulo te mostrará cómo puedes hacer que todos tus componentes sean seguros con PropTypes. Omitiré los cambios para los siguientes capítulos, porque agregan refactorings innecesarios de código. Pero debe mantenerlos y actualizarlos a lo largo del camino para mantener su tipo de interfaz de componentes seguro.

Inicialmente puede importar PropTypes. Tienes que ser cuidadoso de tu versión de React, porque en React versión 15.5 la importación cambió. Revisa tu *package.json* para encontrar tu versión de React.

Si es 15.5 o más, tiene que instalar un paquete independiente.

```
npm install --save prop-types
```

Si su versión es 15.4 o inferior, puede utilizar el paquete React ya instalado.

Ahora, dependiendo de su versión, puede importar PropTypes.

```
// React 15.5 y superior
import PropTypes from 'prop-types';

// React 15.4 e inferior
import React, { Component, PropTypes } from 'react';
```


Comencemos a asignar una interfaz de props a los componentes:

```js
const Button = ({ onClick, className = '', children }) =>
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

Button.propTypes = {
  onClick: PropTypes.func,
  className: PropTypes.string,
  children: PropTypes.node,
};
```

Eso es. Se toma cada argumento de la función y se le asigna un PropType. Los PropTypes básicos para primitivos y objetos complejos son:

```
* PropTypes.array
* PropTypes.bool
* PropTypes.func
* PropTypes.number
* PropTypes.object
* PropTypes.string
```


Además, tiene dos propTypes más para definir un fragmento renderizable (nodo), por ejemplo, una cadena y un elemento React.

```
* PropTypes.node
* PropTypes.element
```

Ya usaste el PropType  `node`  para el componente Button. En general hay más definiciones de PropType que puede leer en la documentación oficial de React.

At the moment all of the defined PropTypes for the Button are optional. The parameters can be null or undefined. But for several props you want to enforce that they are defined. You can make it a requirement that these props are passed to the component.
Por el momento todos los propTypes definidos para el botón son opcionales. Los parámetros pueden ser nulos o no definidos. Pero para varios props deseas que se definan. Puedes hacer que sea un requisito que estas props se pasen al componente.

```
Button.propTypes = {
  onClick: PropTypes.func.isRequired,
  className: PropTypes.string,
  children: PropTypes.node.isRequired,
};
```

The `className` is not required, because it can default to an empty string. Next you will define a PropType interface for the Table component:
El `className` no es necesario, ya que puede predeterminarse a una cadena vacía. A continuación, definirás una interfaz PropType para el componente Table:

```
Table.propTypes = {
  list: PropTypes.array.isRequired,
  onDismiss: PropTypes.func.isRequired,
};
```

Puedes definir el contenido de una matriz PropType más explícitamente:

```
Table.propTypes = {
  list: PropTypes.arrayOf(
    PropTypes.shape({
      objectID: PropTypes.string.isRequired,
      author: PropTypes.string,
      url: PropTypes.string,
      num_comments: PropTypes.number,
      points: PropTypes.number,
    })
  ).isRequired,
  onDismiss: PropTypes.func.isRequired,
};
```

Solo el `objectID` es requerido, porque tu sabes que parte de su código depende de ello. Las otras propiedades sólo se muestran, por lo que no son necesarias. Además, no puedes estar seguro de que la API de Hacker News siempre tenga una propiedad definida para cada objeto del array.

Eso es para PropTypes. Pero hay un aspecto más. Puede definir props predeterminadas en tu componente. Vamos a tomar de nuevo el componente Button. Las propiedas `className` tienen un parámetro predeterminado de ES6 en la firma de componente.

```
const Button = ({ onClick, className = '', children }) =>
  ...
```

Puede sustituirlo por el Prop de defecto interno de React:

```
const Button = ({ onClick, className, children }) =>
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

Button.defaultProps = {
  className: '',
};
```

Igual que el parámetro predeterminado de ES6, el valor predeterminado garantiza que la propiedad se establece en un valor predeterminado cuando el componente primario no lo especifica. La comprobación de tipo PropType ocurre después de evaluar el valor predeterminado.

### Ejercicios:

* respondete las siguientes preguntas
* ¿el componente App tiene una interfaz PropType?
* definir la interfaz PropType para el componente Search
* agregue y actualice las interfaces PropType cuando agregue y actualice componentes en los próximos capítulos
* eer más sobre [React PropTypes](https://facebook.github.io/react/docs/typechecking-with-proptypes.html)

## Pruebas instantáneas con Jest

[Jest](https://facebook.github.io/jest/) es un framework JavaScript de pruebas. En Facebook se utiliza para validar el código JavaScript. En la comunidad React se utiliza para la cobertura de pruebas de componentes React . Por suerte *create-react-app*  ya viene con Jest.

Comencemos a probar sus primeros componentes. Antes de que pueda hacer eso, tiene que exportar los componentes de su archivo *src/App.js* para testearlos en un archivo diferente.

```
...

class App extends Component {
  ...
}

...

export default App;

export {
  Button,
  Search,
  Table,
};
```

En tu archivo *App.test.js* encontrarás una primera prueba. Comprueba que el componente App se renderiza sin errores.

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
});
```
Puedes ejecutarlo mediante el comando interactivo *create-react-app* en la línea de comandos.
```
npm run test
```

Ahora Jest te permite escribir pruebas de instantáneas. Estas pruebas realizan una instantánea del componente representado y ejecutan esta instantánea contra futuras instantáneas. Cuando cambia una instantánea futura, se le notificará durante la prueba. Puedes aceptar el cambio de instantánea, ya que cambió la implementación del componente a propósito, o denegar el cambio e investigar por un error.

Jest almacena las instantáneas en una carpeta. Sólo de esa manera puede mostrar las diferencias a futuras instantáneas. Además, las instantáneas pueden compartirse entre equipos.

Debes instalar una biblioteca de utilidades antes de poder escribir su primera prueba de instantánea.

```
npm install --save-dev react-test-renderer
```

Ahora puede ampliar la prueba del componente App con su primera prueba instantánea.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
import App from './App';

describe('App', () => {

  it('renders', () => {
    const div = document.createElement('div');
    ReactDOM.render(<App />, div);
  });

  test('snapshots', () => {
    const component = renderer.create(
      <App />
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
```

Vuelva a ejecutar sus pruebas y verifica cómo las pruebas tienen éxito o fallan. Deben tener éxito. Una vez que cambie la salida del bloque de render en su componente App, la prueba de instantánea debe fallar. A continuación, puede decidir actualizar la instantánea o investigar en el componente de la aplicación.

Vamos a agregar más pruebas para nuestros componentes independientes. En primer lugar, el componente de búsqueda:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
import App, { Search } from './App';
...

describe('Search', () => {

  it('renders', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Search>Search</Search>, div);
  });

  test('snapshots', () => {
    const component = renderer.create(
      <Search>Search</Search>
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
```

En segundo lugar, el componente Button:

```
...
import App, { Search, Button } from './App';
...

describe('Button', () => {

  it('renders', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Button>Give Me More</Button>, div);
  });

  test('snapshots', () => {
    const component = renderer.create(
      <Button>Give Me More</Button>
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
```

Por último, pero no menos importante, el componente Tabla:

```
...
import App, { Search, Button, Table } from './App';
...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  it('renders', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Table { ...props } />, div);
  });

  test('snapshots', () => {
    const component = renderer.create(
      <Table { ...props } />
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
```
Las pruebas instantáneas generalmente se mantienen bastante básicas. Sólo desea cubrir que el componente no cambia su salida. Una vez que cambia la salida, tiene que decidir si acepta los cambios. De lo contrario, tendrá que fijar el componente cuando la salida no sea la salida deseada.

### Ejercicios:

* vea cómo fallan las pruebas de instantánea una vez que cambia la implementación de su componente
  * aceptar o denegar el cambio de instantánea
* manten tus pruebas de instantáneas actualizadas cuando la implementación cambie en los próximos capítulos
* leer mas sobre [Jest in React](https://facebook.github.io/jest/docs/tutorial-react.html)

## Pruebas unitarias con Enzyme

[Enzyme](https://github.com/airbnb/enzyme) es una utilidad de prueba de Airbnb para afirmar, manipular y recorrer sus componentes React. Puede utilizarlo para realizar pruebas de unidad para complementar sus pruebas de instantánea.

Vamos a ver cómo se puede utilizar enzyme. Primero tienes que instalarlo ya que no viene con *create-react-app*.

```
npm install --save-dev enzyme react-addons-test-utils
```

Ahora puede escribir su primera prueba de unidad en el bloque de descripción de la tabla. Usaras `shallow()` para renderizar tu componente y asegurarte que la tabla tiene dos elementos.


```
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
import { shallow } from 'enzyme';
import App, { Search, Button, Table } from './App';

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  ...

  it('shows two items in list', () => {
    const element = shallow(
      <Table { ...props } />
    );

    expect(element.find('.table-row').length).toBe(2);
  });

});
```

Shallow renderiza el componentes sin componentes secundarios. Puedes hacer la prueba muy dedicada a un componente.

Enzyme tiene tres mecanismos de renderización en su API. Ya conoes `shallow()`, pero también existen `mount()` y `render()`. Ambos instancian instancias del componente principal y todos los componentes secundarios. Adicionalmente `mount()` te da más acceso a los métodos de ciclo de vida de los componentes. Pero, ¿cuándo utilizar qué mecanismo de renderización? Aquí algunas reglas básicas:

* Comience siempre con una prueba superficial `shallow()`
* Si `componentDidMount()` o `componentDidUpdate()` deben ser testeados, usa `mount()`
* Si deseas testear el ciclo de vida de los componentes y el comportamiento de los hijos, utiliza  `mount()`
* Si deseas testear el renderizado de componentes hijos con menos gastos que `mount()` y  no estás interesado en los métodos del ciclo de vida, use `render()`.

Usted podría continuar a la unidad de prueba de sus componentes. Pero asegúrese de mantener las pruebas simples y mantenibles. De lo contrario tendrá que refactorizarlos una vez que cambie sus componentes. Es por eso que Facebook introdujo las pruebas de Snapshot con Jest en primer lugar.

### Ejercicios:

* mantenga sus pruebas de unidad actualizadas durante los siguientes capítulos
* leer más sobre  [enzyme and its rendering API](https://github.com/airbnb/enzyme)


¡Has aprendido cómo organizar tu código y cómo probarlo! Repasemos los últimos capítulos:

* React
  * PropTypes te permite definir controles de tipado de componentes
  * Jest le permite escribir pruebas instantáneas para sus componentes
  * Enzyme le permite escribir pruebas unitarias para sus componentes
* ES6
  * instrucciones de importación y exportación le ayudan a organizar su código
* General
  * La organización de código te permite escalar tu aplicación con las mejores prácticas

Puede encontrar el código fuente en e [repositorio oficial](https://github.com/rwieruch/hackernews-client/tree/393ce5a350aa34b1c7ae056333f7bb7b0807caef).