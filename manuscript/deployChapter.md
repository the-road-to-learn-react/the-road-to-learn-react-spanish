# Pasos Finales hacia Producción

Los últimos capítulos te mostrarán cómo llevar tu aplicación a un estado de producción. Usarás el servicio de hospedaje gratuito Heroku.
Mientras llevas tu aplicación a producción, aprenderás un poco más acerca de *create-react-app*.

## Eject

El conocimiento que se presenta a continuación, **no es necesario** para llevar tu aplicación a producción. Sin embargo, vale la pena mencionarlo. *create-react-app* incluye una característica para prevenir la dependencia a un proveedor. La dependencia a un proveedor generalmente ocurre cuando has invertido en una tecnología y no hay forma de escapar de esta. En un caso de dependencia a proveedor es difícil cambiar la tecnología. Afortunadamente, en *create-react-app* tienes una ruta de escape con "eject".

En tu *package.json* encontrarás los scripts para *start*, *test* y *build* tu aplicación.
El último script es *eject*.
Lo puedes utilizar, *pero no hay forma de volver. **Es una operación en una sola dirección. Una vez que haces eject, ¡no hay vuelta atrás!**

Si acabas de comenzar a aprender React, no tiene sentido dejar el entorno conveniente de *create-react-app*.

Si te sientes lo suficientemente seguro como para ejecutar `npm run eject`, el comando copiará toda la configuración y dependencias a tu *package.json* y en un nuevo directorio *config/*. Aplicarás a todo el proyecto una configuración personalizada con herramientas que incluyen Babel, Webpack, y ESLint. Después de todo, tendrás control sobre todas estas herramientas.

La documentación oficial dice que *create-react-app* es adecuado para proyectos de tamaño pequeño a mediano. No debes sentir la obligación de user el comando *eject* a menos que te sientas preparado.

### Ejercicios:

* lee más acerca de [eject](https://github.com/facebookincubator/create-react-app#converting-to-a-custom-setup)

## Despliega tu Aplicación

Al final, ninguna aplicación se debe quedar en localhost. Querrás ponerla en línea. Heroku es una plataforma donde puedes hospedar tu aplicación. Ellos ofrecen una integración perfecta con React. Para ser más específico: Es posible desplegar una aplicación create-react-app en minutos. Es un despliegue que requiere cero configuración, por lo que sigue la filosofía de create-react-app.

Debes cumplir con dos requisitos antes de que sea posible desplegar tu aplicación en Heroku:

* Instalar el [Heroku CLI](https://devcenter.heroku.com/articles/heroku-command-line)
* Crear una [cuenta gratuita en Heroku](https://www.heroku.com/)

Si has instalado Homebrew, puedes instalar el Heroku CLI desde la línea de comando:

{title="Command Line",lang="text"}
~~~~~~~~
brew update
brew install heroku-toolbelt
~~~~~~~~

Ahora puedes usar git y el Heroku CLI para hacer deploy de tu aplicación.

{title="Command Line",lang="text"}
~~~~~~~~
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
~~~~~~~~

Eso es todo. Espero que tu aplicación esté ahora en funcionamiento. Si consigues problemas puedes consultar los siguientes recursos:

* [Desplegando React con cero configuración](https://blog.heroku.com/deploying-react-with-zero-configuration)
* [Heroku Buildpack para create-react-app](https://github.com/mars/create-react-app-buildpack)
