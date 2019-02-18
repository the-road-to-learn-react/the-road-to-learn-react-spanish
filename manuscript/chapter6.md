# Manejo del Estado en React y mucho más

Conociste en capítulos previos los conceptos básicos del manejo del estado en React. Este capítulo profundiza un poco más en el tema. Aprenderás buenas prácticas, como aplicarlas y porque podrías considerar usar una librería especialmente diseñada para la gestión de estado.

## Traspaso del Estado

El único componente con estado en tu aplicación, usando ES6, es el componente `App`. Se encarga de gran parte del estado y lógica (métodos) de la aplicación. Quizás hayas notado que pasaste muchas propiedades a tu componente `Table`. De las cuáles, la mayoría de sólo se usan dentro de dicho componente. No tiene sentido que el componente `App` reconozca estas propiedades.

Por ejemplo, la función de clasificación solamente se utiliza en el componente `Table`. Podrías mover esta funcionalidad adentro del componente. Ya que el componente `App` no necesita saber nada de ella.

El proceso de refactorización de un subestado desde un componente a otro es conocido como *traspaso del estado* (lifting state). En tu caso, quieres mover el estado que no es usado en el componente `App`, más cerca del componente `Table`. El estado pasa del componente padre al componente hijo.

Para tratar con los estados y métodos en el componente `Table`, éste tiene que convertirse a un componente de clase usando ES6. La refactorización de componente funcional sin estado a un componente de clase usando ES6 puede lograrse de manera sencilla.

El componente `Table` como un componente funcional sin estado:

Your Table component as a functional stateless component:

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
    ...
  );
}
~~~~~~~~

Puede refactorizarse a un componente de clase usando ES6:

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
class Table extends Component {
  render() {
    const {
      list,
      sortKey,
      isSortReverse,
      onSort,
      onDismiss
    } = this.props;

    const sortedList = SORTS[sortKey](list);
    const reverseSortedList = isSortReverse
      ? sortedList.reverse()
      : sortedList;

    return (
      ...
    );
  }
}
# leanpub-end-insert
~~~~~~~~

Ya que quieres trabajar con estados y métodos en tu componente, tienes que añadir un constructor y un estado inicial:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class Table extends Component {
# leanpub-start-insert
  constructor(props) {
    super(props);

    this.state = {};
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Ahora, puedes mover estados y métodos de clase con la funcionalidad de clasificación desde el componente `App` hasta el componente `Table`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class Table extends Component {
  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      sortKey: 'NONE',
      isSortReverse: false,
    };

    this.onSort = this.onSort.bind(this);
# leanpub-end-insert
  }

# leanpub-start-insert
  onSort(sortKey) {
    const isSortReverse = this.state.sortKey === sortKey &&
      !this.state.isSortReverse;

    this.setState({ sortKey, isSortReverse });
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Recuerda que es necesario borrar el estado y el método de clase que moviste desde tu componente `App`

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
      isLoading: false,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
  }

  ...

}
~~~~~~~~

También puedes hacer más ligero al componente `Table`. Para ello, debes mover las propiedades que le son asignadas a éste y que se encuentran dentro del componente `app`, porque son utilizadas de manera interna por el componente `Table`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading
    } = this.state;
# leanpub-end-insert

    ...

    return (
      <div className="page">
        ...
        { error
          ? <div className="interactions">
            <p>Something went wrong.</p>
          </div>
          : <Table
# leanpub-start-insert
            list={list}
            onDismiss={this.onDismiss}
# leanpub-end-insert
          />
        }
        ...
      </div>
    );
  }
}
~~~~~~~~

Dentro del componente `Table`, usa el método interno `onSort()` y el estado interno del componente `Table`:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class Table extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      list,
      onDismiss
    } = this.props;

    const {
      sortKey,
      isSortReverse,
    } = this.state;
# leanpub-end-insert

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
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Title
            </Sort>
          </span>
          <span style={{ width: '30%' }}>
            <Sort
              sortKey={'AUTHOR'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Author
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'COMMENTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Comments
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'POINTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Points
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            Archive
          </span>
        </div>
        { reverseSortedList.map((item) =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Acabas de realizar una refactorización crucial al mover la funcionalidad y el estado de un componente a otro componente para aligerar al componente del que se extraen los métodos y estado. De nuevo, el componente `API` perteneciente a `Table` se volvió más ligero, porque internamente, este interactúa con la funcionalidad de clasificación.

El traspaso de estado puede realizarse de otra manera también: De componente hijo a componente padre. Esto se conoce como traspaso inverso de estado (`lifting state up`). Imagina que estás trabajando con el estado local dentro de un componente hijo y quieres que el estado sea visible dentro del componente padre también. Sería necesario que traspases el estado al componente padre. Es decir, realizar un traspaso de estado invertido de estado. En este caso, el componente padre lidia con el estado local y permite que éste sea accesible para todos sus componentes hijo.

### Ejercicios:

* Lee acerca de [traspaso de estado en React](https://reactjs.org/docs/lifting-state-up.html)
* Lee acerca del traspaso de estado en el artículo [learn React before using Redux](https://www.robinwieruch.de/learn-react-before-using-redux/)

## Revisión: setState()

Hasta ahora, has utilizado la función `setState()` de React para gestionar el estado interno de los componentes. Es posible pasar un objeto a la función donde se actualice parte del estado local.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState({ value: 'hello'});
~~~~~~~~

Pero, `setState()` no toma solamente un objeto. En su segunda versión, puedes pasar una función para actualizar el estado.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  ...
});
~~~~~~~~

Hay un caso crucial donde tiene sentido utilizar una función en vez de un objeto: Cuando actualizas el estado dependiendo del estado previo o de las propiedades. Si no utilizas una función, la gestión local de estado puede generar fallas o bugs. El método `setState()` de React es asíncrono. React recopila todas las llamadas `setState()` y las ejecuta eventualmente. Algunas veces, el estado anterior cambia antes de la llamada al método `setState()`.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const { oneCount } = this.state;
const { anotherCount } = this.props;
this.setState({ count: oneCount + anotherCount });
~~~~~~~~

Imagina que `oneCount` y `anotherCount` cambian de manera asíncrona en alguna otra parte del código al momento en que llamas a `setState()`. En una aplicación en constante crecimiento, habrán varias llamadas a `setState()` a lo largo de dicha aplicación. Gracias a que `setState()` se ejecuta de forma asíncrona, para este caso podrías utilizar valores antiguos.

Con el enfoque en funciones, la función en `setState()` es un callback que opera en el estado y propiedades al tiempo en que se ejecuta la función callback. Incluso cuándo `setState()` es asíncrono, con una función se toman el estado y las propiedades al mismo tiempo en que `setState()` se ejecuta.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  const { oneCount } = prevState;
  const { anotherCount } = props;
  return { count: oneCount + anotherCount };
});
~~~~~~~~

En nuestro código, el método `setSearchTopStories()` utiliza el estado anterior y este es un buen ejemplo de cuándo utilizar una función en vez de un objeto dentro de `setState()`. Justo ahora, el código luce así:

{title="src/App.js",lang="javascript"}
~~~~~~~~
setSearchTopStories(result) {
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
~~~~~~~~

Aquí se extraen los valores del estado, pero el estado se actualizó de forma asíncrona en base al estado anterior. Ahora, usaremos el enfoque funcional para prevenir bugs originados por estados antiguos:

{title="src/App.js",lang="javascript"}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;

# leanpub-start-insert
  this.setState(prevState => {
    ...
  });
# leanpub-end-insert
}
~~~~~~~~

Hace un momento implementamos un bloque que ahora puedes mover dentro de la función redireccionándolo para que opere en `prevState` en vez de `this.state`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;

  this.setState(prevState => {
# leanpub-start-insert
    const { searchKey, results } = prevState;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    return {
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
      isLoading: false
    };
# leanpub-end-insert
  });
}
~~~~~~~~

Esto resolverá la situación con un estado obsoleto, pero todavía hay una mejoría que puede implementarse. Debido a que es una función, puede extraerla y así mejorar la legibilidad del código. Una ventaja extra de utilizar una función en vez de un objeto es que la función puede estar ubicada fuera del componente. Aún sería necesario utilizar una función de alto nivel para  pasarle el resultado porque queremos actualizar el estado en base al resultado obtenido de la API.

{title="src/App.js",lang="javascript"}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  this.setState(updateSearchTopStoriesState(hits, page));
}
~~~~~~~~

La función `updateSearchTopStoriesState()` tiene que retornar una función. Es una función de alto nivel (`higher-order function`) que puede ser definida fuera del componente `App`. Nota como la función personalizada cambia ligeramente ahora.

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const updateSearchTopStoriesState = (hits, page) => (prevState) => {
  const { searchKey, results } = prevState;

  const oldHits = results && results[searchKey]
    ? results[searchKey].hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  return {
    results: {
      ...results,
      [searchKey]: { hits: updatedHits, page }
    },
    isLoading: false
  };
};
# leanpub-end-insert

class App extends Component {
  ...
}
~~~~~~~~

El enfoque en funciones y no en objetos previene posibles fallas o bugs dentro de `setState()`, a la vez que la legibilidad y la mantenibilidad del código también se ven mejoradas. Además, se pueden realizar pruebas fuera del componente `App`. Recomiendo el exportar y realizar pruebas como una buena practica.

### Ejercicios:

* Lee sobre [como utilizar de manera correcta el estado en React](https://reactjs.org/docs/state-and-lifecycle.html#using-state-correctly)
* Exporta `updateSearchTopStoriesState` desde el archivo
  * Escribe una prueba que pase `hits`, `page` y un estado previo para que finalmente se espere un estado nuevo. 
* Refactoriza tus métodos `setState()` para que utilicen una función, si consideras que es necesario porque utiliza propiedades o estados
* Ejecuta de nuevo tus pruebas y verifica que todo esté actualizado

## Domando el Estado

Capítulos anteriores te enseñaron que la gestión de estado puede llegar a ser un tema crucial en aplicaciones grandes, así como React y otros frameworks SPA tienen problemas con esto. A medida que las aplicaciones se hacen más y más complejas, el gran reto en las aplicaciones web es el de domar y controlar el estado.

En comparación con otras soluciones, React ya ha tomado un paso a la delantera. Un flujo de datos unidireccional y una API simple que sirvan para gestionar estados en los componentes es indispensable. Estos conceptos hacen más sencillo el lidiar con los estados y los cambios de estado. También facilita lidiar con ellos a nivel de componente y a nivel de aplicación hasta cierto grado.

Es posible que utilizar estados desactualizados pueda generar bugs al momento de utilizar objetos en vez de funciones en `setState()`. Realizamos traspasos de estado para compartirlos u ocultarlos de acuerdo a lo que sea necesario en distintos componentes. Algunas veces, un componente necesita realizar un traspaso de estado porque sus componentes hijos dependen de dicho estado. Quizá el componente está muy alejado del árbol de componentes, así que el estado debe ser accesible a lo largo de todo el árbol de componentes. Componentes están altamente involucrados en la gestión de estado, debido a que la principal responsabilidad de estos consiste en representar a la interfaz de usuario (UI).

Es por esto que hay soluciones que se encargan de la gestión de estado. Librerías como [Redux](https://redux.js.org/introduction) ó [MobX](https://mobx.js.org/) son ambas soluciones efectivas para implementar en aplicaciones React. Incluyen extensiones, [react-redux](https://github.com/reactjs/react-redux) y [mobx-react](https://github.com/mobxjs/mobx-react), para integrarlas en la capa de vistas de React. Redux y MobX están fuera del alcance de este libro, pero te animo a que estudies las diferentes maneras que existe para lidiar con la creciente necesidad por una óptima gestión de estado a medida que tus aplicaciones React se vuelven más y más complejas.

### Ejercicios:

* Lee acerca de [gestión externa de estado y cómo aprenderla](https://www.robinwieruch.de/redux-mobx-confusion/)
* Revisa mi segundo libro acerca de gestión de estado en React llamado [Taming the State in React (Domando el Estado en React)](https://roadtoreact.com/)

{pagebreak}

¡Aprendiste sobre gestión de estado avanzada en React! Vamos a recapitular un poco:

* **React**
  * El traspaso de estado en React puede realizarse de dos maneras en componentes que así lo permitan
  * `setState()` puede utilizar una función para prevenir fallas o bugs generados por estados desactualizados
  * Existen distintas soluciones externas que pueden ayudarte a domar el estado en React

Puedes encontrar el código fuente en el [repositorio oficial](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.6).
