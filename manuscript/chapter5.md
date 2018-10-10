# Componentes React Avanzados

Este capítulo se enfoca en la implementación de componentes React avanzados. Aprenderás sobre componentes de orden superior y cómo implementarlos, navegando aún más profundo en el ecosistema React.

## Ref a DOM Element

A veces necesitas interactuar con tus nodos DOM en React. El atributo `ref` te da acceso a un nodo dentro de tus elementos. Por lo general, ese se conoce como un antipatrón Reat, porque debes usar su forma declarativa de hacer las cosas y su flujo de datos unidireccional. Aprendiste sobre esto cuando se introdujo el primer campo de busqueda, pero hay ciertos casos donde necesitas acceso al nodo DOM. La documentación oficial menciona tres de estos casos de uso:

* usar la API DOM (focus, media playback etc.)
* invocar animaciones de nodo DOM imperativas
* para integrar con una biblioteca de terceros que necesita el nodo DOM (e.g. [D3.js](https://d3js.org/))

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

El campo de entrada debe enfocarse al momento en que la aplicación es renderizada. Pero, ¿cómo obtendría acceso a `ref` en un componente funcional sin estado, sin el objeto `this`? El siguiente componente funcional sin estado lo demuestra.

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

Ahora regresemos a la aplicación. Es posible que desee mostrar un indicador de carga cuando envíe una solicitud de búsqueda a Hacker News API. La solicitud es asincrónica y debe mostrarle a su usuario algun indicador de que algo va a suceder. Definamos un componente de carga reutilizable en su archivo *src/App.js*.

```js
const Loading = () =>
  <div>Loading ...</div>
```

Ahora necesitará una propiedad para almacenar el estado de carga. En función del estado de carga, puede decidir mostrar el componente Cargando más adelante.

```js
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
      isLoading: false,
    };

    ...
  }

  ...

}
```

El valor inicial de esa propiedad es falso. No carga nada antes de montar el componente de la aplicación.

Cuando realiza la solicitud, establece un estado de carga en verdadero. Finalmente, la solicitud tendrá éxito y puede establecer el estado de carga en falso.

```js
class App extends Component {

  ...

  setSearchTopstories(result) {
    const { hits, page } = result;
    const { searchKey, results } = this.state;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
      isLoading: false
    });
  }

  fetchSearchTopstories(searchTerm, page) {
    this.setState({ isLoading: true });

    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(response => response.json())
      .then(result => this.setSearchTopstories(result))
      .catch(e => e);
  }

  ...

}
```

En el último paso, usarás el componente de carga en tu aplicación. Una representación condicional basada en el estado de carga decidirá si muestra un componente de carga o un componente de botón. El último es su botón para obtener más datos.

```js
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      isLoading
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <div className="interactions">
          { isLoading
            ? <Loading />
            : <Button
              onClick={() => this.fetchSearchTopstories(searchKey, page + 1)}>
              More
            </Button>
          }
        </div>
      </div>
    );
  }
}
```

Inicialmente, el componente de carga aparecerá cuando inicie su aplicación, ya que realiza una solicitud en `componentDidMount()`. No hay un componente Table porque la lista está vacía. Cuando btiene la respuesta de la API Hacker News, se muestra el resultado, el estado de carga se establece en falso y el componente Loading desaparece.  Aparece el botón "More" para buscar más datos. Una vez que obtenga más datos, el botón desaparecerá. De lo contrario, se mostrará el componente Loading.

### Ejercicios:

* usar una librería como [Font Awesome](http://fontawesome.io/) para mostrar un icono de carga en lugar del texto "Loading ..."

## Componentes de orden superior

Componentes de orden superior (HOC) son un concepto avanzado en React. HOCs son equivalentes a las funciones de orden superior. Toman cualquier entrada, la mayoría de las veces un componente, pero también argumentos opcionales, y devuelven un componente como resultado. El componente devuelto es una versión mejorada del componente de entrada y se puede usar en su JSX.

Los HOC se usan para diferentes casos de uso. Pueden preparar propiedades, gestionar el estado o alterar la representación de un componente. Un caso de uso podría ser utilizar un HOC como ayudante para una representación condicional. Imagine que tiene un componente List que muestra una lista de elementos o nada, porque la lista está vacía o nula. El HOC podría ocultar que la lista no representaría nada cuando no hay una lista. Por otro lado, el componente Lista simple no necesita preocuparse por una lista inexistente.Solo le importa hacer la lista.

Hagamos un HOC simple que tome un componente como entrada y devuelva un componente. Puedes colocarlo en tu archivo *src/App.js*.

```js
function withFoo(Component) {
  return function(props) {
    return <Component { ...props } />;
  }
}
```

Una bonita convención es prefijar el nombre de un HOC con `with` . Dado que está utilizando JavaScript ES6, puede expresar el HOC de manera más concisa con una función de flecha ES6.

```js
const withFoo = (Component) => (props) =>
  <Component { ...props } />
```

En el ejemplo, el componente de entrada se mantendría igual que el componente de salida. No pasa nada. Realiza la misma instancia de componente y pasa todas las propiedades al componente de salida. Pero eso es inútil. Vamos a mejorar el componente de salida. El componente de salida debe mostrar el componente de carga, cuando el estado de carga es verdadero, de lo contrario, debería mostrar el componente de entrada. Una representación condicional es un gran caso de uso para un HOC.

```js
const withLoading = (Component) => (props) =>
  props.isLoading ? <Loading /> : <Component { ...props } />
```

Según la propiedad de carga, puede aplicar una representación condicional. La función devolverá el componente Loading o el componente Input.

En general, puede ser muy eficaz distribuir un objeto, como el objeto props, como entrada para un componente. Vea la diferencia en el siguiente fragmento de código.

```js
// before you would have to destructure the props before passing them
const { foo, bar } = props;
<SomeComponent foo={foo} bar={bar} />

// but you can use the object spread operator to pass all object properties
<SomeComponent { ...props } />
```

Hay una pequeña cosa que debes evitar. Usted pasa todas las props incluyendo la propiedad `isLoading`, al pasar el objeto, al componente Input. Sin embargo, el componente Input no se preocupa por la propiedad `isLoading`. Puede usar la re-desestructuración de ES6 para evitarlo.

```js
const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading ? <Loading /> : <Component { ...rest } />
```

Toma una propiedad del objeto, pero conserva el objeto restante. También funciona con múltiples propiedades. Es posible que ya lo hayas leído en el [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

Ahora puedes usar el HOC en tu JSX. Un caso de uso en la aplicación podría ser mostrar el botón "More" o el componente Loading. El componente de carga ya está encapsulado en el HOC, pero falta un componente Input. En el caso de uso de mostrar un componente Button o un componente Loading, el Button es el componente de entrada del HOC. El componente de salida mejorado es un componente ButtonWithLoading.

```js
const Button = ({ onClick, className = '', children }) =>
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
  isLoading ? <Loading /> : <Component { ...rest } />

const ButtonWithLoading = withLoading(Button);
```

Todo se define ahora. Como último paso, debe usar el componente ButtonWithLoading, que recibe el estado de carga como una propiedad adicional. Mientras el HOC consume la propiedad de carga, todas las demás propiedad pasan al componente Button.

```js
class App extends Component {

  ...

  render() {
    ...
    return (
      <div className="page">
        ...
        <div className="interactions">
          <ButtonWithLoading
            isLoading={isLoading}
            onClick={() => this.fetchSearchTopstories(searchKey, page + 1)}>
            More
          </ButtonWithLoading>
        </div>
      </div>
    );
  }
}
```

Cuando ejecute nuevamente las pruebas, observará que su prueba para el componente App falla. Debería mostrar la siguiente diferencia en la línea de comando.

```js
-     <button
-       className=""
-       onClick={[Function]}
-       type="button">
-       More
-     </button>
+     <div>
+       Loading ...
+     </div>
```

You can either fix the component now, when you think there is something wrong about it, or can accept the new snapshot. Because you introduced the Loading component in this chapter, you can accept the failing snapshot test with `u` on the command line in the interactive test.

Higher order components are an advanced technique in React. They have multiple purposes like improved reusability of components, greater abstraction, composability of components and manipulations of props, state and view. Don't worry if you don't understand them immediately. It takes time to get used to them.

I encourage you to read the [gentle introduction to higher order components](https://www.robinwieruch.de/gentle-introduction-higher-order-components/). It gives you another approach to learn them, shows you an elegant way to use them the functional programming way and solves specifically the problem of conditional rendering with higher order components.

### Ejercicios:

* experiment with the HOC you have created
* think about a use case where another HOC would make sense
  * implement the HOC, if there is a use case

## Advanced Sorting

You have already implemented a client- and server-side search interaction. Since you have a Table component, it would make sense to enhance the Table with advanced interactions. What about enabling sorting by the Table columns?

It would be possible to write your own sort function, but personally I prefer to use a utility library for such cases. [Lodash](https://lodash.com/) is one of these utility libraries. Let's install and use it for the sort functionality.

```
npm install --save lodash
```

Now you can import the sort functionality of lodash in your *src/App.js* file.

```js
import React, { Component } from 'react';
import { sortBy } from 'lodash';
import './App.css';
```

You have several columns in your Table. There are title, author, comments and points columns. You can define sort functions where each function takes a list and returns a list of items sorted by property. Additionally you will need one default sort function which doesn't sort but only returns the unsorted list.

```js
...

const SORTS = {
  NONE: list => list,
  TITLE: list => sortBy(list, 'title'),
  AUTHOR: list => sortBy(list, 'author'),
  COMMENTS: list => sortBy(list, 'num_comments').reverse(),
  POINTS: list => sortBy(list, 'points').reverse(),
};

class App extends Component {
  ...
}
...
```

You can see that two of the sort functions return a reversed list. That's because you want to see the items with the highest comments and points rather than to see the items with the lowest.

The `SORTS` object allows you to reference any sort function now.

Again your App component is responsible for storing the state of the sort. The initial state will be the initial default sort function, which doesn't sort at all and returns the input list as output.

```js
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  isLoading: false,
  sortKey: 'NONE',
};
```

Once you choose a different `sortKey`, let's say the `AUTHOR` key, you will sort the list with the appropriate sort function.

Now you can define a new sort method in your App component that simply sets a `sortKey` to your internal component state.

```js
class App extends Component {

  constructor(props) {

    ...

    this.needsToSearchTopstories = this.needsToSearchTopstories.bind(this);
    this.setSearchTopstories = this.setSearchTopstories.bind(this);
    this.fetchSearchTopstories = this.fetchSearchTopstories.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
    this.onSort = this.onSort.bind(this);
  }

  onSort(sortKey) {
    this.setState({ sortKey });
  }
  ...

}
```

The next step is to pass the method and `sortKey` to your Table component.

```js
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      isLoading,
      sortKey
    } = this.state;

    ...

    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
        <Table
          list={list}
          sortKey={sortKey}
          onSort={this.onSort}
          onDismiss={this.onDismiss}
        />
        <div className="interactions">
          ...
        </div>
      </div>
    );
  }
}
```

The Table component is responsible for sorting the list. It takes one of the `SORT` functions by `sortKey` and passes the list as input. Afterward it keeps mapping over the sorted list.

```js
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
  <div className="table">
    { SORTS[sortKey](list).map(item =>
      <div key={item.objectID} className="table-row">
        ...
      </div>
    )}
  </div>
```

In theory the list would get sorted by one of the functions. But the default sort is set to `NONE`. So far no one executes the `onSort()` method to change the `sortKey`. Let's extend the Table with a row of headers that use Sort components in columns to sort each column.

```js
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
  <div className="table">
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
    { SORTS[sortKey](list).map(item =>
      ...
    )}
  </div>
```

Each Sort component gets a specific `sortKey` and the general `onSort()` function. Internally it calls the method with the `sortKey` to set the specific key.

```js
const Sort = ({ sortKey, onSort, children }) =>
  <Button onClick={() => onSort(sortKey)}>
    {children}
  </Button>
```

As you can see, the Sort component reuses your common Button component. On a button click each individual passed `sortKey` will get set by the `onSort()` method. Now you should be able to sort the list when you click on the column headers.

But a button in a column header looks a bit silly. Let's give the Sort a proper `className`.

```js
const Sort = ({ sortKey, onSort, children }) =>
  <Button
    onClick={() => onSort(sortKey)}
    className="button-inline"
  >
    {children}
  </Button>
```

It should look nice now. The next goal would be to implement reverse sort as well. The list should get reverse sorted once you click a Sort component twice. First you need to define the reverse state.

```js
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  isLoading: false,
  sortKey: 'NONE',
  isSortReverse: false,
};
```

Now in your sort method you can evaluate if the list is reverse sorted. It is when `sortKey` in the state is the same as the incoming `sortKey` and the reverse state is not already set to true.

```js
onSort(sortKey) {
  const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
  this.setState({ sortKey, isSortReverse });
}
```

Again you can pass the reverse prop to your Table component.

```js
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      isLoading,
      sortKey,
      isSortReverse
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <Table
          list={list}
          sortKey={sortKey}
          isSortReverse={isSortReverse}
          onSort={this.onSort}
          onDismiss={this.onDismiss}
        />
        ...
      </div>
    );
  }
}
```

The Table has to have an arrow function block body to compute the data now.

```js
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
        ...
      </div>
      { reverseSortedList.map(item =>
        ...
      )}
    </div>
  );
}
```

The reverse sort should work now.

Last but not least you have to deal with one open question for the sake of an improved user experience. Can a user distinguish which column is actively sorted? So far, it is not possible. Let's give the user a visual feedback.

Each Sort component gets its specific `sortKey` already. It could be used to identify the activated sort. You can pass the `sortKey` from the internal component state as active sort key to your Sort component.

```js
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
            activeSortKey={sortKey}
          >
            Title
          </Sort>
        </span>
        <span style={{ width: '30%' }}>
          <Sort
            sortKey={'AUTHOR'}
            onSort={onSort}
            activeSortKey={sortKey}
          >
            Author
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'COMMENTS'}
            onSort={onSort}
            activeSortKey={sortKey}
          >
            Comments
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'POINTS'}
            onSort={onSort}
            activeSortKey={sortKey}
          >
            Points
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          Archive
        </span>
      </div>
      { reverseSortedList.map(item =>
          ...
      )}
    </div>
  );
}
```

Now in your Sort component you know based on the `sortKey` and `activeSortKey` if the sort is active. Give your Sort component an extra `class` attribute, when it is sorted, to give the user a visual feedback.

```js
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
```

The way to define the `class` is a bit clumsy, isn't it? There is a neat little library to get rid of this. First you have to install it.

```
npm install --save classnames
```

And second you have to import it on top of your *src/App.js* file.

```js
import React, { Component } from 'react';
import { sortBy } from 'lodash';
# leanpub-start-insert
import classNames from 'classnames';
# leanpub-end-insert
import './App.css';
```

Now you can use it to define your component `className` with conditional classes.

```js
const Sort = ({
  sortKey,
  activeSortKey,
  onSort,
  children
}) => {
  const sortClass = classNames(
    'button-inline',
    { 'button-active': sortKey === activeSortKey }
  );

  return (
    <Button
      onClick={() => onSort(sortKey)}
      className={sortClass}
    >
      {children}
    </Button>
  );
}
```

Again, when you run your tests, you should see failing snapshot tests but also failing unit tests for the Table component. Since you changed again your component representations, you can accept the snapshot tests. But you have to fix the unit test. In your *src/App.test.js* file you need to provide a `sortKey` and the `isSortReverse` boolean for the Table component.

```js
...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
    sortKey: 'TITLE',
    isSortReverse: false,
  };

  ...

});
```

Once again you might need to accept the failing snapshot tests for your Table component, because you provided props for the Table and the full component renders now.

Your advanced sort interaction is complete now.

### Exercises:

* use a library like [Font Awesome](http://fontawesome.io/) to indicate the (reverse) sort
  * it could be an arrow up or down icon next to each Sort header
* read more about the [classnames library](https://github.com/JedWatson/classnames)

{pagebreak}

You have learned advanced component techniques in React! Let's recap the last chapters:

* React
  * the ref attribute to reference DOM nodes
  * higher order components are a common way to build advanced components
  * implementation of advanced interactions in React
  * conditional classNames with a neat helper library
* ES6
  * rest destructuring to split up objects and arrays

You can find the source code in the [official repository](https://github.com/rwieruch/hackernews-client/tree/9456117fb67bbe98d7e3f41bbc85b4a035020e7e).