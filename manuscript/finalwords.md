# Esbozo

Llegamos al final de The Road to learn React. Espero que te haya ayudado ganar tracción con React. Si te gusto el libro compártelo con tus amigos como una manera de aprender React. Deberia de ser utilizado como un regalo. Además, una reseña del libro en [Amazon](https://www.amazon.com/dp/B077HJFCQX) o [Goodreads](https://www.goodreads.com/book/show/33541539-the-road-to-learn-react) puede ayudar con proyectos futuros.

**¿A donde ir ahora?** Puedes extender la aplicación por tu cuenta o sumergirte en tu propio proyecto React. Antes de que te sumerjas en otro libro, curso o tutorial, deberías de crear un proyecto React con tus propias manos. Hazlo por una semana, llevalo a producción como se demostró en el ultimo capitulo, y contáctame en [Twitter](https://twitter.com/rwieruch). Tengo curiosidad de que construirás después de haber leído el libro. Tambien puedes encontrarme en [GitHub](https://github.com/rwieruch) para compartir tu repositorio.

Si buscas expandir aún más tu aplicación, puedo recomendar varios caminos de aprendizaje:

* **Conectar la aplicación a una Base de Datos y/o Autenticación:** En una aplicación de React, quizá necesites implementar persistencia de datos. Los datos deben ser almacenados en una base de datos para que sobrevivan hasta luego del cierre de sesión del navegador y puedan ser compartidos entre diferentes usuarios utilizando distinas aplicaciones. La mejor manera de lograr esto es con Firebase. En [este tutorial](https://www.robinwieruch.de/complete-firebase-authentication-react-tutorial/), encontrarás una guía paso-a-paso sobre como utilizar Firebase. Puedes utilizar Firebase para almacenar entidades de usuarios en tiempo real.

* **Gestión de Estado:** Has utilizado `this.setState()` para manejar el estado interno de los componentes. Es un comienzo perfecto. Sin embargo, en una aplicación en crecimiento experimentaras los limites del estado interno de los componentes. Por lo tanto tienes a tu alcance librerias de manejo de estado de terceros  como [Redux o MobX](https://www.robinwieruch.de/redux-mobx-confusion/). Mi [siguiente libro](https://gumroad.com/products/uwiyI) te servira de guía en este tema.

* **Mecanización con WebPack y Babel:** En el libro utilizaste *create-react-app* para crear tu propia aplicación. En algún punto cuando hayas aprendido React, te convendría aprender la mecanización que lo rodea. Te permite preparar tus propios proyectos sin *create-react-app*. Puedo recomendar seguir una preparación mínima con [Webpack y Babel](https://www.robinwieruch.de/minimal-react-webpack-babel-setup/). Después podrías [utilizar ESLint](https://www.robinwieruch.de/react-eslint-webpack-babel/) para seguir un estilo unificado de código en tu aplicación.

* **Organización de Código:** En tu camino leyendo el libro te encontraste con un cápitulo acerca de la Organización de Código. Puedes aplicar esos cambios ahora, si todavía no lo has hecho. Organizara tus componentes en archivos y carpetas estructuradas (módulos). Además, te ayudara a comprender y aprender los principios de división de código, reusabilidad, mantenibilidad y diseño de módulos API.

* **Pruebas:** El libro solo araño la superficie de las pruebas. Si no eres familiar con el tema en general, puedes explorar en profundidad los conceptos de pruebas unitarias y de integración, especialmente en el contexto de aplicaciones React. En el nivel de implementación, recomiendo quedarse con Enzyme y Jest con el fin de refinar tu enfoque a las pruebas en React.

* **Sitaxis de Componentes React:** Las mejores prácticas para implementar componentes React están en constante evolución. En otros recursos ncontrarás muchas maneras de escribir tus propios componente, especialmente componentes de clase React. El repositorio de GitHub llamado [react-alternative-class-component-syntax](https://github.com/the-road-to-learn-react/react-alternative-class-component-syntax) es un excelente recurso para conocer opciones alternativas para escribir componentes de clase React.

* **Componentes UI:** Muchos principiantes cometen el error de introducir librerías de componentes UI muy pronto en sus proyectos. Es mucho más práctico aprender a implementar y usar desplegables, checkboxes o diálogos en React por medio de elementos HTML estandar. La mayoría de estos componentes gestionan su propio estado a nivel local. Un checkbox necesita saber si esta chequeado o no, así que es necesario implementarlos como componentes controlados. Luego de haber conocido las implementaciones básicas, agregar una librería de componentes UI con checkboxes y diálogos como componente React debería resultar sencillo.

* **Enrutamiento:** Puedes aplicar enrutamiento en tu aplicación con [react-router](https://github.com/ReactTraining/react-router). Hasta ahora solo tienes una página en tu aplicación. React Router te ayuda a tener múltiples páginas a tráves de distintos URLs.

* **Tipado:** En un capitulo utilizo React PropTypes para definir las interfaces de los componentes. Es una buena práctica realizada para prevenir errores. Pero los PropTypes solo son comprobados durante la ejecución. Puedes ir un paso más allá para introducir tipado estático el cual comprueba durante la compilación. [TypeScript](https://www.typescriptlang.org/) es un enfoque popular. Pero en el ecosistema de React se usa frecuentemente [Flow](https://flowtype.org/). Yo recomiendo darle una oportunidad a Flow.

* **React Native:** [React Native](https://facebook.github.io/react-native/) trae tu aplicación a dispositivos móviles. Puedes aplicar tus conocimientos de React para enviar aplicaciones iOS y Android. La curva de aprendizaje, una vez hayas aprendido React, no debería de ser muy inclinada en React Native. Ambos comparten los mismos principios. Solo encontraras una disposición diferente de componentes en dispositivos móviles del que estás acostumbrado en aplicaciones web.

* **Otros Proyectos:** Hay muchos tutoriales que se enfocan solo en React para construir aplicaciones interesantes y que sirven de buena práctica para que apliques lo que aprendiste en este libro antes de que te muevas a proyectos intermedios. Aquí hay algunos de mi autoría:
  * [A paginated and infinite scrolling list](https://www.robinwieruch.de/react-paginated-list/)
  * [Showcasing tweets on a Twitter wall](https:/www.robinwieruch.de/react-svg-patterns/)
  * [Connecting your React application to Stripe for charging money](https://www.robinwieruch.de/react-express-stripe-payment/).

Te invito a visitar [mi sitio web](https://www.robinwieruch.de/) para encontrar más temas interesantes sobre  desarrollo web, además de temas generales en ingeniería del software. Puedes [suscribirte](https://www.getrevue.co/profile/rwieruch) para recibir nuevos artículos, libros y cursos. También puedes visitar la plataforma de [Road to React](https://roadtoreact.com) donde ofrezco lecciones más avanzadas sobre el ecosistema React.

Finalmente, espero encontrar más [Patrons](https://www.patreon.com/rwieruch) dispuestos a patrocinar mi contenido, hay muchos estudiantes que no pueden pagar por contenido educacional. Por eso, publico gan parte de mi contenido gratuitamente. Patrocinarme en Patreon permíte que otros tengan acceso a este contenido de forma gratuita.

Una vez más, si te gustó el libro, quiero que te tomes un momento de pensar en una persona interesada en React en aprender React. Acercate a esa persona y comparte el libro. Esto significaría mucho para mí. El propósito de este libro es el ser dado a otros. Mejorará con el tiempo, cuando más personas lo utilizen y compartan sus comentarios.

Muchas gracias por leer The Road to learn React.

Saludos,

Robin Wieruch


