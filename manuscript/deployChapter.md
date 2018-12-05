# Pasos Finales a Producción

Los últimos capítulos te mostrarán cómo hacer deploy de tu aplicación a producción.
Usarás el servicio de hospedaje gratuito Heroku.
En el camino a hacer deploy de tu aplicación aprenderás más acerca de *create-react-app*.

## Eject

El siguiente paso y conocimiento **no es necesario** para hacer deploy de tu aplicación aa producción. Sin embargo, te lo quiero explicar. *create-react-app* viene con una característica para prevenir la dependencia a un proveedor. Dependencia a un proveedor generalmente ocurre cuando has invertido en una tecnología pero no hay forma de escapar de ella. En una dependencia a proveedor es difícil cambiar la tecnología. En *create-react-app* tienes el escape con "eject".

En tu *package.json* encontrarás los scripts para *start*, *test* y *build* tu aplicación.
El último script es *eject*.
Lo puedes utilizar, pero no hay forma de volver.
**Es una operación en una sola dirección. Una vez que haces eject, ¡no hay vuelta atrás!**
Si acabas de comenzar a aprender React, no tiene sentido dejar el entorno conveniente de *create-react-app*.

Si corrieras el comando `npm run eject`, el comando copiaría toda la configuración y dependencias a tu *package.json* y un nuevo directorio *config/*. Convertirías todo el proyecto a una configuración personalizada con herramientas que incluyen Babel, Webpack, y ESLint. Después de todo, tendrías control sobre todas estas herramientas.

La documentación oficial dice que *create-react-app* es adecuado para proyectos de tamaño pequeño a mediano. No debes sentir la obligación de user el comando *eject*.

### Ejercicios:

* lee más acerca de [eject](https://github.com/facebookincubator/create-react-app#converting-to-a-custom-setup)

## Haz deploy de tu Aplicación

Al final ninguna aplicación se debe quedar en localhost. Tú quieres ir en vivo. Heroku es una plataforma como servicio donde puedes hospedar tu aplicación. Ellos ofrecen una integración perfecta con React. Para ser más específico: Es posible hacer un deploy de una aplicación create-react-app en minutos. Es un deploy de cero configuración por lo que sigue la filosofía de create-react-app.

Debes cumplir con dos requisitos and the que puedas hacer el deploy de tu aplicación a Heroku:

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

Eso es todo. Espero que tu aplicación esté ahora en funcionamiento. Si te encuentras con problemas puedes consultar los siguientes recursos:

* [Haciendo deploy de React con cero configuración](https://blog.heroku.com/deploying-react-with-zero-configuration)
* [Heroku Buildpack para create-react-app](https://github.com/mars/create-react-app-buildpack)
