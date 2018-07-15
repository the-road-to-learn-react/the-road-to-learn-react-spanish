# Conceptos básicos en React

Este capítulo te guiará a través de los aspectos básicos de React. Habla sobre lo que es el estado y las interacciones en componentes, pués, los componentes estáticos son un poco aburridos ¿no? Además, aprenderás diferentes maneras de declarar un componente y cómo mantenerlos ensamblables y reutilizables. Prepárate para darle vida a tus componentes.

## Estado interno del componente

El estado interno de un componente, también conocido cómo estado local, te permite almacenar, modificar y eliminar propiedades almacenadas dentro de un componente. El componente de clase ES6 puede utilizar un constructor para inicializar el estado interno del componente. El constructor se llama una sola vez cuando el componente se inicializa.

A continuación, conozcamos el constructor de clase donde se puede establecer el estado interno inicial del componente.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

# leanpub-start-insert
  constructor(props) {
    super(props);
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

El componente `App` es una subclase de `Component`, a esto se debe el `extends Component` en la declaración del componente `App`. Más adelante conocerás más acerca de componentes de clase ES6.

Es obligatorio llamar a `super(props);`, estableciendo así `this.props` dentro de tu constructor, en caso de que quieras acceder a el. De lo contrario, al intentar accesar a `this.props` retornará `undefined`.

Ahora, en tu caso, el estado inicial en tu componente debería ser la lista de elementos de muestra.

{title="src/App.js",lang=javascript}
~~~~~~~~
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  ...
];

class App extends Component {

  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      list: list,
    };
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

El estado está ligado a la clase por medio del objeto `this`. Por lo tanto, puedes acceder al estado local dentro de todo el componente. Por ejemplo, puede ser utilizado en el método `render()`. Anteriormente mapeaste una lista estática de elementos en tu método `render()`, que fué definido fuera del componente. Ahora, usarás la lista proveniente del estado local dentro de tu componente.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {this.state.list.map(item =>
# leanpub-end-insert
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Esta lista es parte del componente ahora, pues, reside en el estado interno del componente.  Podrías fácilmente agregar artículos, cambiarlos o quitarlos de esta lista. Cada vez que cambies el estado del componente, el metodo `render()` de tu componente se ejecutará de nuevo. Así es como puedes fácilmente cambiar el estado de un componente interno y asegurarte de que el componente se vuelva a renderizar y muestre la información correcta proveniente del estado local.

Pero ten cuidado. No cambies el estado directamente. Tienes que usar un método llamado `setState()` para modificarlo. Este método lo conocerás en un próximo capítulo.

### Ejercicios:

* experimenta con el estado interno
  * define mas estados iniciales dentro del contructor
  * usa y accede al estado dentro de tu metodo `render()`
* lee más sobre [el constructor de clase ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)

## Inicializador de Objetos ES6

En JavaScript ES6 puedes utilizar una sintaxis abreviada para inicializar objetos de manera concisa. Imagina la siguiente inicialización de objeto:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name: name,
};
~~~~~~~~

Cuando el nombre de una propiedad dentro de un objeto es igual al nombre de la variable, puedes hacer lo siguiente:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name,
};
~~~~~~~~

En tu aplicación puedes hacer lo mismo. El nombre de la variable de lista y el nombre de la propiedad de estado comparten el mismo nombre.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
this.state = {
  list: list,
};

// ES6
this.state = {
  list,
};
~~~~~~~~

Los nombres abreviados de método también te ayudarán mucho. En JavaScript ES6 puedes inicializar métodos en un objeto de manera más concisa.


{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var userService = {
  getUserName: function (user) {
    return user.firstname + ' ' + user.lastname;
  },
};

// ES6
const userService = {
  getUserName(user) {
    return user.firstname + ' ' + user.lastname;
  },
};
~~~~~~~~

Y por último pero no menos importante, en JavaScript ES6 es posible utilizar nombres de propiedad calculados.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var user = {
  name: 'Robin',
};

// ES6
const key = 'name';
const user = {
  [key]: 'Robin',
};
~~~~~~~~

Es posible que los nombres de propiedad calculados suenen cómo algo extraño para ti en este momento. ¿Por qué los necesitarías? En un capítulo posterior del libro, responderemos esta interrogante cuándo los utilizes para insertar valores dentro de un objeto de manera dinámica.

### Ejercicios:

* experimenta con el inicializador de objetos ES6
* lee más sobre [inicializador de objetos ES&](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer)

## Flujo de datos unidireccional

Ahora tienes un estado interno en tu componente `App`. Sin embargo, no has manipulado su estado interno todavía. El estado es estático y por lo tanto también lo es el componente. Una buena manera de experimentar con la manipulación de estado es generando interacciones entre componentes.

Agreguemos un botón a cada elemento de la lista a continuación. El botón tendra el nombre "Dismiss" y permitirá remover dicho elemento de la lista. Será útil eventualmente, cómo cuando sólo desees mantener una lista de elementos no leídos, y eliminar los elementos en los que no estes interesado.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
# leanpub-start-insert
            <span>
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
# leanpub-end-insert
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

El método de clase `onDismiss()` aún no está definido, nos haremos cargo de ello en un momento. Por ahora enfocate en el selector `onClick` perteneciente al elemento `button`.

Como puedes ver, el método `onDismiss()` en la función `onClick` está encerrado dentro de otra función. De esta manera puedes ubicarte en la propiedad `objectID` perteneciente al objeto `item`, y así identificar el elemento que será eliminado al presionar el botón correspondiente. Una manera alternativa sería, definiendo la función fuera del selector `onClick`, y solamente pasar la función definida al selector. Más adelante explicaré el tema de los selectores de elementos con más detalle.

¿Notaste las multilíneas para el elemento `button`? Elementos con multiples atributos en una sola línea eventualmentese desordenan. Es por eso que para definir el elemento `button` y sus propiedades se utilizan multilíneas e identado, manteniendo todo legible. Esto no es obligatorio, sólo una pequeña recomendación.

Ahora, tienes que implementar la funcionalidad `onDismiss()`. Se necesita un `id` para identificar el elemento a descartar. La función está vinculada a la clase y por lo tanto, se convierte así en un método de clase, por esta razón accesas a el con `this.onDismiss()` y no `onDismiss()`. El objeto `this` representa la instanciación de tu clase. Ahora, para definir `onDismiss()` cómo método de clase, necesitas enlazarlo con el constructor.


{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-end-insert
  }

  render() {
    ...
  }
}
~~~~~~~~

En el siguiente paso, tienes que definir su funcionalidad, la lógica, en tu clase. Métodos de clase pueden ser definidos de la siguiente manera.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onDismiss(id) {
    ...
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Ahora, puedes definir lo que sucede dentro del método de clase. Básicamente, quieres quitar de la lista el artículo identificado con el `id` y almacenar una lista actualizada en tu estado local. Al final, la lista actualizada será usada dentro del método `render()` para mostrarse en pantalla. El elemento removido no debería ser visible ahora.

 Puedes remover un elemento de una lista utilizando la funcionalidad de filtro de array. La función de filtro toma una función para evaluar cada elemento de la lista iterando sobre esta. Si la evaluación de un item resulta en verdadero, el elemento se queda en la lista. De lo contrario se eliminará. Además, la función devuelve una nueva lista y no altera la lista antigua. Mantiene la estructura de datos inmutables.


{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(function isNotId(item) {
    return item.objectID !== id;
  });
# leanpub-end-insert
}
~~~~~~~~

En el siguiente paso, puedes extraer la función y pasarla a la función filtro.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  function isNotId(item) {
    return item.objectID !== id;
  }

  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

Esto puede hacercerse de forma más concisa utilizando una función flecha ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

Podrías incluso hacerlo en una línea, como hiciste con el selector `onClick()` del botón, aunque podría ser menos legible.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(item => item.objectID !== id);
# leanpub-end-insert
}
~~~~~~~~

La lista elimina ahora el elemento seleccionado. Sin embargo, el estado aún no se actualiza. Por lo tanto, puedes utilizar el metodo de clase `setState()` para actualizar la lista en el estado interno del componente.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-start-insert
  this.setState({ list: updatedList });
# leanpub-end-insert
}
~~~~~~~~

Ahora, vuelve a ejecutar tu aplicación y prueba el botón "Dismiss". Debería funcionar correctamente.

Esto que acabas de experimentar, dentro de React es conocido como **flujo de datos unidireccional**. Ejecutas una acción en la capa de vistas con `onClick()`, acto seguido una función o método de clase modifica el estado interno del componente y el metodo `render()` del componente se ejecuta de nuevo para actualizar la capa de vistas.

![Actualización del estado interno con flujo de datos unidireccional](images/set-state-to-render-unidirectional.png)

### Ejercicios:

* leer más sobre [El estado y ciclos de vida en React](https://facebook.github.io/react/docs/state-and-lifecycle.html)

## Interacciones con Formularios y Eventos

Añadamos otra interacción para experimentar formularios y eventos en React. La interacción es una funcionalidad de búsqueda. La entrada del campo de búsqueda se debe utilizar para filtrar la lista basada en la propiedad de título de un elemento.

Primero defina su campo de entrada en su JSX.

~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input type="text" />
        </form>
        { this.state.list.map(item =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

En el escenario siguiente, se escribirá en el campo y se filtrará la lista temporalmente por el término de búsqueda. Para poder filtrar la lista, necesitas el valor del campo de entrada para actualizar el estado. Pero, ¿cómo acceder al valor? Puede utilizar **eventos sintéticos** en  React para acceder a la carga útil del evento.

Vamos a definir una funcion callbakc `onChange()` para el campo de entrada.

~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

La función está vinculada al componente y, por tanto, un método de clase de nuevo. Tienes que vincular y definir el métodos.

~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  onSearchChange() {
    ...
  }

  ...
}
~~~~~~~~

El argumento del método le da acceso al evento React sintético. El evento tiene el valor del campo de entrada en su objeto de destino. Ahora puedes manipular el estado para el término de búsqueda:

~~~~~~~~
class App extends Component {

  ...

  onSearchChange(event) {
    this.setState({ searchTerm: event.target.value });
  }

  ...
}
~~~~~~~~

Además hay que definir el estado inicial para el `searchTerm` en el constructor

~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
      searchTerm: '',
    };

    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

Ahora almacena el valor de entrada en su estado de componente interno cada vez que el valor en el campo de entrada cambia. Sin embargo, la lista no se actualiza todavía. Deberá filtrar la lista temporalmente en función del `searchTerm`. Eso es bastante simple. Antes de asignar la lista puede aplicar un filtro en ella. Ya ha utilizado la funcionalidad de filtro JavaScript incorporada.

~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
        { this.state.list.filter(...).map(item =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Aproximemos la función de filtro de una manera diferente esta vez. Queremos definir el argumento del filtro - la funcion - fuera de nuestro componente de clase ES6. Allí no tenemos acceso al estado del componente - por lo tanto no tenemos acceso a la propiedad `searchTerm` para evaluar la condición del filtro. Tenemos que pasar el `searchTerm` a la función de filtro y tienen que devolver una nueva función para evaluar la condición. Eso se llama una función de orden superior.

Normalmente no mencionaría las funciones de orden superior, pero en un libro de React tiene total sentido. Tiene sentido conocer las funciones de orden superior, Porque React trata con un concepto llamado componentes de orden superior. Conocerás el concepto más adelante en el libro. Ahora de nuevo, vamos a enfocarnos en la funcionalidad del filtro.

Primero tienes que definir la función de orden superior fuera de tu clase.

~~~~~~~~
function isSearched(searchTerm) {
  return function(item) {
    // some condition which returns true or false
  }
}

class App extends Component {

  ...

}
~~~~~~~~

La función toma el `searchTerm` y devuelve otra función que toma un elemento. La función devuelta se utilizará para filtrar la lista en función de la condición definida en la función.

Let's define the condition.

~~~~~~~~
function isSearched(searchTerm) {
  return function(item) {
    return !searchTerm ||
      item.title.toLowerCase().includes(searchTerm.toLowerCase());
  }
}

class App extends Component {

  ...

}
~~~~~~~~

La condición dice varias cosas. Filtrar la lista sólo cuando esta establecido `searchTerm`. Cuando se establece un `searchTerm`, Usted coincide con el patrón `searchTerm` entrante con el título del elemento. Puedes hacerlo con la funcionalidad incorporada  en JavaScript `includes`. Sólo cuando el patrón coincide, devuelve true y el elemento permanece en la lista. Pero tenga cuidado con la coincidencia de patrones: No debes olvidar las minúsculas de ambas cadenas. De lo contrario, habrá desajustes entre un término de búsqueda 'redux' y un título de artículo 'Redux'.

Una cosa queda por mencionar: Hemos engañado un poco utilizando la funcionalidad incluida en JavaScript. Ya es una característica ES6. ¿Cómo se vería eso en JavaScript ES5? Usted usaría la funcion `indexOf()` para obtener el índice del elemento en la lista. Cuando el elemento está en la lista, `indexOf()` devolverá un índice positivo.

~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

Otra impecable refactorización se puede hacer de nuevo con una ES6 arrow function. Hace que la función sea más concisa:

~~~~~~~~
// ES5
function isSearched(searchTerm) {
  return function(item) {
    return !searchTerm || item.title.toLowerCase().includes(searchTerm.toLowerCase());
  }
}

// ES6
const isSearched = (searchTerm) => (item) =>
  !searchTerm || item.title.toLowerCase().includes(searchTerm.toLowerCase());
~~~~~~~~

Uno podría discutir qué función es más legible. Personalmente prefiero la segundo. El ecosistema de React utiliza una gran cantidad de conceptos de programación funcional. Sucede a menudo que usted utilizará una función que devuelve una función (funciones de orden superior). En ES6 puedes expresarlas de forma más concisa con arrow functions.

Por último, pero no menos importante, tienes que usar la funcion definida `isSearched()` para filtrar tu lista.

~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
        { this.state.list.filter(isSearched(this.state.searchTerm)).map(item =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Ahora la funcionalidad de búsqueda debería funcionar ahora. Pruebala.

### Ejercicios:

* leer mas sobre [eventos React](https://facebook.github.io/react/docs/handling-events.html)
* leer mas sobre [funciones de orden](https://en.wikipedia.org/wiki/Higher-order_function)

## Desestructuración ES6

Hay una manera en ES6 para acceder a las propiedades en objetos y arrays fácilmente. Se llama desestructuración. Comparar el fragmento siguiente en JavaScript ES5 y ES6.

~~~~~~~~
const user = {
  firstname: 'Robin',
  lastname: 'Wieruch',
};

// ES5
var firstname = user.firstname;
var lastname = user.lastname;

// ES6
const { firstname, lastname } = user;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch
~~~~~~~~

Mientras tiene que agregar una línea extra cada vez que desee acceder a una propiedad de objeto en ES5, puedes hacerlo en una línea en ES6. Además, no es necesario tener nombres de propiedad duplicados. Una mejor práctica para la legibilidad es utilizar multilíneas cuando desestructuras un objeto en múltiples propiedades.

~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

Lo mismo ocurre con los arrays. También puede desestructurarlos, pero manténgalos más legibles con multilines..

~~~~~~~~
const users = ['Robin', 'Andrew', 'Dan'];
const [
  userOne,
  userTwo,
  userThree
] = users;

console.log(userOne, userTwo, userThree);
// output: Robin Andrew Dan
~~~~~~~~

Quizás notaste que el estado en el componente App puede destructurarse de la misma manera.Puede acortar el filtro y la línea map del código
~~~~~~~~
  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        ...
        { list.filter(isSearched(searchTerm)).map(item =>
          ...
        )}
      </div>
    );
~~~~~~~~

Puede hacerlo de la manera ES5 o ES6:

~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

Pero como el libro utiliza JavaScript ES6 la mayor parte del tiempo, debes atenerse a ES6.

### Ejercicios:

* leer mas sobre [ES6 destructuring](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## Componentes Controlados

Ya has aprendido sobre el flujo de datos unidireccional en React. La misma ley se aplica al campo de entrada, que actualiza el estado que a su vez filtra la lista. El estado fue cambiado, el metodo `render()` e ejecuta de nuevo y utiliza el estado reciente `searchTerm` para aplicar la condición de filtro.

¿Pero no nos olvidamos de algo en el elemento de entrada? Una etiqueta de entrada HTML viene con un atributo `value`. El atributo valor normalmente tiene el valor que se muestra en el campo de entrada - en nuestro caso la propiedad `searchTerm`. Sin embargo, parece que no lo necesitamos en React.

Eso está mal. Elemnetos de formulario como `<input>`, `<textarea>` y `<select>` mantiene su propio estado. Modifican el valor internamente una vez que alguien lo cambia desde el exterior. En React se les llama **componente no controlado**, porque maneja su propio estado. En React, debe asegurarse de que estos elementos son **componentes controlados**.

¿Cómo debes hacer eso? Sólo tiene que establecer el atributo de valor del campo de entrada. El valor ya está guardado en la propiedad de estado `searchTerm`.

~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <form>
          <input
            type="text"
            value={searchTerm}
            onChange={this.onSearchChange}
          />
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

Eso es todo. El ciclo de flujo de datos unidireccional para el campo de entrada es autónomo ahora. El estado componente interno es la única fuente de verdad para el campo de entrada.

Toda la gestión interna del estado y el flujo de datos unidireccional podría ser nuevo para usted. Pero una vez que esté acostumbrado a él, será su flujo natural para implementar las cosas en React. En general, React trajo un nuevo patrón con el flujo de datos unidireccional al mundo de las aplicaciones de una sola página. Es adoptado por varios frameworks y librerias.

### Ejercicios:

* leer mas sobre [React forms](https://facebook.github.io/react/docs/forms.html)

## Dividir Componentes

Ahora tienes un componente de App grande. Sigue creciendo y sera confuso eventualmente. Puede comenzar a dividirlo en pedazos - componentes más pequeños.

Comencemos a utilizar un componente para la entrada de búsqueda y un componente para la lista de elementos.

~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <Search />
        <Table />
      </div>
    );
  }
}
~~~~~~~~

Puede pasar las propiedades de los componentes que pueden utilizar ellos mismos.

~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        />
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Ahora puede definir los componentes junto a su componente App. Estos componentes serán también componentes de la clase ES6. Renderizan los mismos elementos como antes
.

El primero es el componente de búsqueda.

~~~~~~~~
class App extends Component {
  ...
}

class Search extends Component {
  render() {
    const { value, onChange } = this.props;
    return (
      <form>
        <input
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
~~~~~~~~

El segundo es el componente Tabla.

~~~~~~~~
...

class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        { list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
              <button
                onClick={() => onDismiss(item.objectID)}
                type="button"
                >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Ahora tiene tres componentes de clase ES6. PTal vez usted ha notado el objeto `this.props`. Las props - abrevitura para las propiedades - tienen todos los valores que han pasado a los componentes cuando los utiliza en su componente App. Usted podría reutilizar estos componentes en otro lugar, pero pasarles diferentes valores. Son reutilizables.

### Ejercicios:

* averiguar qué componentes podría dividir
  * pero no lo haga ahora, de lo contrario se encontrará con conflictos en los próximos capítulos

## Componentes Ensamblables

Hay una propiedad más pequeña que es accesible en el objeto props: la propiedad `hijo`. Puedes utilizarlo para pasar elementos a sus componentes desde arriba - Que son desconocidos para el componente mismo - pero hacen posible ensamblar componentes entre sí. Veamos cómo se ve esto cuando solo pasa un texto (string) como hijo al componente Search.

~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        >
          Search
        </Search>
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Ahora, el componente de búsqueda puede desestructurar la propiedad children de props. A continuación, puede especificar dónde deben mostrarse los hijos.

~~~~~~~~
class Search extends Component {
  render() {
    const { value, onChange, children } = this.props;
    return (
      <form>
        {children} <input
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
~~~~~~~~

Ahora el texto "Search" debe estar visible al lado de su campo de entrada. Cuando utilice el componente Search en algún otro lugar, puedes elegir un texto diferente si tu quieres. Después de todo, no es sólo el texto que puede pasar como hijos. Puedes pasar un elemento y arboles de elementos (que puede ser encapsulado por los componentes de nuevo) como hijos. La propiedad hijo hace posible tejer componentes entre sí.

### Ejercicios:

* leer mas sobre [the composition model of React](https://facebook.github.io/react/docs/composition-vs-inheritance.html)

## Componentes Reutilizables

Los componentes reutilizables y componibles le permiten crear jerarquías de componentes capaces. Son la base de su capa de vista. Los últimos capítulos mencionan a menudo el término reutilización. Puedes volver a utilizar los componentes Table y Search. No olvides el componente App.

Vamos a definir un componente más reutilizable - un componente Button - que eventualmente se reutiliza más a menudo.

~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
      className,
      children,
    } = this.props;

    return (
      <button
        onClick={onClick}
        className={className}
        type="button"
      >
        {children}
      </button>
    );
  }
}
~~~~~~~~

Podría parecer superfluo declarar tal componente. Usaras un `Button` en lugar de un `button`. Sólo ahorra el `type="button"`. Excepto para el atributo type, debe definir todo lo demás cuando desee utilizar el componente Button. Pero hay que pensar en la inversión a largo plazo aquí. Imagine que tiene varios botones en su aplicación, pero desea cambiar un atributo, estilo o comportamiento para el botón. Sin el componente tendrías que refactorizar cada botón. En lugar del componente Button asegurate de tener una única fuente de verdad. Un botón para refactorizar todos los botones a la vez.

Ya que ya tienes un elemento button, puedes utilizar el componente Button en su lugar. Omite el atributo de tipo.

~~~~~~~~
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        { list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
              <Button onClick={() => onDismiss(item.objectID)}>
                Dismiss
              </Button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

El componente Button espera una propiedad `className` property en las props. Pero no pasamos ninguna `className` cuando el Button fue usado. Debe ser más explícito en el componente Button que el `className` es opcional.

Puedes utilizar una característica de JavaScript ES6: el parámetro predeterminado.

~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
      className = '',
      children,
    } = this.props;

    ...
  }
}
~~~~~~~~

Ahora, siempre que no haya ninguna propiedad `className`, ll valor será una cadena vacía.

### Ejercicios:

* leer mas sobre [ES6 default parameters](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)

## Declaraciones de componentes

Por ahora tienes cuatro componentes de clase ES6. Pero puedes hacerlo mejor. Permítanme introducir componentes funcionales sin estado como alternativa para componentes de clase ES6. Antes de refactorizar sus componentes, vamos a introducir los diferentes tipos de componentes.

* **Componentes funcionales sin esatdo:** Estos componentes son funciones que reciben una entrada y devuelven una salida. La entrada es el objeto props. La salida es una instancia de componente. Hasta ahora es bastante similar a un componente de clase ES6. Sin embargo, los componentes funcionales sin estados son funciones (funcional) y no tinene un estado interno (stateless). No puede acceder al estado con `this.state` porque no hay un objeto `this`. Además, no tienen métodos de ciclo de vida. Todavía no has aprendido los métodos del ciclo de vida, pero ya usaste dos: `constructor()` y `render()`. Mantenga este hecho en mente sobre los componentes funcionales sin estado, cuando llegue al capítulo de métodos de ciclo de vida más adelante.

* **Componentes de clase ES6:** Ya utilizaste este tipo de declaración de componente. En la definición de clase se extienden desde el componente React. El `extend` engancha todos los métodos del ciclo de vida - disponibles en la API del componente React - al componente. Como he mencionado, ya usaste dos de ellos. Además, puedes almacenar y manipular el estado en componentes de clase ES6.

* **React.createClass:** Esta declaración de componente se utilizó en versiones anteriores de React y aún se utiliza en aplicaciones de JavaScript ES5 React. Pero [Facebook  lo declaró obsoleto](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html) en favor de ES6. Incluso añadieron un [Advertencia de depreciación en la versión 15.5](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html). No lo usarás en el libro.

Pero, ¿cuándo usar componentes funcionales sin estado sobre componentes de clase ES6? Una regla general es usar componentes funcionales sin estado cuando no se necesitan métodos de ciclo de vida de componentes o componentes internos. Por lo general, comienza a implementar sus componentes como componentes funcionales sin estado. Una vez que necesite acceder a los métodos de estado o de ciclo de vida, debe refactorizarlo a un componente de clase ES6.

Volvamos a tu aplicación. El componente App utiliza el estado interno. Es por eso que tiene que permanecer como un componente de clase ES6. Pero los otros tres componentes de su clase ES6 son sin estado, sin métodos de ciclo de vida. Vamos a refactorizar juntos el componente de búsqueda a un componente funcional sin estado. La refactorización del componente Tabla y botón seran tu ejercicio.

~~~~~~~~
function Search(props) {
  const { value, onChange, children } = props;
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

Básicamente eso es todo. Pero puedes hacer más código inteligente en un componente funcional sin estado. Ya conoces la desestructuración ES6. La mejor práctica es utilizarla en la firma de funciones para desestructurar las props.

~~~~~~~~
function Search({ value, onChange, children }) {
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

Pero puede mejorar. Ya sabes que las funciones de flecha ES6 te permiten mantener tus funciones concisas. Puedes quitar el cuerpo del bloque de la función. En un cuerpo conciso un retorno implícito se adjunta así que usted puede quitar la declaración return. Dado que su componente funcional sin estado es una función, puede mantenerla concisa también.

~~~~~~~~
const Search = ({ value, onChange, children }) =>
  <form>
    {children} <input
      type="text"
      value={value}
      onChange={onChange}
    />
  </form>
~~~~~~~~

El último paso es especialmente útil para hacer cumplir sólo tener props como entrada y un elemento como salida. Nada en el medio. Sin embargo, podrías *hacer algo* usando un cuerpo del bloque en su función de la flecha ES6.

~~~~~~~~
const Search = ({ value, onChange, children }) => {

  // do something

  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

Pero no lo necesitas por ahora. Es por eso que puede mantener la versión anterior sin el cuerpo del bloque.

Ahora usted tiene un componente funcional ligero sin estado. Una vez que necesitarás tener acceso a su estado de componente interno o métodos de ciclo de vida, lo refactorizarías a un componente de clase ES6. Además, usted vio cómo se puede usar JavaScript ES6 en los componentes React para hacerlos más elegantes.

### Ejercicios:

* Refactorizar el componente de tabla y botón a componentes funcionales sin estado
* leer mas sobre [ES6 class components and functional stateless components](https://facebook.github.io/react/docs/components-and-props.html)

## Estilización de Componentes

Vamos a añadir un estilo básico a nuestra aplicación y sus componentes. Puedes reutilizar los archivos *src/App.css* y *src/index.css*. Estos archivos deben estar ya en su proyecto desde que lo inició con create-react-app. Tambien deben ser importados en sus archivos *src/App.js* y *src/index.js*. He preparado algunos CSS que puede simplemente copiar y pegar en estos archivos, pero no dude en utilizar su propio estilo.

~~~~~~~~
body {
  color: #222;
  background: #f4f4f4;
  font: 400 14px CoreSans, Arial,sans-serif;
}

a {
  color: #222;
}

a:hover {
  text-decoration: underline;
}

ul, li {
  list-style: none;
  padding: 0;
  margin: 0;
}

input {
  padding: 10px;
  border-radius: 5px;
  outline: none;
  margin-right: 10px;
  border: 1px solid #dddddd;
}

button {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #dddddd;
  background: transparent;
  color: #808080;
  cursor: pointer;
}

button:hover {
  color: #222;
}

*:focus {
  outline: none;
}
~~~~~~~~


~~~~~~~~
.page {
  margin: 20px;
}

.interactions {
  text-align: center;
}

.table {
  margin: 20px 0;
}

.table-header {
  display: flex;
  line-height: 24px;
  font-size: 16px;
  padding: 0 10px;
  justify-content: space-between;
}

.table-empty {
  margin: 200px;
  text-align: center;
  font-size: 16px;
}

.table-row {
  display: flex;
  line-height: 24px;
  white-space: nowrap;
  margin: 10px 0;
  padding: 10px;
  background: #ffffff;
  border: 1px solid #e3e3e3;
}

.table-header > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.table-row > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.button-inline {
  border-width: 0;
  background: transparent;
  color: inherit;
  text-align: inherit;
  -webkit-font-smoothing: inherit;
  padding: 0;
  font-size: inherit;
  cursor: pointer;
}

.button-active {
  border-radius: 0;
  border-bottom: 1px solid #38BB6C;
}
~~~~~~~~

Ahora puede utilizar el estilo en algunos de sus componentes. No te olvides de utilizar `className` de React en lugar de `class` como atributo HTML.

Primero, aplícalo en tu componente de la clase App ES6.

~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
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
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Segundo, aplíquelo en su componente funcional sin estado Tabla.

~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
  <div className="table">
    { list.filter(isSearched(pattern)).map(item =>
      <div key={item.objectID} className="table-row">
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
        <span>
          <Button
            onClick={() => onDismiss(item.objectID)}
            className="button-inline"
          >
            Dismiss
          </Button>
        </span>
      </div>
    )}
  </div>
~~~~~~~~

Ahora has estilizado tu aplicación y componentes con CSS basico. Debe lucir decente. Como sabes, JSX mezcla HTML y JavaScript. Uno podría discutir para agregar CSS en la mezcla también. Eso se llama estilo en línea. Puede definir objetos JavaScript y pasarlos al atributo de estilo de un elemento.

Mantengamos flexibles la anchura de columna de la tabla mediante el uso del estilo en línea.

~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
  <div className="table">
    { list.filter(isSearched(pattern)).map(item =>
      <div key={item.objectID} className="table-row">
        <span style={{ width: '40%' }}>
          <a href={item.url}>{item.title}</a>
        </span>
        <span style={{ width: '30%' }}>
          {item.author}
        </span>
        <span style={{ width: '10%' }}>
          {item.num_comments}
        </span>
        <span style={{ width: '10%' }}>
          {item.points}
        </span>
        <span style={{ width: '10%' }}>
          <Button
            onClick={() => onDismiss(item.objectID)}
            className="button-inline"
          >
            Dismiss
          </Button>
        </span>
      </div>
    )}
  </div>
~~~~~~~~

Está realmente en línea ahora. Usted podría definir los objetos de estilo fuera de sus elementos para hacerlo más limpio..

~~~~~~~~
const largeColumn = {
  width: '40%',
};

const midColumn = {
  width: '30%',
};

const smallColumn = {
  width: '10%',
};
~~~~~~~~

Después de eso podrías utilizarlo en tus columnass: `<span style={smallColumn}>`.

En general, te encontrarás con diferentes opiniones y soluciones para el estilo en React. Justo utilizaste puro estilo CSS y en línea ahora. Es suficiente para empezar.

No quiero ser dogmático aquí, pero quiero dejarte más opciones. Puedes leer sobre ellos y aplicarlos por su cuenta. Pero si eres nuevo en React, te recomendaría que te mantengas puro estilo CSS y en línea por ahora.

* [radium](https://github.com/FormidableLabs/radium)
* [aphrodite](https://github.com/khan/aphrodite)
* [styled-components](https://github.com/styled-components/styled-components)
* [CSS Modules](https://github.com/css-modules/css-modules)

¡Has aprendido los fundamentos de React que te permitirán escribir tus propias aplicaciónes! Repasemos los últimos capítulos:

* React
  * usar `this.state` y `setState()` para administrar el estado del componente interno
  * utilizar formularios y eventos en React para agregar interacciones
  * flujo de datos unidireccional es un concepto importante en React
  * declarar componentes con hijos y componentes reutilizables
  * uso e implementación de componentes de clase ES6 y componentes funcionales sin estado
  * enfoques para diseñar sus componentes
* ES6
  * funciones de flecha con bloque y cuerpos concisos para acortar sus declaraciones de funciones
  * las funciones que están enlazadas a una clase son métodos de clase
  * desestructuración de objetos y matrices
  * parámetros predeterminados
* General
  * funciones de orden superior

Una vez más, tiene sentido tomar un descanso. Interiorizar lo aprendido y aplicarlo por ti mismo. Puedes experimentar con el código fuente que has escrito hasta ahora. También puedes conocer más en la  [documentación oficial](https://facebook.github.io/react/docs/installation.html).

El código fuente está disponible en el [repositorio oficial](https://github.com/rwieruch/hackernews-client/tree/2705dcd1a2027c4a6ecb8132428b399785afdfa5).
