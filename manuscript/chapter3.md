# Trabajar con una API real

Ahora es el momento de trabajar en serio con una API. Puede llegar a ser muy aburrido trabajar solamente con datos estaticos.

Si no estas familiarizado con el concepto de API, te recomiendo leer [ mi viaje donde llegué a conocer el mundo de las APIs](https://www.robinwieruch.de/what-is-an-api-javascript/).


¿Conoces la plataforma [Hacker News](https://news.ycombinator.com/)? Es un agregador de noticias sobre tecnologia. En este libro utilizaremos la API de Hacker News para obtener las ultimas noticias. Es una API [basica](https://github.com/HackerNews/API) de [busqueda](https://hn.algolia.com/api) donde podremos conseguir noticias, esto ultimo nos concierne para alimentar nuestra aplicacion. 
Puedes visitar la informacion de la API en cualquier momento y de esa forma, obtener un mejor conocimiento de la estructura de los datos.


## Métodos del ciclo de vida
Vas a necesitar el conocimiento de los metodos del ciclo de vida de una aplicacion React antes de poder empezar a buscar datos en una API. Estos metodos son realmente importantes. 
Pueden utilizarse en componentes de clase ES6, pero no en componentes funcionales sin estado (Stateless Functional Components)

¿Recuerdas cuando un capítulo anterior te enseñe sobre las clases de JavaScript ES6 y cómo se utilizan en React? Aparte del método `render()`, mencioné varios métodos que se pueden sobrescribir en un componente de clase React ES6. Todos estos son los métodos del ciclo de vida. Vamos a sumergirnos en ellos:

Ya conoces dos métodos de ciclo de vida en un componente de clase ES6: `constructor()` y `render()`.

El constructor sólo se llama cuando se crea una instancia del componente y se inserta en el DOM. El componente se instancia. Ese proceso se llama montaje del componente.

El método `render()` también se llama durante el proceso de montaje, pero también cuando el componente actualiza. Cada vez que el estado o las props de un componente cambian, se llama al método `render()`.

Ahora ya sabes más acerca de estos dos métodos del ciclo de vida y cuándo se llaman. Ya los has utilizado. Pero hay más de ellos.

El montaje de un componente tiene dos métodos de ciclo de vida más: `componentWillMount()` y `componentDidMount()`. El constructor se llama primero, `componentWillMount()` se llama antes del método `render()` y `componentDidMount()` se llama después del método `render()`.

En general, el proceso de montaje tiene 4 métodos de ciclo de vida. Se invocan en el siguiente orden:

* constructor()
* componentWillMount()
* render()
* componentDidMount()

Pero ¿que pasa con la actualización del ciclo de vida de un componente que sucede cuando el estado o las propiedades cambian? En general, tiene 5 métodos de ciclo de vida en el siguiente orden:

* componentWillReceiveProps()
* shouldComponentUpdate()
* componentWillUpdate()
* render()
* componentDidUpdate()

Por último, pero no menos importante, está el desmontaje del ciclo de vida. Sólo tiene un método de ciclo de vida: `componentWillUnmount()`.

No es necesario conocer todos estos métodos de ciclo de vida desde el principio. Puede ser intimidante, pero no usarás todos - incluso en una aplicación React madura. Sin embargo, es bueno saber que cada método del ciclo de vida se puede usar en casos particulares:

* **constructor(props)** - Se llama cuando el componente se inicializa. Puedes establecer un estado inicial del componente y vincular métodos de clase útiles durante ese método de ciclo de vida.

* **componentWillMount()** - Se llama antes del método del `render()`. Es por eso que podría ser utilizado para establecer el estado del componente interno, Porque no activará una segunda renderización del componente. Generalmente se recomienda utilizar el `constructor()` para establecer el estado inicial.

* **render()** - Este método del ciclo de vida es **obligatorio** y devuelve los elementos como una salida del componente. El método debe ser puro y por lo tanto no debe modificar el estado del componente. Recibe como entrada propiedades (props) y estados (state) y regresa un elemento.

* **componentDidMount()** - Se llama una sola vez cuando el componente fue montado. Ese es el metodo perfecto para realizar una solicitud asincrónica para obtener datos de una API. Los datos obtenidos se almacenan en el estado interno del componente para mostrarlos en el método `render()`.

* **componentWillReceiveProps(nextProps)** - Se llama durante la actualización de un ciclo de vida. Como entrada recibirá las siguientes props . Puedes comparar las props siguientes con las anteriores (`this.props`) para aplicar un comportamiento diferente basado en la diferencia. Además, puede establecer el estado en función de las siguientes props.

* **shouldComponentUpdate(nextProps, nextState)** - Siempre se llama cuando el componente se actualiza debido a cambios de estado o props. Lo usarás en aplicaciones de React maduras para optimizaciones de rendimiento. Dependiendo de un booleano que regrese de este método de ciclo de vida, el componente y todos sus hijos se renderizaran o no en la actualización de un ciclo de vida. Puedes evitar que se ejecute el método de ciclo de vida render de un componente.

* **componentWillUpdate(nextProps, nextState)** - El método del ciclo de vida se invoca antes del método `render()`. Ya se disponen las siguientes props y el próximo estado. Se puede utilizar el método como última oportunidad para realizar las preparaciones antes de ejecutar el método render. Hay que tener en cuenta que ya no se puede activar  `setState()`. Si deseas calcular el estado basado en las siguientes props, tienes que usar `componentWillReceiveProps()`.

* **componentDidUpdate(prevProps, prevState)** - El método del ciclo de vida se invoca inmediatamente después del método `render()`. Puedes usarlo como oportunidad para realizar operaciones DOM o para realizar más solicitudes asíncronas.

* **componentWillUnmount()** - Se llama antes de destruir tu componente. Puedes utilizar el método del ciclo de vida para realizar tareas de limpieza.

Ya has utilizado los metodos `constructor()` y `render()`. Estos son los comunmente utilizados por componentes de clase ES6. Hay que recordar que el metodo `render()` es necesario, de lo contrario no devolveria una instancia del componente.


### Ejercicios:

* Leer mas sobre [Ciclos de vida en React (En ingles)](https://facebook.github.io/react/docs/react-component.html)
* Leer mas sobre [Estado y Ciclo de vida en React (En ingles)](https://facebook.github.io/react/docs/state-and-lifecycle.html)

## Obteniendo Datos

Ahora estás listo para obtener datos de la API de Hacker News. He mencionado un método de ciclo de vida que se puede utilizar para obtener datos: `componentDidMount()`, donde se utilizará la API nativa para realizar la solicitud.

Antes de poder usarlo, vamos a configurar las constantes de url y los parámetros por defecto para dividir la solicitud de API en partes.

~~~~~~~~
import React, { Component } from 'react';
import './App.css';

const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';

...
~~~~~~~~

En JavaScript ES6 puedes usar [plantillas de cadena de texto](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/template_strings) para concatenar cadenas. Usaremos estas plantillas de cadenas de texto para armar la URL con la que vamos a conectarnos en la API (Endpoint)


~~~~~~~~
// ES6
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

// ES5
var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux
~~~~~~~~

Esto ayudara a que la URL sea flexible en el tiempo.

Pero vayamos a la solicitud de API donde usarás la url. El proceso completo de búsqueda de datos se presentará de una vez, Pero cada paso se explicará después.

~~~~~~~~
...

class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      result: null,
      searchTerm: DEFAULT_QUERY,
    };

    this.setSearchTopstories = this.setSearchTopstories.bind(this);
    this.fetchSearchTopstories = this.fetchSearchTopstories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  setSearchTopstories(result) {
    this.setState({ result });
  }

  fetchSearchTopstories(searchTerm) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
      .then(response => response.json())
      .then(result => this.setSearchTopstories(result))
      .catch(e => e);
  }

  componentDidMount() {
    const { searchTerm } = this.state;
    this.fetchSearchTopstories(searchTerm);
  }

  ...
}
~~~~~~~~
Muchas cosas suceden en el codigo anterior. Pensé en mostrarlo en varias partes pero seria mas dificil comprender las relaciones entre cada pieza. Permitanme poder explicarles cada uno en detalle.

En primer lugar, puedes eliminar la lista artificial de elementos, porque obtenemos un resultado de la API de Hacker News. El estado inicial de su componente tiene un resultado vacío y un término de búsqueda predeterminado. El mismo término de búsqueda predeterminado se utiliza en el campo de búsqueda y en su primera solicitud.

En segundo lugar, utilizas el método `componentDidMount()` para obtener los datos después de que el componente se ha montado. En la primera busqueda, se procedera a utilizar el termino de busqueda predeterminado en el estado del componente. Por eso mismo, obtendra historias relacionadas a "redux".

En tercer lugar, la búsqueda nativa se utiliza. Las cadenas de plantilla de JavaScript ES6 le permiten componer la url con el `searchTerm`. TLa url es el argumento de la función API de búsqueda nativa. La respuesta necesita ser transformada en json, es un paso obligatorio en una búsqueda nativa, y finalmente se puede establecer en el estado del componente interno.

Por último, pero no menos importante, no olvide vincular sus nuevos métodos de componentes.

Ahora puede utilizar los datos obtenidos en lugar de la lista artificial de elementos.Sin embargo, tienes que tener cuidado otra vez. El resultado no es sólo una lista de datos. [Es un objeto complejo con meta información y una lista de éxitos (historias).](https://hn.algolia.com/api) Puede emitir el estado interno con `console.log(this.state);` ent tu método `render()` para visualizarlo.

Utilizemos el resultado para mostrarlo. Pero lo preveeremos de renderizar cualquier cosa - return null - cuando no hay resultado. Una vez que la solicitud a la API tuvo éxito, el resultado se guarda en el estado y el componente App se volverá a renderizar con el estado actualizado.

~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;

    if (!result) { return null; }

    return (
      <div className="page">
        ...
        <Table
          list={result.hits}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Repasemos lo que sucede durante el ciclo de vida del componente. El componente es inicializa por el constructor. Después de que se renderiza por primera vez. Pero evitas que muestre, porque el resultado está vacío. Entonces se ejecuta el método `componentDidMount()`. En ese método, obtienes los datos de la API de Hacker News de forma asincrónica. Una vez que los datos llegan, cambia el estado interno del componente. Después de eso, el ciclo de vida de la actualización entra en juego. El componente ejecuta de nuevo el método  `render()`, pero esta vez con datos poblados en su estado interno del componente. El componente y, por tanto, el componente Tabla con su contenido se vuelve a renderizar.

Utilizó la API de recuperación nativa que es compatible con la mayoría de los navegadores para realizar una solicitud asincrónica a una API. La configuración de *create-react-app* garantiza su compatibilidad con todos los navegadores. Hay paquetes de terceros de node que puedes usar para sustituir la API de búsqueda nativa: [superagent](https://github.com/visionmedia/superagent) y [axios](https://github.com/mzabriskie/axios).

Regresa a tu aplicación: La lista de hits debe ser visible ahor. Pero el boton "Dismiss" no funciona. Lo arreglaremos en el siguiente capitulo.

### Ejercicios:

* leer mas sobre [ES6 plantilas de cadena de texto](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/template_strings)
* leer mas sobre [the native fetch API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* experimenta con [Hacker News API](https://hn.algolia.com/api)

## ES6 Operadores de propagación

El botón "Dismiss" no funciona porque el método `onDismiss()` no tiene conocimiento del objeto complejo resultante. Cambiemos eso:

~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
    ...
  });
}
~~~~~~~~

Pero, ¿qué pasa en `setState()` ahora? Desafortunadamente el resultado es un objeto complejo. La lista de hits es sólo una de las múltiples propiedades del objeto. Sin embargo, sólo la lista se actualiza, cuando un elemento se quita en el objeto resultante, mientras que las otras propiedades permanecen igual.

Un enfoque podría ser mutar los hits en el objeto resultante. Lo demostraré, pero no lo haremos de esa manera.

~~~~~~~~
this.state.result.hits = updatedHits;
~~~~~~~~

React abarca la programación funcional. Por lo tanto, no debes mutar un objeto (o mutar el estado directamente). Un mejor enfoque es generar un nuevo objeto basado en la información que tienes. De este modo, ninguno de los objetos se altera. Mantendrás las estructuras de datos inmutables. Siempre devolverás un objeto nuevo y nunca alterarás un objeto.

Vamos a hacerlo en JavaScript ES5. `Object.assign()` toma como primer argumento un objeto destino. Todos los argumentos siguientes son objetos de origen. Estos objetos se combinan en el objeto de destino. El objeto de destino puede ser un objeto vacío. Abarca inmutabilidad, porque ningún objeto fuente se muta. Sería similar a lo siguiente:

~~~~~~~~
const updatedHits = { hits: updatedHits };
const updatedResult = Object.assign({}, this.state.result, updatedHits);
~~~~~~~~

Ahora vamos a hacerlo en el método  `onDismiss()`:

~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
    result: Object.assign({}, this.state.result, { hits: updatedHits })
  });
}
~~~~~~~~

Eso es en JavaScript ES5. Existe una solución más sencilla en ES6 y futuras versiones de JavaScript. ¿Puedo presentarles el operador de propagación? Sólo consta de tres puntos: `...` Cuando se utiliza, cada valor de una matriz u objeto se copia a otra matriz u objeto.

Examinemos el operador de propagación de arreglos de ES6, aunque aún no lo necesites.

~~~~~~~~
const userList = ['Robin', 'Andrew', 'Dan'];
const additionalUser = 'Jordan';
const allUsers = [ ...userList, additionalUser ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

La variables `allUsers`  es un arreglo completamente nuevo. Las otras variables `userList` y `additionalUser` siguen igual. Incluso puedes combinar dos arreglos de esa manera en un nuevo arreglo.

~~~~~~~~
const oldUsers = ['Robin', 'Andrew'];
const newUsers = ['Dan', 'Jordan'];
const allUsers = [ ...oldUsers, ...newUsers ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

Ahora echemos un vistazo al operador de propagación de objetos. ¡No es ES6! Es una [propuesta para una futura versión de ES](https://github.com/sebmarkbage/ecmascript-rest-spread) ya utilizada por la comunidad React. Es por eso que *create-react-app* incorporó la característica en la configuración.

Básicamente es el mismo que el operador de ES6 de dispersión de array de JavaScript pero con objetos. Copia cada par de valores clave en un nuevo objeto.

~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
const user = { ...userNames, age };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

Multiples objetos pueden dispersarse como en el ejemplo de dispersion de arreglos.

~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const userAge = { age: 28 };
const user = { ...userNames, ...userAge };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

Después de todo se puede utilizar para reemplazar ES5 `Object.assign()`.

~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
    result: { ...this.state.result, hits: updatedHits }
  });
}
~~~~~~~~

El boton "Dismiss" debería funcionar de nuevo.

### Ejercicios:

* leer más sobre [Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* leer más sobre the [ES6 array spread operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)
  * the object spread operator is briefly mentioned

## Renderizado Condicional

El renderizado condicional se introduce muy temprano en las aplicaciones React. Sucede cuando se quiere tomar la decisión de renderizar uno u otro elemento. A veces significa renderizar un elemento o nada. Después de todo, una representación condicional de uso más simple puede ser expresada por una sentencia if-else en JSX.


El objeto `resultante` en el estado de interno del componente es nulo al principio. Hasta el momento, el componente App no ​​devuelve elementos cuando el resultado no ha llegado de la API. Eso ya es un renderizado condicional, porque vuelves antes del método `render()` por cierta condición. El componente App no renderiza nada o sus elementos.

Pero vamos un paso más allá. Tiene más sentido envolver el componente Table, que es el único componente que depende del resultado, en un renderizado condicional independiente. Todo lo demás se debe mostrar, aunque no hay ningún `resultado` aún. Simplemente puede utilizar una expresión ternaria en su JSX.

~~~~~~~~
class App extends Component {

  ...

  render() {t
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
        </div>
        { result
          ? <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
          : null
        }
      </div>
    );
  }
}
~~~~~~~~

Esa es tu segunda opción para expresar un renderizado condicional. Una tercera opción es el operador `&&` lógico . En JavaScript, un `true && 'Hello World'` siempre se evalúa como 'Hello World'. Un `false && 'Hello World'` siempre se evalúa como falso.

```js
const result = true && 'Hello World';
console.log(result);
// output: Hello World

const result = false && 'Hello World';
console.log(result);
// output: false
```

En React puedes hacer uso de ese comportamiento. Si la condición es verdadera, la expresión después del operador lógico `&&` será la salida. Si la condición es falsa, React ignora y omite la expresión. Es aplicable en el caso de renderizado condicional de la tabla, ya que debe devolver una Tabla o nada.

~~~~~~~~
{ result &&
  <Table
    list={result.hits}
    pattern={searchTerm}
    onDismiss={this.onDismiss}
  />
}
~~~~~~~~

Estos fueron algunos enfoques para utilizar el renderizado condicional en React. Puedes leer sobre [más alternativas en mi sitio web](https://www.robinwieruch.de/conditional-rendering-react/) donde mantengo una lista exhaustiva de renderizaciones condicionales. Además, conocerá sus diferentes casos de uso y cuándo aplicarlos.

Después de todo, debería poder ver los datos obtenidos en tu aplicación. Todo excepto la Tabla se muestra cuando la búsqueda de datos está pendiente. Una vez que la solicitud resuelve el resultado, se muestra la tabla.

### Ejercicios:

* leer más sobre [React renderizado condicional](https://facebook.github.io/react/docs/conditional-rendering.html)
* leer más sobre [diferentes formas para renderizaciones condicionales](https://www.robinwieruch.de/conditional-rendering-react/)

## Búsqueda por cliente o por servidor

Ahora cuando utilices el campo de búsqueda, filtrarás la lista. Sin embargo eso está sucediendo en el lado del cliente. Ahora vas a utilizar la API de Hacker News para buscar en el servidor. De lo contrario, sólo te ocuparías de la primera respuesta de la API que recibiste de `componentDidMount()` con el parámetro del término de búsqueda predeterminado.

Puede definir un método `onSubmit()` en su componente de clase ES6, que obtenga resultados de la API de Hacker News. Será la misma búsqueda que en tu método `componentDidMount()`. Pero lo buscara con el término modificado de la entrada del campo de búsqueda.

~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      result: null,
      searchTerm: DEFAULT_QUERY,
    };

    this.setSearchTopstories = this.setSearchTopstories.bind(this);
    this.fetchSearchTopstories = this.fetchSearchTopstories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...

  onSearchSubmit() {
    const { searchTerm } = this.state;
    this.fetchSearchTopstories(searchTerm);
  }

  ...
}
~~~~~~~~

El componente de búsqueda obtiene un botón adicional. El botón tiene que activar explícitamente la búsqueda. De lo contrario, obtendrás datos de la API de Hacker News cada vez que tu campo de entrada cambie.

Como alternativa, podría rebatir (retrasar) la funcion `onChange()` y ahorrarte el botón, pero añadiría más complejidad en este momento. Vamos a mantenerlo simple sin un rebote.

Primero, pasa el método `onSearchSubmit()` a tu componente de búsqueda.

```js
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
            onSubmit={this.onSearchSubmit}
          >
            Search
          </Search>
        </div>
        { result &&
          <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
        }
      </div>
    );
  }
}
```

En segundo lugar, introduzce un botón en tu componente de búsqueda. El botón tiene el `type="submit"` y el formulario utiliza su atributo `onSubmit()` para pasar el método `onSubmit()`. Puedes reutilizar la propiedad de los hijos, pero esta vez se utilizará como el contenido del botón.

```js
const Search = ({
  value,
  onChange,
  onSubmit,
  children
}) =>
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
```

En la tabla puedes quitar la funcionalidad de filtro, ya que no habrá más ningún filtro en el cliente (búsqueda). El resultado viene directamente de la API Hacker News después de haber hecho clic en el botón "Buscar".

```js
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        ...
        { result &&
          <Table
            list={result.hits}
            onDismiss={this.onDismiss}
          />
        }
      </div>
    );
  }
}

...

const Table = ({ list, onDismiss }) =>
  <div className="table">
    { list.map(item =>
      ...
    )}
  </div>
```

Ahora cuando intentes buscar, notarás que el navegador se recarga. Es el comportamiento nativo del navegador para un callback en un formulario. En React, a menudo encontrarás el método de evento `preventDefault()` para suprimir el comportamiento nativo del navegador.

```js
onSearchSubmit(event) {
  const { searchTerm } = this.state;
  this.fetchSearchTopstories(searchTerm);
  event.preventDefault();
}
```

Ahora deberías poder buscar diferentes historias de Hacker News. Interactúas con una API del mundo real. No debería haber más búsqueda de lado del cliente.

### Ejercicios:

* leer mas sobre [synthetic events in React](https://facebook.github.io/react/docs/events.html)

## Búsqueda paginada

¿Ha observado de cerca la estructura de datos devuelta? La [Hacker News API](https://hn.algolia.com/api) devuelve más que una lista de visitas. La propiedad de página, que es 0 en la primera respuesta, se puede utilizar para obtener más datos paginados. Sólo tiene que pasar la siguiente página con el mismo término de búsqueda a la API.

Vamos a extender las constantes API ensamblables para que pueda ocuparse de los datos paginados.
~~~~~~~~
const DEFAULT_QUERY = 'redux';
const DEFAULT_PAGE = 0;

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
const PARAM_PAGE = 'page=';
~~~~~~~~

Ahora puedes utilizar estas constantes para agregar el parámetro de página a tu solicitud de API.

```js
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}`;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux&page=
```

El método `fetchSearchTopstories()` tomará la página como segundo argumento. Los métodos `componentDidMount()` y `onSearchSubmit()` toman `DEFAULT_PAGE` para las llamadas iniciales a la API. Deben buscar la primera página en la primera solicitud. Cada búsqueda adicional debe buscar la siguiente página.

```js
class App extends Component {

  ...

  componentDidMount() {
    const { searchTerm } = this.state;
    this.fetchSearchTopstories(searchTerm, DEFAULT_PAGE);
  }

  fetchSearchTopstories(searchTerm, page) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}`)
      .then(response => response.json())
      .then(result => this.setSearchTopstories(result))
      .catch(e => e);
  }

  onSearchSubmit(event) {
    const { searchTerm } = this.state;
    this.fetchSearchTopstories(searchTerm, DEFAULT_PAGE);
    event.preventDefault();
  }

  ...

}
```

Ahora puede utilizar la página actual de la respuesta del API en `fetchSearchTopstories()`. Puedes utilizar este método en un botón para obtener más historias en un clic de botón. Utilicemos el botón para obtener más datos paginados de la API de Hacker News. Sólo necesitas definir la función `onClick()` que toma el término de búsqueda actual y la página siguiente (página actual + 1).

```
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    const page = (result && result.page) || 0;
    return (
      <div className="page">
        <div className="interactions">
        ...
        { result &&
          <Table
            list={result.hits}
            onDismiss={this.onDismiss}
          />
        }
        <div className="interactions">
          <Button onClick={() => this.fetchSearchTopstories(searchTerm, page + 1)}>
            More
          </Button>
        </div>
      </div>
    );
  }
}
```

Debe asegurarte de predeterminar a la página 0 cuando no hay ningún resultado.

Falta un paso. Traes la siguiente página de datos, pero sobreescribirás la página de datos anterior. Deseas concatenar los datos antiguos y nuevos. Vamos a ajustar la funcionalidad para agregar los nuevos datos en lugar de sobrescribirlos.

```
setSearchTopstories(result) {
  const { hits, page } = result;

  const oldHits = page !== 0
    ? this.state.result.hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  this.setState({
    result: { hits: updatedHits, page }
  });
}
```

Primero, obtienes los hits y la página del resultado.

En segundo lugar, usted tiene que comprobar si ya hay antiguos hits.  Cuando la página es 0, es una nueva solicitud de búsqueda de `componentDidMount()` o `onSearchSubmit()`. . Los éxitos están vacíos. Pero cuando hace clic en el botón  "More" para buscar datos paginados, la página no es 0. It is the next page. Es la página siguiente. Los hits antiguos ya están almacenados en tu estado y por lo tanto se pueden utilizar.

En tercer lugar, no deseas sobrescribir los antiguos resultados. Puedes combinar hits antiguos y nuevos de la solicitud de API reciente. La combinación de ambas listas se puede realizar con el operador de distribución de ES6 de JavaScript.

En cuarto lugar, establece los hits combinados y la página en el estado del componente interno.

Puedes hacer un último ajuste. Cuando pruebes el botón "More" sólo obtiene algunos elementos de la lista. El URL de la API se puede extender para buscar más elementos de la lista con cada solicitud. Una vez más, puede agregar más constantes de ruta compuestas.

~~~~~~~~
const DEFAULT_QUERY = 'redux';
const DEFAULT_PAGE = 0;
const DEFAULT_HPP = '100';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
const PARAM_PAGE = 'page=';
const PARAM_HPP = 'hitsPerPage=';
~~~~~~~~

Ahora puede usar las constantes para extender la URL de la API.

~~~~~~~~
fetchSearchTopstories(searchTerm, page) {
  fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
    .then(response => response.json())
    .then(result => this.setSearchTopstories(result))
    .catch(e => e);
}
~~~~~~~~

Posteriormente, la solicitud a la Hacker News API busca más elementos de la lista en una solicitud que antes.

### Ejercicios:

* experimentar con el [Hacker News API parameters](https://hn.algolia.com/api)

## Caché del Cliente

Cada búsqueda envía una solicitud a la API de Hacker News. Puedes buscar "redux", seguido de "react" y eventualmente "redux" de nuevo. En total hace 3 peticiones. Pero buscaste "redux" dos veces y las dos veces tomó todo un viaje de ida y vuelta asíncrono para buscar los datos. En una memoria caché del lado del cliente se almacenara cada resultado. Cuando se realiza una solicitud a la API, comprueba si ya hay un resultado. Si está allí, se utiliza la caché. De lo contrario, se realizará una solicitud de API para recuperar los datos.

Para tener un caché de cliente para cada resultado, tiene que almacenar múltiples `results` en lugar de un `result` en tu estado de componente interno. El objeto de resultados será un mapa con el término de búsqueda como clave y el resultado como valor. Cada resultado de la API se guardará mediante el término de búsqueda (clave).

En este momento tu resultado en el estado del componente es similar al siguiente:

~~~~~~~~
result: {
  hits: [ ... ],
  page: 2,
}
~~~~~~~~

Imagine que ha realizado dos solicitudes de API. Uno para el término de búsqueda "redux" y otro para "react". El mapa de resultados debería tener el siguiente aspecto:

~~~~~~~~
results: {
  redux: {
    hits: [ ... ],
    page: 2,
  },
  react: {
    hits: [ ... ],
    page: 1,
  },
  ...
}
~~~~~~~~

Vamos a implementar una caché en el cliente con React `setState()`. En primer lugar, cambie el nombre del objeto `result` a `results` en el estado inicial del componente.  En segundo lugar, defina una `searchKey` temporal que se utiliza para almacenar cada `result`.

~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
    };

    ...

  }

  ...

}
~~~~~~~~

La `searchKey` debe establecerse antes de realizar cada solicitud. Refleja el `searchTerm`. Usted puede preguntarse: ¿Por qué no usamos el `searchTerm` en primer lugar? El `searchTerm` es una variable fluctuante, ya que se cambia cada vez que escribes en el campo de búsqueda de entrada. Sin embargo, al final necesitarás una variable no fluctuante. Determina el término de búsqueda enviado recientemente a la API y puede utilizarse para recuperar el resultado correcto del mapa de resultados. Es un puntero a su resultado actual en la caché.

~~~~~~~~
componentDidMount() {
  const { searchTerm } = this.state;
  this.setState({ searchKey: searchTerm });
  this.fetchSearchTopstories(searchTerm, DEFAULT_PAGE);
}

onSearchSubmit(event) {
  const { searchTerm } = this.state;
  this.setState({ searchKey: searchTerm });
  this.fetchSearchTopstories(searchTerm, DEFAULT_PAGE);
  event.preventDefault();
}
~~~~~~~~

Ahora tiene que ajustar la funcionalidad donde el resultado se almacena en el estado del componente interno. Debe almacenar cada resultado por `searchKey`.

~~~~~~~~
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
      }
    });
  }

  ...

}
~~~~~~~~

El `searchKey` se utilizará como clave para guardar los resultados y la página actualizados en un mapa de `results`.

Primero, tienes que recuperar el `searchKey` desde el estado del componente. Recuerde que e `searchKey` se pone en `componentDidMount()` y `onSearchSubmit()`.

En segundo lugar, los hits antiguos tienen que fusionarse con los nuevos hits como antes. Pero esta vez los antiguos hits son recuperados del mapa de `results` con el  `searchKey` como clave.

En tercer lugar, un nuevo resultado puede setearse en el mapa `results` en el estado. Examinemos el objeto `results` en `setState()`.

~~~~~~~~
results: {
  ...results,
  [searchKey]: { hits: updatedHits, page }
}
~~~~~~~~

La parte inferior se cerciora de almacenar el resultado actualizado por `searchKey` en el mapa de resultados. El valor es un objeto con una propiedad de visitas y página. El `searchKey` el término de búsqueda. Ya aprendiste la sintaxis `[searchKey]`. Es un nombre de propiedad computado ES6. Le ayuda a asignar valores dinámicamente en un objeto.

TLa parte superior tiene que objetar la propagación de todos los demás resultados por `searchKey` en el estado. De lo contrario, perdería todos los resultados almacenados anteriormente.

Ahora almacena todos los resultados por término de búsqueda. Ese es el primer paso para habilitar su caché. En el siguiente paso, puede recuperar el resultado dependiendo del término de búsqueda de su mapa de resultados.

~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey
    } = this.state;

    const page = (
      results &&
      results[searchKey] &&
      results[searchKey].page
    ) || 0;

    const list = (
      results &&
      results[searchKey] &&
      results[searchKey].hits
    ) || [];

    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
        <Table
          list={list}
          onDismiss={this.onDismiss}
        />
        <div className="interactions">
          <Button onClick={() => this.fetchSearchTopstories(searchKey, page + 1)}>
            More
          </Button>
        </div>
      </div>
    );
  }
}
~~~~~~~~

Puesto que definiste a una lista vacía cuando no hay resultado por `searchKey`, Ahora puedes ahorrate el renderizado condicional para el componente Tabla. Además, tendrá que pasar el `searchKey` en lugar del `searchTerm` al botón "More". De lo contrario, su búsqueda paginada depende del valor `searchTerm` que es fluctuante. Además, asegúrese de mantener la propiedad fluctuante `searchTerm` para el campo de entrada en el componente "Search".

La funcionalidad de búsqueda debería funcionar de nuevo. Almacena todos los resultados de la API de Hacker News.

Además, el método `onDismiss()` necesita ser mejorado. Todavía se ocupa del objeto `result`. Ahora tiene que tratar con múltiples `results`.

~~~~~~~~
  onDismiss(id) {
    const { searchKey, results } = this.state;
    const { hits, page } = results[searchKey];

    const isNotId = item => item.objectID !== id;
    const updatedHits = hits.filter(isNotId);

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      }
    });
  }
~~~~~~~~

El botón "Dismiss" debería funcionar de nuevo.

Sin embargo, nada impide que la aplicación envíe una solicitud de API en cada envío de búsqueda. Aunque puede haber ya un resultado, no hay ninguna comprobación que impida la solicitud. La funcionalidad de caché no está completa todavía. El último paso sería evitar la solicitud cuando un resultado está disponible en la caché.

~~~~~~~~
class App extends Component {

  constructor(props) {

    ...

    this.needsToSearchTopstories = this.needsToSearchTopstories.bind(this);
    this.setSearchTopstories = this.setSearchTopstories.bind(this);
    this.fetchSearchTopstories = this.fetchSearchTopstories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  needsToSearchTopstories(searchTerm) {
    return !this.state.results[searchTerm];
  }

  ...

  onSearchSubmit(event) {
    const { searchTerm } = this.state;
    this.setState({ searchKey: searchTerm });

    if (this.needsToSearchTopstories(searchTerm)) {
      this.fetchSearchTopstories(searchTerm, DEFAULT_PAGE);
    }

    event.preventDefault();
  }

  ...

}
~~~~~~~~

Ahora su cliente hace una solicitud a la API sólo una vez, aunque usted busca un término de búsqueda dos veces. Incluso los datos paginados con varias páginas se almacenan en caché de esa manera, porque siempre se guarda la última página de cada resultado en el mapa `results`.

¡Has aprendido a interactuar con una API en React! Repasemos los últimos capítulos:

* React
  * Métodos del ciclo de vida de los componentes de clase ES6 para diferentes casos de uso
  * componentDidMount() para las interacciones API
  * renderizados condicionales
  * eventos sintéticos en los formularios
* ES6
  * cadenas de plantilla para componer cadenas
  * operador de propagación para estructuras de datos inmutables
  * nombres de propiedad calculados
* General
  * Interacción con la API de Hacker News
  * busqueda nativa con la API del navegador
  * búsqueda de cliente y servidor
  * paginación de datos
  * almacenamiento en caché del lado del cliente

Una vez más tiene sentido tomar un descanso. Internalizar los aprendizajes y aplicarlos por su cuenta. Puede experimentar con el código fuente que ha escrito hasta ahora.

Puedes encontrar el código fuente en el [repositorio oficial](https://github.com/rwieruch/hackernews-client/tree/e60436a9d6c449e76a362aef44dd5667357b7994).
