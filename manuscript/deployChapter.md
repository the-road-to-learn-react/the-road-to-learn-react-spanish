# Pasos Finales hacia Producción

Los últimos capítulos le mostrarán cómo llevar su aplicación a un estado de producción. Usará el servicio de hospedaje gratuito Heroku.
Mientras lleva su aplicación a producción, aprenderá un poco más acerca de *create-react-app*.

## Eject

El conocimiento que se presenta a continuación, **no es necesario** para llevar tu aplicación a producción. Sin embargo, vale la pena mencionarlo. *create-react-app* incluye una característica para prevenir la dependencia a un proveedor. La dependencia a un proveedor generalmente ocurre cuando ha invertido en una tecnología y no hay forma de escapar de ésta. En un caso de dependencia a proveedor es difícil cambiar la tecnología. Afortunadamente, en *create-react-app* tiene una ruta de escape con "eject".

En tu *package.json* encontrará los scripts para *start*, *test* y *build* de tu aplicación.
El último script es *eject*.
Lo puedes utilizar, *pero no hay forma de volver*. **Es una operación en una sola dirección. Una vez que haces eject, ¡no hay vuelta atrás!**

Si acaba de comenzar a aprender React, no tiene sentido dejar el entorno conveniente de *create-react-app*.

Si se siente lo suficientemente seguro como para ejecutar `npm run eject`, el comando copiará toda la configuración y dependencias a tu *package.json* y en un nuevo directorio *config/*. Aplicará a todo el proyecto una configuración personalizada con herramientas que incluyen Babel, Webpack y ESLint. Después de todo, tendrá el control sobre todas estas herramientas.

La documentación oficial dice que *create-react-app* es adecuada para proyectos de tamaño pequeño a mediano. No debe sentir la obligación de user el comando *eject* a menos que se sienta preparado.

### Ejercicios:

* lee más acerca de [eject](https://github.com/facebookincubator/create-react-app#converting-to-a-custom-setup)

## Despliega tu Aplicación

Al final, ninguna aplicación se debe quedar en localhost. Querrá ponerla en línea. Heroku es una plataforma donde puede hospedar tu aplicación. Ellos ofrecen una integración perfecta con React. Para ser más específico: es posible desplegar una aplicación create-react-app en minutos. Es un despliegue que requiere cero configuración, por lo que sigue la filosofía de create-react-app.

Debe cumplir con dos requisitos antes de que sea posible desplegar tu aplicación en Heroku:

* Instalar el [Heroku CLI](https://devcenter.heroku.com/articles/heroku-command-line)
* Crear una [cuenta gratuita en Heroku](https://www.heroku.com/)

Si ha instalado Homebrew, puede instalar el Heroku CLI desde la línea de comando:

{title="Command Line",lang="text"}
~~~~~~~~
brew update
brew install heroku-toolbelt
~~~~~~~~

Ahora puede usar git y el Heroku CLI para hacer deploy de su aplicación.

{title="Command Line",lang="text"}
~~~~~~~~
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
~~~~~~~~

Eso es todo. Espero que tu aplicación esté ahora en funcionamiento. Si tiene problemas puede consultar los siguientes recursos:

* [Desplegando React con cero configuración](https://blog.heroku.com/deploying-react-with-zero-configuration)
* [Heroku Buildpack para create-react-app](https://github.com/mars/create-react-app-buildpack)
