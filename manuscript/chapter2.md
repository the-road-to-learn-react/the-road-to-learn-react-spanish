# Conceptos Básicos en React

Este capítulo te guiará a través de los aspectos básicos de React.  Conocerás lo que es el estado y las interacciones dentro de componentes. Además, verás distintas maneras de declarar un componente y cómo hacerlos reutilizables. Prepárate para darle vida propia a tus componentes React.

## Estado interno de Un Componente

El estado interno de un componente, también conocido como estado local, te permite almacenar, modificar y eliminar propiedades almacenadas dentro de un componente. El componente de clase ES6 puede utilizar un constructor para inicializar el estado interno del componente. El constructor se llama una sola vez al inicializar el componente.

A continuación, veámos el constructor de clase donde se puede establecer el estado interno inicial del componente.

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

El componente `App` es una subclase de `Component`, a esto se debe el `extends Component` en la declaración del componente `App`. En instantes conocerás más acerca de componentes de clase ES6, por ahora volvamos a `App`.

Es obligatorio llamar a `super(props);`, pues habilita `this.props` dentro de tu constructor, para que puedas acceder a él. De lo contrario, al intentar accesar a `this.props`dentro de tu constructor retornará `undefined`.

En tu caso, el estado inicial dentro del componente `App` debería ser la lista de elementos.

{title="src/App.js",lang=javascript}
~~~~~~~~
const list = [
  {
    title: 'React',
    url: 'https://reactjs.org/',
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

El estado está ligado a la clase por medio del objeto `this`, por tanto se puede acceder a este dentro de todo el componente. Por ejemplo, puedes usar el estado dentro del método `render()`. Anteriormente mapeaste una lista estática de elementos dentro del método `render()` que fue definida fuera del componente `App`. Ahora, accederás a la lista almacenada en el estado local dentro del componente.

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

Esta lista es ahora parte del componente, es decir se encuentra almacenada en el estado interno del componente.  Podrías fácilmente agregar artículos, cambiarlos o quitarlos de la lista. Cada vez que el estado del componente cambie, el método `render()` de tu componente se ejecutará de nuevo. Así es como puedes fácilmente cambiar el estado de un componente interno y asegurarte de que el componente se vuelva a renderizar y muestre la información correcta proveniente del estado local.

Pero ten cuidado. No cambies el estado directamente. Para modificarlo tienes que usar un método llamado `setState()`, que conocerás en un próximo capítulo.

### Ejercicios:

* experimenta con el estado interno
  * define más estados iniciales dentro del constructor
  * usa y accede al estado dentro de tu método `render()`
* lee más sobre [el constructor de clase ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)

## Inicializador de Objetos ES6 (ES6 Object Initializer)

En JavaScript ES6 puedes utilizar sintaxis abreviada para inicializar objetos de manera concisa. Imagina la siguiente inicialización de objeto:

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

En tu aplicación puedes hacer lo mismo. El nombre de la variable `list` y el nombre de la propiedad de estado comparten el mismo nombre.

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

Los nombres de método abreviados también te ayudarán mucho. En JavaScript ES6 puedes inicializar métodos dentro de un objeto de manera concisa también.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var userService = {
  getUserName: function(user) {
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

Y por último, pero no menos importante, en JavaScript ES6 es posible utilizar nombres de propiedad calculados (computed property names).

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

Probablemente los nombres de propiedad calculados suenen cómo algo extraño para ti en este momento. ¿Por qué los necesitarías? Más adelante responderemos esta interrogante, cuándo los utilices para insertar valores dentro de un objeto de manera dinámica.

### Ejercicios:

* experimenta con el inicializador de objetos ES6
* lee más sobre el [inicializador de objetos ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer)

## Flujo de Datos Unidireccional

Ahora ya hay un estado interno inicializado dentro del componente `App`. Sin embargo aún no manipulas dicho estado interno. Este estado es aún estático y por lo tanto también lo es el componente. Una buena manera de experimentar con la manipulación de estado es generando interacciones entre componentes.

Agreguemos un botón a cada elemento de la lista presentada a continuación. El botón tendrá el nombre "Dismiss" y permitirá remover al elemento de la lista que lo contiene. Este botón eventualmente será útil, por ejemplo, cuando sólo quieras mantener una lista de elementos no leídos y eliminar los que no te interesen.

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

El método de clase `onDismiss()` aún no está definido, lo definiremos en un momento. Por ahora enfócate en el selector `onClick` del elemento `button`.

Como puedes ver, el método `onDismiss()` de la función `onClick` está encerrado dentro de otra función. De esta manera, puedes ubicarte en la propiedad `objectID` perteneciente al objeto `item`, y así identificar el elemento que será eliminado al presionar el botón correspondiente. Una manera alternativa sería definiendo la función fuera de `onClick`, e incluir solamente la función definida dentro del selector. Más adelante explicaré el tema de los selectores de elementos con más detalle.

¿Notaste las multilíneas y el indentado en el elemento `button`? Elementos con múltiples atributos en una sola línea pueden ser difíciles de leer. Es por eso que para definir el elemento `button` y sus propiedades utilizo multilíneas e identado, manteniendo así todo legible. Esto no es obligatorio, pero si muy recomendable.

Ahora, tienes que implementar la función `onDismiss()`. Se necesita un `id` para identificar el elemento que se quiere eliminar. `onDismiss()` está vinculada a la clase `App` y por lo tanto se convierte en un método de clase, por esta razón se debe accesar a él con `this.onDismiss()` y no simplemente `onDismiss()`. El objeto `this` representa la relación de la clase. Ahora, para definir `onDismiss()` cómo método de clase necesitas enlazarlo con el constructor.


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

Para el siguiente paso tienes que definir la funcionalidad lógica de `onDismiss()` dentro de la clase. Los métodos de clase pueden ser definidos como se muestra a continuación.

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

Ahora define lo que sucede dentro del método de clase. Básicamente, quieres eliminar de la lista un artículo identificado por medio de un `id` y actualizar la lista en el estado local del componente. Al final, la lista actualizada será usada dentro del método `render()` y se mostrará en pantalla sin el elemento que recien eliminaste.

 Puedes remover un elemento de una lista utilizando el método incorporado de JavaScript `filter()`. Este método crea una lista que contiene todos los elementos de la lista original que pasan la prueba establecida.
 
El método `filter()` recibe como parámetro otra función e itera sobre los elementos dentro de una lista dada. Si la evaluación de un ítem resulta en verdadero, el elemento se queda en la lista, de lo contrario dicho elemento se elimina. Al finalizar devuelve una nueva lista con los resultados, sin alterar la lista original. `filter()` es compatible con la convención establecida para React que promueve las estructuras de datos inmutables.


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

En el siguiente paso, toma la función `isNotId()` que acabas de declarar y pasarla al método `filter()` como se muestra a continuación.

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

Es posible definir la función `isNotId()` de manera más concisa utilizando una función flecha ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

Podrías incluso hacerlo todo en una sola línea, como hiciste con el selector `onClick()` del botón, aunque puede resultar difícil de leer.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(item => item.objectID !== id);
# leanpub-end-insert
}
~~~~~~~~

Ahora es posible eliminar de la lista al elemento cliqueado. Sin embargo, el estado aún no se actualiza. Para actualizar el estado interno del componente puedes utilizar el método de clase `setState()`.

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

Ahora ejecuta nuevamente la aplicación y prueba el botón "Dismiss". Debe funcionar correctamente. Esto que acabas de experimentar se conoce como **Flujo de Datos Unidireccional**. Ejecutas una acción en la capa de vistas con `onClick()`, luego una función o método de clase modifica el estado interno del componente y acto seguido el respectivo método `render()` es ejecutado para actualizar la capa de vistas.

### Ejercicios:

* lee más sobre [el estado y los ciclos de vida en componentes React](https://facebook.github.io/react/docs/state-and-lifecycle.html)

## Enlaces (Bindings)
Dentro del siguiente componente se muestra un método de clase enlazado correctamente en el constructor de clase.

Es importante que conozcas los Enlaces que tienen lugar dentro de las clases JavaScript ES6 al momento de utilizar componentes de clase React ES6. En el capítulo anterior enlazaste el método de clase `onDismiss()` al constructor de la clase `App`.

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

  ...
}
~~~~~~~~

¿Por qué es necesario hacer esto? Enlazar es necesario, porque los métodos de clase no enlazan `this` de manera automática a la instancia de la clase. Veamos esta idea en acción con la ayuda del siguiente componente de clase ES6.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

El componente se renderiza sin problema, pero cuando presiones el botón, recibirás el mensaje `undefined` en la consola de desarrollador. Esta es una de las principales fuentes de bugs en React, pues, si quieres acceder a `this.state` desde un método de clase, esto no será posible porque `this` es `undefined` por defecto. Para poder acceder a `this` desde tus métodos de clase tienes que enlazar `this`  a los métodos de clase.

En el siguiente componente, dentro del constructor de clase se puede ver que el método de clase `onClickMe` se encuentra correctamente asociado al objeto `this`.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
# leanpub-start-insert
  constructor() {
    super();

    this.onClickMe = this.onClickMe.bind(this);
  }
# leanpub-end-insert

  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Al cliquear nuevamente el botón, el objeto `this`, más específicamente la instancia de clase, debería estar definido y podrás acceder a `this.state`.

Enlazar métodos de clase puede hacerse desde cualquier otra parte también. Como por ejemplo, dentro del método de clase `render()`.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
# leanpub-start-insert
        onClick={this.onClickMe.bind(this)}
# leanpub-end-insert
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Pero deberías evitarlo porque el método de clase sería enlazado cada vez que se ejecute `render()`. Básicamente, el enlace se ejecutará cada vez que el componente se actualize, lo que compromete el rendimiento. Al momento de enlazar un método de clase al constructor debes enlazarlo al principio, solo una vez cuando el componente es instanciado. Es una mejor manera de hacerlo.

Otra cosa que algunos hacen de vez en cuando es: Definir la lógica de negocios de sus métodos de clase dentro del constructor.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

# leanpub-start-insert
    this.onClickMe = () => {
      console.log(this);
    }
# leanpub-end-insert
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Debes evitarlo también. Con el tiempo esto desordenará el constructor. El constructor solo está allí para que sea posible instanciar la clase y todas sus propiedades. Es por eso que la lógica de negocio de los métodos de clase debe ser definida fuera del constructor.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

    this.doSomething = this.doSomething.bind(this);
    this.doSomethingElse = this.doSomethingElse.bind(this);
  }

  doSomething() {
    // do something
  }

  doSomethingElse() {
    // do something else
  }

  ...
}
~~~~~~~~

Por último pero no menos importante, vale la pena mencionar que los métodos de clase pueden auto-enlazarse utilizando funciones flecha de ES6.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe = () => {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Si realizar enlazes dentro de los constructores repetidas veces  te resulta molesto, puedes hacerlo de la manera antes mencionada. La documentación oficial de React sugiere que se enlacen los métodos de clase dentro del constructor, por eso el libro adoptará este enfoque también.

### Ejercicios:

* Prueba los diferentes tipos de enlace mencionados anteriormente y logea en la consola de desarrollador (console.log) el objeto `this`

## Manejador de Eventos (Event Handler)

En esta sección adquirirás un mayor entendimiento acerca de los manejadores de eventos en elementos. En tu aplicación. Dentro de tu aplicación, estas utilizando el siguiente elemento `button` para eliminar un elemento de la lista.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Esto ya representa un caso de uso complejo porque tienes que pasar un valor al método de clase y por lo tanto, debes colocarlo dentro de otra función flecha. Básicamente, lo que debe ser pasado al manejador de evento es una función. El siguiente código no funcionaría, el método de clase sería ejecutado inmediatamente al abrir la aplicación dentro del navegador.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Al usar `onClick={doSomething()}`, la función `doSomething()` se ejecutaría de inmediato al momento de abrir la aplicación en el navegador. La expresión pasada al manejador es evaluada. Y cómo el valor retornado por la función no es una función, nada pasaría al presionar el botón. Pero, al utilizar `onClick={doSomething}` donde `doSomething` es una función, esta sería ejecutada al momento de presionar el botón. Y la misma regla aplica para el método de clase `onDismiss()` que es usado en tu aplicación.

Sin embargo, utilizar `onclick={this.onDismiss}` no sería suficiente, porque de alguna manera la propiedad `item.objectID` debe ser pasada al método de clase para identificar el elemento que va a ser eliminado. Por eso puede ser colocado cómo parámetro dentro de otra función y acceder a la propiedad. A este concepto se le conoce cómo función de orden superior en JavaScript y será explicado más adelante.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Una alternativa sería definir la función externa en otra parte y solo pasar la función definida ya definida al manejador, o mejor dicho, solo invocar la función desde dicho manejador.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item => {
# leanpub-start-insert
          const onHandleDismiss = () =>
            this.onDismiss(item.objectID);
# leanpub-end-insert

          return (
            <div key={item.objectID}>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
              <span>
                <button
# leanpub-start-insert
                  onClick={onHandleDismiss}
# leanpub-end-insert
                  type="button"
                >
                  Dismiss
                </button>
              </span>
            </div>
          );
        }
        )}
      </div>
    );
  }
}
~~~~~~~~

Después de todo, debe ser una función lo que es pasado al manejador del elemento. Cómo ejemplo, prueba este código:

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
            ...
            <span>
              <button
# leanpub-start-insert
                onClick={console.log(item.objectID)}
# leanpub-end-insert
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

Este correrá al abrir la aplicación en el navegador, pero no cuando presiones el botón. Mientras que la siguiente pieza de código solo correrá cuando presiones el botón. Es decir, la función será ejecutada cuando acciones el manejador.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={function () {
    console.log(item.objectID)
  }}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

De nuevo, para mantenerlo conciso, puedes transformarlo en una función flecha JavaScript ES6. Similar a lo que hicimos con el método de clase `ondismiss()`.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={() => console.log(item.objectID)}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Es común que sea complicado para algunos principiantes utilizar funciones dentro de un manejador de eventos. Por eso, intento explicarlo en mayor detalle aquí. Al final deberías tener el siguiente código dentro del elemento `button` para tener una concisa función flecha de una sola línea, que además puede acceder a la propiedad `objectID` del objeto `item`.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            ...
            <span>
# leanpub-start-insert
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Otro tema relevante en cuánto a rendimiento es la implicación de utilizar funciones flecha dentro de manejadores de eventos es, por ejemplo, el manejador `onClick` envuelve al método `onDismiss()` dentro de una función flecha para así poder captar el identificador del elemento. Así cada vez que el método `render()` se ejecuta, el manejador instancia una función flecha de orden superior. Esto puede impactar de cierta manera el rendimiento de tu aplicación, pero en la mayoría de los casos no se notará. Imagina que tienes una gran tabla de datos con 1000 elementos y cada fila o columna posee dicha función flecha dentro de su manejador de evento, en este caso vale la pena pensar en las implicaciones de rendimiento y por lo tanto podrías implementar un componente Botón dedicado a enlazar el método dentro del constructor. Pero antes de que eso suceda es una optimización prematura. Por ahora basta con que te enfoques meramente en aprender React.

### Ejercicios:

* Experimenta con diferentes formas de utilizar funciones dentro del manejador `onClick` de tu botón

## Interacciones con Formularios y Eventos

Ahora verás otra interacción relacionada con formularios y eventos en React. Dicha interacción es una funcionalidad de búsqueda. La entrada del campo de búsqueda se debe utilizar para filtrar la lista basada en la propiedad de título de un elemento.

Primero, define el campo de entrada en tu JSX.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        <form>
          <input type="text" />
        </form>
# leanpub-end-insert
        {this.state.list.map(item =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

En el escenario siguiente, escribirás dentro del campo establecido y se filtrará la lista temporalmente de acuerdo al término de búsqueda especificado. Para poder filtrar la lista necesitas el valor del campo de entrada para actualizar el estado. Pero, ¿cómo acceder a tal valor? En React puedes utilizar los **eventos sintéticos (synthetic events)**  para porder acceder al valor que necesitas de este evento.

Vamos a definir un manejador `onChange()` para el campo de entrada.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
# leanpub-start-insert
          <input
            type="text"
            onChange={this.onSearchChange}
          />
# leanpub-end-insert
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

La función está vinculada al componente y, por tanto se le considera un método de clase. Todavía tienes que vincularlo y definirlo.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onSearchChange() {
    ...
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

Al usar un manejador dentro de tu elemento, obtienes acceso al evento sintetico de React dentro de la función callback.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  onSearchChange(event) {
# leanpub-end-insert
    ...
  }

  ...
}
~~~~~~~~

El evento tiene el valor del campo de entrada en su objeto de destino. Por lo que es posible actualizar el estado local con el término de búsqueda utilizando `this.setState()` nuevamente.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  onSearchChange(event) {
# leanpub-start-insert
    this.setState({ searchTerm: event.target.value });
# leanpub-end-insert
  }

  ...
}
~~~~~~~~

Adicionalmente, hay que definir el estado inicial para la propiedad `searchTerm` dentro del constructor de la clase. El campo de entrada estará vacío al principio y por lo tanto el valor de `searchTerm` debe ser una cadena de texto vacía (empty string).

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
# leanpub-start-insert
      searchTerm: '',
# leanpub-end-insert
    };

    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

El valor de entrada es almacenado dentro del estado de componente interno cada vez que el valor en el campo de entrada cambia.

Una nota observación a la hora de actualizar el estado local de un componente React. Es justo asumir que al actualizar `searchTerm` con `this.setState()` la lista debe ser pasada también, con el fin de preservarla, pero no es el caso. `this.setState()` preserva las propiedades de su hermano dentro del estado del objeto al momento de actualizar una propiedad específica en él.

De vuelta a la aplicación, la lista aún no está filtrada en base a la información del campo de entrada que se almacena en el estado local del componente. Básicamente, se busca filtrar la lista de manera temporal en base al elemento `searchTerm`. Ya tienes todo lo necesario para hacerlo, entonces ¿cómo filtrar la lista de manera temporal? Es posible aplicar un filtro dentro del método `render()`. Este filtro solo sería si `searchTerm` coincide con el título de propiedad del elemento. Ya utilizaste anteriormente el método `filter` de JavaScript, aquí lo utilizarás nuevamente. Es posible agregar en primer lugar la función `filter` antes que `map`, porque `filter` retorna un nuevo array, por lo que la función `map` resulta ser muy conveniente en esta ocasión.

{title="src/App.js",lang=javascript}
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
# leanpub-start-insert
        {this.state.list.filter(...).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Ahora analizemos la función `filter` desde otro punto de vista. Queremos definir el argumento de `filter` (la función que es pasada cómo parámetro a `filter`) fuera de nuestro componente de clase ES6. Desde allí no se tiene acceso al estado del componente y por lo tanto no tenemos acceso a la propiedad `searchTerm` para evaluar la condición del filtro. Tenemos que pasar `searchTerm` a la función de filtro y esta tiene que devolver una nueva función para evaluar la condición. A esta nueva función se le conoce cómo función de orden superior (higher-order function).

Normalmente no mencionaría las funciones de orden superior, pero en un libro que habla acerca de React, esto es importante. Es necesario conocer sobre las funciones de orden superior porque React trata con un concepto llamado componentes de orden superior. Este concepto se explora más adelante en el libro. Por ahora, volvamos a enfocarnos en la función `filter` y su funcionalidad.

Primero, tienes que definir la función de orden superior fuera del componente `App`.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function isSearched(searchTerm) {
  return function (item) {
    // some condition which returns true or false
  }
}
# leanpub-end-insert

class App extends Component {

  ...

}
~~~~~~~~

La función `isSearched()` toma a `searchTerm` como parámetro y devuelve otra función. La función devuelta tiene acceso al elemento del objeto porque es la función que es pasada cómo parámetro a la función `filter`. Adicionalmente, la función devuelta se utilizará para filtrar la lista en base a la condición definida dentro de la función. Vamos a definir la condición.
{title="src/App.js",lang=javascript}
~~~~~~~~
function isSearched(searchTerm) {
  return function (item) {
# leanpub-start-insert
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
# leanpub-end-insert
  }
}

class App extends Component {

  ...

}
~~~~~~~~
La condición establece que el patrón entrante de `searchTerm` coincide con el título de la propiedad perteneciente al elemento de la lista. Esto se puede lograr con la funcionalidad `includes` de JavaScript. Solo cuáundo el patrón coincide, se retorna verdadero y el ítem permanece dentro de la lista. Cuando el patrón no coincide el ítem es removido de la lista. Pero ten cuidado cuándo los patrones coinciden: No debes olvidar convertir a minúsculas ambas cadenas de texto. De otro modo habrán inconsistencias entre un término de busqueda 'redux' y un ítem con el título 'Redux'. Ya que estamos trabajando con una lista inmutable que retorna una nueva lista al usar la función `filter()`, la lista original almacenada en el estado local no es modificada en lo absoluto.

Resta algo por mencionar: Hicimos un poco de trampa utilizando la funcionalidad `includes `de JavaScript. Es una característica propia de ES6. ¿Cómo se podría sustituir en JavaScript ES5? Podrías utilizar la funcion `indexOf()` para obtener el índice del elemento en la lista, cuando el elemento se encuentre en la lista `indexOf()` retornará su índice en el array.

~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

Otra refactorización inteligente puede ser lograda utilizando de nuevo una función flecha ES6. Hace que la función sea más concisa:

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

Puede ponerse en duda cuál de las funciónes es más legible. Personalmente prefiero la segunda. El ecosistema de React utiliza una gran cantidad de conceptos de programación funcional. Sucede a menudo que puedes utilizar una función que devuelve otra función (funciones de orden superior). En ES6 estas se pueden expresar de forma más concisa utilizando funciones flecha de ES6.

Por último pero no menos importante, tienes que utilizar la funcion definida `isSearched()` para filtrar tu lista. `isSearched` recibe cómo parámetro la propiedad `searchTerm` directamente del estado local, retorna la entrada de la función `filter()` y filtra la lista de acuerdo a la condición del filtro. Después mapea la lista filtrada para mostrar en pantalla un elemento correpondiente a cada ítem.

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

Ahora la funcionalidad de búsqueda debería trabajar correctamente. Pruébala.

### Ejercicios:

* lee más sobre [eventos React](https://facebook.github.io/react/docs/handling-events.html)
* lee más sobre [Funciones de Orden Superior](https://en.wikipedia.org/wiki/Higher-order_function)

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

* leer más sobre [ES6 destructuring](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## Componentes Controlados

Ya has aprendido sobre el flujo de datos unidireccional en React. La misma ley se aplica al campo de entrada, que actualiza el estado que a su vez filtra la lista. El estado fue cambiado, el método `render()` e ejecuta de nuevo y utiliza el estado reciente `searchTerm` para aplicar la condición de filtro.

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

* leer más sobre [React forms](https://facebook.github.io/react/docs/forms.html)

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

* leer más sobre [the composition model of React](https://facebook.github.io/react/docs/composition-vs-inheritance.html)

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

* leer más sobre [ES6 default parameters](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)

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
* leer más sobre [ES6 class components and functional stateless components](https://facebook.github.io/react/docs/components-and-props.html)

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
