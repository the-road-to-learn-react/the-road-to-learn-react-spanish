# Componentes React Avanzados

Este capítulo se enfoca en la implementación de componentes React avanzados. Aprenderás sobre componentes de orden superior y cómo implementarlos, navegando aún más profundo en el ecosistema React.

## Ref a DOM Element

A veces necesitas interactuar con tus nodos DOM en React. El atributo `ref` te da acceso a un nodo dentro de tus elementos. Por lo general, ese se conoce como un antipatrón Reat, porque debes usar su forma declarativa de hacer las cosas y su flujo de datos unidireccional. Aprendiste sobre esto cuando se introdujo el primer campo de busqueda, pero hay ciertos casos donde necesitas acceso al nodo DOM. La documentación oficial menciona tres de estos casos de uso:

* usar la API DOM (focus, media playback etc.)
* invocar animaciones de nodo DOM imperativas
* para integrar con una biblioteca de terceros que necesita el nodo DOM (por ejemplo [D3.js](https://d3js.org/))

Hagámoslo por ejemplo con el componente de búsqueda. Cuando la aplicación se renderiza por primera vez, el campo de entrada debe enfocarse. Ese es un caso de uso en el que sería necesario acceder a la API del DOM. En general, se puede utilizar el atributo `ref` tanto en componentes funcionales sin estado como en componentes de clase ES6. En este ejemplo, será necesario un método de ciclo de vida. Así que el procedimiento es propuesto utilizando el atributo `ref` con un componente de clase ES6.

El paso inicial es refactorizar el componente funcional sin estado (functional stateless component) a un componente de clase ES6.

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
class Search extends Component {
  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
# leanpub-end-insert
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
        />
        <button type="submit">
          {children}
        </button>
      </form>
# leanpub-start-insert
    );
  }
}
# leanpub-end-insert
~~~~~~~~

El objeto `this` de un componente de clase ES6 sirve para hacer referencia al nodo DOM con el atributo `ref`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class Search extends Component {
  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
# leanpub-start-insert
          ref={el => this.input = el}
# leanpub-end-insert
        />
        <button type="submit">
          {children}
        </button>
      </form>
    );
  }
}
~~~~~~~~

Ahora puedes enfocar el campo de entrada cuando el componente se monte usando el objeto `this`, el método del ciclo de vida apropiado y la API del DOM.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class Search extends Component {
# leanpub-start-insert
  componentDidMount() {
    if (this.input) {
      this.input.focus();
    }
  }
# leanpub-end-insert

  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
          ref={el => this.input = el}
        />
        <button type="submit">
          {children}
        </button>
      </form>
    );
  }
}
~~~~~~~~

El campo de entrada debe enfocarse al momento en que la aplicación es renderizada. Pero, ¿cómo obtendrías acceso a `ref` en un componente funcional sin estado, sin el objeto `this`? El siguiente componente funcional sin estado lo demuestra.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Search = ({
  value,
  onChange,
  onSubmit,
  children
}) => {
# leanpub-start-insert
  let input;
# leanpub-end-insert
  return (
    <form onSubmit={onSubmit}>
      <input
        type="text"
        value={value}
        onChange={onChange}
# leanpub-start-insert
        ref={el => this.input = el}
# leanpub-end-insert
      />
      <button type="submit">
        {children}
      </button>
    </form>
  );
}
~~~~~~~~

Ahora se puede utilizar el elemento de entrada (`input`) del DOM. En el ejemplo del caso de uso de foco, esto no ayudaría mucho, porque no existe un método de ciclo de vida para desencadenar el enfoque. Así que por ahora no se utilizara la variable de entrada (`input`). Pero en el futuro es posible que encuentres otros casos de uso donde si tiene sentido utilizar un componente funcional sin estado con el atributo `ref`.

### Ejercicios

* lee más sobre [uso general del atributo ref en React](https://facebook.github.io/react/docs/refs-and-the-dom.html)
* lee más sobre [usos del atributo ref en React](https://www.robinwieruch.de/react-ref-attribute-dom-node/)

## Cargando ...

Ahora regresemos a la aplicación, donde posiblemente quieras mostrar un indicador de carga al enviar una solicitud de búsqueda a la API de Hacker News. La solicitud es asincrónica así que es necesario mostrarle al usuario algun indicador de que algo está sucediendo. Definamos un componente de carga reutilizable dentro del archivo *src/App.js*.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Loading = () =>
  <div>Loading ...</div>
~~~~~~~~

Ahora, será necesario agregar una propiedad para almacenar el estado de carga. En función del estado de carga, puede decidir mostrar el componente `Loading` más adelante.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {
  _isMounted = false;

  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
      error: null,
# leanpub-start-insert
      isLoading: false,
# leanpub-end-insert
    };

    ...
  }

  ...

}
~~~~~~~~

El valor inicial de la propiedad `isLoading` es falso. No se carga nada antes de montar el componente de la aplicación.

Una vez la solicitud es realizada, establece un estado de la propiedad cambia a verdadero. Finalmente, la solicitud tendrá éxito y puede establecer el estado de carga en falso.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  setSearchTopStories(result) {
    ...

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
# leanpub-start-insert
      isLoading: false
# leanpub-end-insert
    });
  }

  fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
    this.setState({ isLoading: true });
# leanpub-end-insert

    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(result => this._isMounted && this.setSearchTopStories(result.data))
      .catch(error => this._isMounted && this.setState({ error }));
  }

  ...

}
~~~~~~~~

En el último paso, usarás el componente `Loading`dentro del componente `App`. Una representación condicional basada en el estado de carga decidirá si muestra el componente `Loading` o el componente `Button`. El último es el botón para obtener más datos.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
# leanpub-start-insert
      isLoading
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <div className="interactions">
# leanpub-start-insert
          { isLoading
            ? <Loading />
            : <Button
                onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}
              >
              More
            </Button>
          }
# leanpub-end-insert
        </div>
      </div>
    );
  }
}
~~~~~~~~

Inicialmente, el componente `Loading` aparecerá al iniciar la aplicación, ya que realiza una solicitud en `componentDidMount()`. No hay un componente `Table` porque la lista está vacía. Cuando se obtiene la respuesta de la API Hacker News, se muestra el resultado, el estado de carga se establece en falso y el componente `Loading` desaparece.  Entonces, aparece el botón "More" para buscar más datos. Una vez que se obtengan más datos, el botón desaparecerá. De lo contrario, se mostrará el componente `Loading`.

### Ejercicios:

* usa una librería como [Font Awesome](http://fontawesome.io/) para mostrar un icono de carga en lugar del texto "Loading ..."

## Componentes de orden superior (Higher-Order Components)

Componentes de orden superior (HOC) son un concepto avanzado en React. HOCs son equivalentes a las funciones de orden superior. Toman cualquier entrada, la mayoría de las veces un componente, pero también argumentos opcionales, y devuelven un componente como resultado. El componente devuelto es una versión mejorada del componente de entrada y se puede usar con JSX.

Los HOCs se utilizan en diferentes casos de uso. Pueden preparar propiedades, gestionar el estado o alterar la representación de un componente. Un caso de uso podría ser utilizar un HOC como ayudante para una representación condicional. Imagina que tienes un componente `List` que muestra una lista de elementos o nada, porque la lista está vacía o nula. El HOC podría ocultar que la lista no representaría nada cuando no hay una lista. Por otro lado, el componente `List` simple no necesita preocuparse por una lista inexistente. Solo le importa renderizar la lista.

Hagamos una HOC simple que tome un componente como entrada y devuelva otro componente. Puedes colocarlo en tu archivo *src/App.js*.

{title="src/App.js",lang="javascript"}
~~~~~~~~
function withFoo(Component) {
  return function(props) {
    return <Component { ...props } />;
  }
}
~~~~~~~~

Una útil convención es prefijar el nombre de un HOC con `with` . Dado que está utilizando JavaScript ES6, puedes expresar el HOC de manera más concisa con una función de flecha ES6.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const withEnhancement = (Component) => (props) =>
  <Component { ...props } />
~~~~~~~~

En el ejemplo anterior, el componente de entrada se mantendría igual que el componente de salida. No pasa nada. El componente de salida debería mostrar el componente `Loading` tan pronto el estado cambie a verdadero, de otra manera debería mostrar el componente `Input`

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => (props) =>
  props.isLoading
    ? <Loading />
    : <Component { ...props } />
# leanpub-end-insert
~~~~~~~~

Según la propiedad de carga, puede aplicar una renderización condicional. La función devolverá el componente `Loading` o el componente `Input`.

En general, puede ser bastante eficiente distribuir un objeto, como el objeto `props`, como entrada para un componente. En el siguiente fragmento de código se puede ver la diferencia:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// before you would have to destructure the props before passing them
const { firstname, lastname } = props;
<UserProfile firstname={firstname} lastname={lastname} />

// but you can use the object spread operator to pass all object properties
<UserProfile { ...props } />
~~~~~~~~

Hay una pequeña cosa que debes evitar. Se pasaron todas las `props`, incluyendo la propiedad `isLoading` al pasar el objeto al componente `Input`. Sin embargo, el componente `Input` no no la existencia de la propiedad `isLoading`. Puedes usar la re-desestructuración de ES6 para evitar esto.

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading
    ? <Loading />
    : <Component { ...rest } />
# leanpub-end-insert
~~~~~~~~

Se toma una propiedad del objeto, pero se conserva el resto del objeto. También funciona con múltiples propiedades. Puedes leer mas acerca de esto en el articulo de la página de Mozilla: [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

Ahora puedes usar HOCs dentro de JSX. Un caso de uso en la aplicación podría ser mostrar el botón "More" o el componente `Loading`. El componente `Loading` ya está encapsulado dentro del HOC, pero falta un componente `Input`. Para mostrar un componente `Button` o bin un componente `Loading`, el Button es el componente de entrada del HOC. El componente de salida mejorado es un componente `ButtonWithLoading`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Button = ({
  onClick,
  className = '',
  children,
}) =>
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

const Loading = () =>
  <div>Loading ...</div>

const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading
    ? <Loading />
    : <Component { ...rest } />

# leanpub-start-insert
const ButtonWithLoading = withLoading(Button);
# leanpub-end-insert
~~~~~~~~

Todo se encuentra definido ya. Como último paso, debes usar el componente `ButtonWithLoading`, que recibe el estado de carga como una propiedad adicional. Mientras el HOC consume la propiedad de carga, todas las demás propiedad pasan al componente `Button`.


{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
    ...
    return (
      <div className="page">
        ...
        <div className="interactions">
# leanpub-start-insert
          <ButtonWithLoading
            isLoading={isLoading}
            onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}
          >
            More
          </ButtonWithLoading>
# leanpub-end-insert
        </div>
      </div>
    );
  }
}
~~~~~~~~

Cuando ejecute nuevamente las pruebas, notarás que la prueba para el componente `App` falla. La línea de comando debería mostrar algo como esto:

{title="Command Line",lang="text"}
~~~~~~~~
-    <button
-      className=""
-      onClick={[Function]}
-      type="button"
-    >
-      More
-    </button>
+    <div>
+      Loading ...
+    </div>
~~~~~~~~

Ahora, puedes intentar arreglar el componente al momento en que pienses que algo está mal con el, o puedes aceptar este pequeño error. Y como en este capítulo acabas de introducir al componente `Loading`, puedes aceptar el pequeño error mostrado en la línea de comandos al realizar la prueba interactiva.

Componentes de orden superior son un partón avanzado en React. Tienen multiples propositos: Mejorada reusabilidad de componentes, mayor abstracción, composibilidad de componentes y la manipulación de props, estados y vistas. Puedes leer [gentle introduction to higher-order components](https://www.robinwieruch.de/gentle-introduction-higher-order-components/). Te ofrece otro enfoque para aprender sobre este tema, te ofrece una manera elegante de de usarlos dentro del paradigma de programación funcional y resuelve el problema de renderizado condicional con componentes de orden superior.

### Exercises:

* Lee [a gentle introduction to higher-order components](https://www.robinwieruch.de/gentle-introduction-higher-order-components/)
* Experimenta con el HOC que acabas de crear
* Piensa sobre un caso de uso donde otro HOC tendría sentido
* Implementa el HOC, si hay un caso de uso para ello

## Sorting Avanzado

Ya implementaste un campo de busqueda que interactua con el lado del servidor y el lado de la aplicación. Y como tienes un componente `Table`, tiene sentido intentar mejorarlo con interacciones un poco mas complejas. Acto seguido, agregarás una función para organizar cada columna utilizando el encabezado de la Tabla.

Es posible que escribas tu propia función de clasificación de elementos en la tabla, pero prefiero usar una utilidad incluida en librerias como [Lodash](https://lodash.com/). Hay otras opciones, pero en el libro usaremos Lodash.

{title="Command Line",lang="text"}
~~~~~~~~
npm install lodash
~~~~~~~~

Ahora, es posible importar la función de clasificación incluida en Lodash dentro del archivo *src/App.js*:

{title="src/App.js",lang="javascript"}
~~~~~~~~
import React, { Component } from 'react';
import axios from 'axios';
# leanpub-start-insert
import { sortBy } from 'lodash';
# leanpub-end-insert
import './App.css';
~~~~~~~~

Ya tienes varias columnas en la tabla: title, author, comments y poits. Puedes definir funciones de clasificación (sort functions) donde cada una toma una lista y returna una lista de elementos clasificados de acuerdo a una propiedad específica. Adicionalmente, necesitarás una función de clasificación por defecto que no clasifique, pero que retorne una lista no clasificada. Esta lista será el estado inicial.

{title="src/App.js",lang="javascript"}
~~~~~~~~
...

# leanpub-start-insert
const SORTS = {
  NONE: list => list,
  TITLE: list => sortBy(list, 'title'),
  AUTHOR: list => sortBy(list, 'author'),
  COMMENTS: list => sortBy(list, 'num_comments').reverse(),
  POINTS: list => sortBy(list, 'points').reverse(),
};
# leanpub-end-insert

class App extends Component {
  ...
}
...
~~~~~~~~

Dos de las funciones de clasificación returnan una lista invertida. Es para ver los ítems con la mayor cantidad de números y puntos, en vez de los ítems con el menor conteo al momento en que la lista es clasificada por primera vez.

Los objetos `SORTS` permiten referenciar a cualquier función de clasificación a partir de ahora.

El componente `App` es responsable de almacenar el estado de la clasificación. El estado inicial será la opción de clasificación por defecto, que no clasifica nada en absoluto, solo retorna la lista.

{title="src/App.js",lang="javascript"}
~~~~~~~~
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  error: null,
  isLoading: false,
# leanpub-start-insert
  sortKey: 'NONE',
# leanpub-end-insert
};
~~~~~~~~

Una vez que has una `sortKey` diferente, por ejemplo `AUTHOR`, es posible clasificar la lista con la función de clasificación adecuada.

Ahora, podemos definir un nuevo método dentro del componente `App` que inicializa un `sortKey` para el estado local del componente, la `sortKey` puede ser utilizada para para ayudar a aplicar la función de clasificación correcta a la lista:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {
  _isMounted = false;

  constructor(props) {

    ...

    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-start-insert
    this.onSort = this.onSort.bind(this);
# leanpub-end-insert
  }

  ...

# leanpub-start-insert
  onSort(sortKey) {
    this.setState({ sortKey });
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

El siguiente paso consiste en pasar el método y el `sortKey` al componente `Tabla`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading,
# leanpub-start-insert
      sortKey
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <Table
          list={list}
# leanpub-start-insert
          sortKey={sortKey}
          onSort={this.onSort}
# leanpub-end-insert
          onDismiss={this.onDismiss}
        />
        ...
      </div>
    );
  }
}
~~~~~~~~

El componente `Table` es responsable de clasificar la lista. Este componente toma una de las funciones `SORT` y le pasa la lista como entrada, y luego sigue mapeando la lista previamente clasificada.

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
# leanpub-end-insert
  <div className="table">
# leanpub-start-insert
    {SORTS[sortKey](list).map(item =>
# leanpub-end-insert
      <div key={item.objectID} className="table-row">
        ...
      </div>
    )}
  </div>
~~~~~~~~

En teoría, la lista debería se clasificada por una de las funciones. Sin embargo la clasificación por defecto está configurada con el valor `NONE`, así que nada es clasificado aún, porque nada ejecuta al método `onSort()` para cambiar el valor de `sortKey`. Se extiende la Tabla con una fila de encabezados de columna que utilizan componentes `Sort` dentro de las columnas para realizar la clasificación de cada una de las columnas:

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
  <div className="table">
# leanpub-start-insert
    <div className="table-header">
      <span style={{ width: '40%' }}>
        <Sort
          sortKey={'TITLE'}
          onSort={onSort}
        >
          Title
        </Sort>
      </span>
      <span style={{ width: '30%' }}>
        <Sort
          sortKey={'AUTHOR'}
          onSort={onSort}
        >
          Author
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        <Sort
          sortKey={'COMMENTS'}
          onSort={onSort}
        >
          Comments
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        <Sort
          sortKey={'POINTS'}
          onSort={onSort}
        >
          Points
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        Archive
      </span>
    </div>
# leanpub-end-insert
    {SORTS[sortKey](list).map(item =>
      ...
    )}
  </div>
~~~~~~~~

Cada componente `Sort` recibe una `sortKey` especifica y la función general `onSort()`. Internamente llama al método con la `sortKey` correspondiente, para determinar la `sortKey` específica.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Sort = ({ sortKey, onSort, children }) =>
  <Button onClick={() => onSort(sortKey)}>
    {children}
  </Button>
~~~~~~~~

Como puedes ver, el componente `Sort` reutiliza el componente `Button`. Durante el click del botón, cada `sortKey` será asignada de manera individual por el método `onSort()`. Ahora será posible clasificar la lista cuándo el encabezado de la columna este seleccionado.

Ahora, mejorarás el estilo del botón en el encabezado de cada columna. Dale un `className` adecuado:

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Sort = ({ sortKey, onSort, children }) =>
# leanpub-start-insert
  <Button
    onClick={() => onSort(sortKey)}
    className="button-inline"
  >
# leanpub-end-insert
    {children}
  </Button>
~~~~~~~~

Esto se hace con el fin de mejorar la UI (Interfáz de Usuario por sus siglas en Inglés). El siguiente objetivo es implementar una clasificación invertida (reverse sort). La lista debería realizar una clasificación invertida al momento en que un componente `Sort` es clickeado dos veces. En primer lugar, es necesario que definas el estado reverso con un booleano. La clasificación puede ser invertida o no invertida (reversed ó non-reversed).

{title="src/App.js",lang="javascript"}
~~~~~~~~
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  error: null,
  isLoading: false,
  sortKey: 'NONE',
# leanpub-start-insert
  isSortReverse: false,
# leanpub-end-insert
};
~~~~~~~~

Ahora dentro del método `Sort`, es posible evaluar si la lista está sorteada en reverso. Se determina que está en reverso si el estado es el mismo que el de la `sortKey` nueva y el estado aún no posee un valor igual a verdadero (true).

{title="src/App.js",lang="javascript"}
~~~~~~~~
onSort(sortKey) {
# leanpub-start-insert
  const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
  this.setState({ sortKey, isSortReverse });
# leanpub-end-insert
}
~~~~~~~~

De nuevo, es posible pasar la propiedad revertida al componente `Tabla`:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading,
      sortKey,
# leanpub-start-insert
      isSortReverse
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <Table
          list={list}
          sortKey={sortKey}
# leanpub-start-insert
          isSortReverse={isSortReverse}
# leanpub-end-insert
          onSort={this.onSort}
          onDismiss={this.onDismiss}
        />
        ...
      </div>
    );
  }
}
~~~~~~~~

La Tabla debe tener una bloque correspondiente a la función flecha con la capacidad de computar datos:

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const Table = ({
  list,
  sortKey,
  isSortReverse,
  onSort,
  onDismiss
}) => {
  const sortedList = SORTS[sortKey](list);
  const reverseSortedList = isSortReverse
    ? sortedList.reverse()
    : sortedList;

  return(
# leanpub-end-insert
    <div className="table">
      <div className="table-header">
        ...
      </div>
# leanpub-start-insert
      {reverseSortedList.map(item =>
# leanpub-end-insert
        ...
      )}
    </div>
# leanpub-start-insert
  );
}
# leanpub-end-insert
~~~~~~~~

Finalmente, queremos brindar información visual al usuario para que pueda distinguir cuáles columnas están clasificadas de manera activa. Cada componente `Sort` ya tiene su `sortKey` específico, el cuál puede ser utilizado para identificar cuando la clasificación esta activa o no. Se pasa el `sortKey` desde el estado del componente interno como una clasificación activa al componente `Sort`:

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Table = ({
  list,
  sortKey,
  isSortReverse,
  onSort,
  onDismiss
}) => {
  const sortedList = SORTS[sortKey](list);
  const reverseSortedList = isSortReverse
    ? sortedList.reverse()
    : sortedList;

  return(
    <div className="table">
      <div className="table-header">
        <span style={{ width: '40%' }}>
          <Sort
            sortKey={'TITLE'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Title
          </Sort>
        </span>
        <span style={{ width: '30%' }}>
          <Sort
            sortKey={'AUTHOR'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Author
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'COMMENTS'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Comments
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'POINTS'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Points
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          Archive
        </span>
      </div>
      {reverseSortedList.map(item =>
          ...
      )}
    </div>
  );
}
~~~~~~~~

Dentro del componente `Sort`, se puede saber si la función de clasificación esta activa en base a `sortKey` y `activeSortKey`. Al componente `Sort` dale un atributo extra llamado `className`, para brindarle información acerca de su estado al usuario:

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const Sort = ({
  sortKey,
  activeSortKey,
  onSort,
  children
}) => {
  const sortClass = ['button-inline'];

  if (sortKey === activeSortKey) {
    sortClass.push('button-active');
  }

  return (
    <Button
      onClick={() => onSort(sortKey)}
      className={sortClass.join(' ')}
    >
      {children}
    </Button>
  );
}
# leanpub-end-insert
~~~~~~~~

Es posible definir `sortClass` de manera más eficiente utilizando la librería llamada `classnames`, que se intala utilizando npm:

{title="Command Line",lang="text"}
~~~~~~~~
npm install classnames
~~~~~~~~

Después de la instalación, lo importamos en el tope del archivo *src/App.js*.

{title="src/App.js",lang="javascript"}
~~~~~~~~
import React, { Component } from 'react';
import axios from 'axios';
import { sortBy } from 'lodash';
# leanpub-start-insert
import classNames from 'classnames';
# leanpub-end-insert
import './App.css';
~~~~~~~~

Ahora, ya puedes usarlo para definir el componente `className` con clases condicionales.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Sort = ({
  sortKey,
  activeSortKey,
  onSort,
  children
}) => {
# leanpub-start-insert
  const sortClass = classNames(
    'button-inline',
    { 'button-active': sortKey === activeSortKey }
  );
# leanpub-end-insert

  return (
# leanpub-start-insert
    <Button
      onClick={() => onSort(sortKey)}
      className={sortClass}
    >
# leanpub-end-insert
      {children}
    </Button>
  );
}
~~~~~~~~

Habrán unidades de prueba fallidas en el componente `Table`. Dado que cambiamos intencionalmente la representación de nuestro componente, aceptamos los resultados de las pruebas, pero todavía es necesario repara la unidad de prueba. En *src/app.test.js*, ingresa una función `sortKey` y el booleano `isSortReverse` para el componente `Tabla`.

{title="src/App.test.js",lang="javascript"}
~~~~~~~~
...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
# leanpub-start-insert
    sortKey: 'TITLE',
    isSortReverse: false,
# leanpub-end-insert
  };

  ...

});
~~~~~~~~

De nuevo, es necesario aceptar las fallas en las pruebas para el componente `Table`, porque utilizamos props extendidas en el. La interacción de clasificación avanzada finalmente ha sido completada.

### Ejercicios:

* Usa una librería como [Font Awesome](http://fontawesome.com/) para indicar la clasificación (reverse). Podrías utilizar una flecha haia arriba o una flehca hacia abajo al lado de cada Encabezado
* Lee más acerca de [classnames library](https://github.com/JedWatson/classnames)

{pagebreak}

Aprendiste sobre técnicas de componentes avanzados en React. Recapitulemos lo visto en el capítulo:

* **React**
  * El atributo `ref` hace referencia a elementos que forman parte del DOM
  * Componentes de orden superior son comúnmente utilizados para construir componentes avanzados
  * Implementación de interacciones avanzadas en React
  * `classNames` condicionales utilizando una librería
* **ES6**
  * Desestructuración para dividir objetos y arrays

Puedes encontrar el código fuente en el [repositorio oficial](https://github.com/rwieruch/hackernews-client/tree/9456117fb67bbe98d7e3f41bbc85b4a035020e7e).
