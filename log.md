##15-06-2015

He instalado un servidor Django de pruebas. Usa bootstrap y angularjs. Se encuentra todo en djangoServerTemplate/

[img1] 

He instalado la biblioteca de Adrián y he estado jugando con ella creando gráficos. 

```
Cd threesome/
vio@lenovo:~$ python -m SimpleHTTPServer
localhost:8000
```

para cargar la versión adecuada de node.js (si no no funciona kibana ni elasticSearch)

```
source ../nvm/nvm.sh
bin/elasticsearch
bin/kibana
```

He estado instalando kibana 5 development y he mirado documentación y videos sobre cómo hacer plugins. Este es de los mejores tutoriales que he encontrado:

https://www.timroes.de/2015/12/02/writing-kibana-4-plugins-basics/

Todavía no he conseguido hacer un plugin “hola mundo”, pero estoy a punto.

He entendido la estructura de crossfilter. Adrián la usa en su código para pintar las gráficas. Mi misión al final es sustituir crossfilter por elasticSearch. Para ello tengo que colaborar mucho con Adrián cambiando o aislando crossfilter de las partes donde pinta, para luego hacerlo con elasticSearch.


##20-06-2015

He estado mirando más documentación de Kibana
Al hacer git pull, se ha roto todo y kibana ya no funciona al ejecutarlo. Era cuestión de actualizar node.js:

```
source ../nvm/nvm.sh
nvm install "$(cat .node-version)"
npm install
```

He instalado el plugin timelion

```
vio@lenovo:~/kibanadev$ 
./bin/kibana-plugin install https://download.elastic.co/kibana/timelion/timelion-5.0.0-alpha3.zip
```

##21-06-2016

He conseguido crear mi primer plugin – el del reloj. No parece que haga nada pero al menos sale en visualizaciones.

Estoy peleando con kibana –dev que parece que no funciona

He hablado con Jesús y me ha dicho que reporte el error, si es que es un error. Pero más importante, me ha recomendado crear un .md en github con notas de lo que hago.

.MD Creado y solucionado el “bug”. Resulta que tenía que cambiar una variable solamente en mi sistema (ver github/Notes)


##27-06-2016

Objetivos:

* Actualizar proyecto Adrián - hecho
* Poner los dos paneles de Adrián en un div en mi servidor Django para ver qué tal se comporta - hecho
* Ordenar y arreglar mi cuenta de Github - hecho
* Hacer un plugin con una consulta sencilla a elasticSearch

He intentado poner los dos paneles 3D en Kibana sin éxito. He tenido muchos líos de módulos javascript. (THREE not defined por ejemplo).


He hablado con Jesús. Dos líneas de avance:
1  Separar crossfilter de la biblioteca gráfica de Adrián. Meter elasticSearch.
2  Un plugin Kibana que haga lo siguiente: coger las líneas de una base de datos de commits y mostrarlo en una tabla html en kibana. Además, permitir aplicar filtros (los filtros de Kibana). Si lo hago bien, debería funcionar automáticamente. Ojo: esto no usa el tratamiento de datos de Adrián.

##01-07-2016

* Hacer algún tutorial completo de Kibana. Parece que no tengo todas las nociones claras.
  Sigo el tutorial oficial de Kibana https://www.elastic.co/guide/en/kibana/current/getting-started.html - hecho
* Hacer plugin: una tabla html con los datos de commits - uff.. necesito ver cómo sacar los datos de elasticSearch a través de Kibana.

No entiendo cómo debería introducir los datos de commits en elasticSearch. No tengo los datos en un formato JSON de elasticSearch.

Ya lo tengo. Jesús me ha pasado un dump que cargo en mi elasticSearch con https://github.com/taskrabbit/elasticsearch-dump
Dejo el dump en Docs/

##07-07-2016

Necesito entender más la estructura de los plugins de Kibana.

* Hacer algún tutorial sobre node.js, npm modules, nvm...
https://www.sitepoint.com/beginners-guide-node-package-manager/
* Hacer tutorial de requireJS - hecho
* Hacer tutorial de Tim Roes donde viene cómo hacer consultar a elasticSearch desde Kibana.
https://www.timroes.de/2016/02/21/writing-kibana-plugins-custom-applications/ - hecho
* Repasar javascript. En este manual viene también muy bien explicado node.js http://eloquentjavascript.net/

##13-07-2016

* He conseguido hacer consultas a elasticSearch a través de Kibana. El tutorial de Tim Roes ya lo hace, pero he añadido una nueva función en la api de Kibana, /ncommits, un nuevo controlador angular y una nueva plantilla html para hacer una consulta a elasticSearch preguntando por el número de commits del índice commits_index.

* He conseguido meter en el plugin hola mundo con el reloj una escena básica de Adrián.

##22-08-2016

* eloquent javascript: repaso javascript, módulos con require (CommonJS), nodejs - repasado
http://eloquentjavascript.net/20_node.html

* Hacer tutorial de requireJS - hecho
https://www.sitepoint.com/understanding-requirejs-for-effective-javascript-module-loading/

##23-08-2016

* Hacer tutorial de angularJS - hecho un repaso general de AngularJS 1. 
* Pensar cómo estructurar la biblioteca 3D de Adrián en módulos (require, export) para usarlos simplemente después.

Solución 1: hacer que todos los módulos sean módulos RequireJS. 

1 Todos aquellos módulos o librerías externas serán cargadas como módulos de nodejs (d3.js, three, crossfilter).

```
npm install --save d3
npm install --save three
npm install --save crossfilter
```
2 Estructurar código de Adrián en módulos RequireJS, esto es, todo lo que está en js/. Para ello hay que establecer dependencias, qué fichero usa qué fichero, para luego definir módulos sencillos al estilo:

```
define(function(products) {
  return {
    reserveProduct: function() {
      console.log("Function : reserveProduct");

      return true;
    }
  }
  });
  ```
  
Y si el módulo depende de otros, entonces al estilo:

```
define(["credits","products"], function(credits,products) {

  console.log("Function : purchaseProduct");

  return {
    purchaseProduct: function() {

      var credit = credits.getCredits();
      if(credit > 0){
        products.reserveProduct();
        return true;
      }
      return false;
    }
  }
});
```

Finalmente, un fichero main.js arrancará todo usando require:

```
require(["purchase"],function(purchase){
  purchase.purchaseProduct();
});
```
Solución 2: Usar el sistema de módulos de node, CommonJS. Para ello:

1. Definir los módulos así:

garble.js
```
module.exports = function(string) {
//código
}
```
2. Usarlos así:

```
var garble = require("./garble");
console.log(garble(argument));
```

ACTUALIZACIÓN: hacer que todo sea módulo requireJS no tiene sentido. Con módulos de node se pueden importar todas las librerías que necesito.

##25-08-2016

* Hacer un plugin más sofisticado que el que tengo, con datos de elasticSearch

##30-08-2016

* Añadir una biblioteca JS al entorno de un plugin Kibana

1. Cargar un módulo de node que Kibana ya tiene preinstalado - Conseguido. Usado 'lodash'

```
	var lodash = require('lodash');
	var output = lodash.without([1, 2, 3], 1);
	console.log(output);
```

3. Cargar la biblioteca 'three' de alguna forma y usarla en el plugin del reloj
```
cd v3metrics/
npm install three
```
En algún sitio de clock.js cargo el módulo node, de manera que puedo acceder a toda la biblioteca three.js a través de la variable THREE.
```
var THREE = require("three");
```

En clock.html uso la biblioteca recién importada. Los métodos y las variables se usan accediendo a través de la variable recién inicializada:

        scene = new THREE.Scene();
        geometry = new THREE.BoxGeometry(200, 200, 200);

NOTA: tener claro qué ejecuta el servidor y qué el cliente. En este caso:
- El servidor resuelve este require("three") encontrando la biblioteca node adecuada donde corresponda (en este caso en v3metrics/build/node_modules).
- El servidor envía al cliente el fichero three.js, de manera que ahora el cliente puede ejecutar código usando esta nueva biblioteca.

4. Cargar una biblioteca personal en un plugin de kibana

TODO: Recomendar a Adrián publicar su biblioteca en npm.

#30-08-2016

He movido el código de uso de la biblioteca three dentro de un controlador angular. Es el sitio que le corresponde.

Intentando actualizar mi versión de kibana con la de desarrollo de github: el problema es que hace dos meses no actualizo. Entonces mi rama origin es muy diferente de la rama kibana/origin. Un simple git merge da un montón de conflictos (he tocado muchos ficheros).

Comandos muy útiles que he aprendido de git:

git push -f origin master : para forzar al servidor a perder algunos commits cuando no se deja
borrar todos los archivos manualmente
git add, git commit
git branch -u kibana/master: he puesto mi rama master a seguir a la rama master de kibana
git pull : se trae todos los cambios de kibana/master a local (porque ahora mi rama master apunta a kibana/master). Este es el comando que necesito utilizar ahora cada vez que quiera actualizar Kibana (hace un fetch y un merge automático).
git rm -r --cached myFolder : to untrack a folder

nvm install 6.4.0 (.node_version)
npm install : para actualizar los módulos de node de la nueva versión de kibana
Borro mis antiguos plugins
Con Yeoman genero un nuevo plugin base

Intento a partir de ahora usar la versión de elasticsearch que trae kibana, me evitaré problemas. Según CONTRIBUTING.md:
npm run elasticsearch
npm start

##02-09-2016

* Traer funcionalidad de mi antiguo plugin elasticsearch_status al nuevo plugin creado con Yeoman

Problemas encontrados:
- npm run elasticsearch. Error. Acciones realizadas: actualizar nvm, source ../nvm/nvm.sh, nvm install .node-version, npm install. #TODO: seguir investigando. De momento sigo usando mi elasticsearch por separado
- Parece que incluso mi elasticsearch antiguo tiene problemas para consultar datos
Discover: [parsing_exception] Unknown key for a START_ARRAY in [stored_fields]., with { line=1 col=302 }
Actualizo elasticsearch a alpha 5 desde la página oficial y cargo el dump con taskrabbit:
```
npm install elasticdump
source nvm/nvm.sh
node_modules/elasticdump/bin/elasticdump --input=/home/vio/Docs/opnfv_git_es.json --output=http://localhost:9200/commits-index
```

* Hacer una visualización con la biblioteca de Adrián, pero sin usar
crossfilter, sino usando un fichero JSON

##13-09-2016

Trabajando en importar lo necesario para poder usar 3dc.js:
- FontUtils.js, TextGeometry.js, Projector.js, OrbitControls.js: como lo único que hacen es atar objetos a THREE, puedo hacer require simplemente y añaden las funciones y variables al objeto THREE ya definido.
- domevents.js, Detector.js y Stats.js definen nuevas variables. Lo que hago es añadir una linea en esos ficheros, exportando dicha variable para poder importarla en clock.js
- THREEx.FullScreen.js, THREEx.WindowResize.js: he quitado las líneas tipo 

/** @namespace */
var THREEx		= THREEx 		|| {};

* Encapsular biblioteca de Adrián como módulo de node

##13-09-2016

Sobre las importaciones: se puede hacer de muchas formas. La forma más fácil es haciendo alguna chapuza. La que he hecho me vale de momento. El otro extremo es por ejemplo tener un módulo de node minimizado con todas las cosas extras que necesita 3dc.js + la propia biblioteca de 3dc.js y especificando lo demás como dependencia.

De momento. Hitos (o líneas de desarrollo):

1. Hacer un 3dc+elasticsearch sin kibana. Conseguir una visualización con 3dc.js, con datos que me traigo de elasticsearch mediante la api para javascript. Sin crossfilter.
2. Hacer un plugin con las siguientes características: una consulta a elasticSearch que me devuelva todos los campos. Selecciono los 20 primeros por ejemplo y los muestro en una tabla html. Mediante los controles de Kibana, permitir: 1. Que se pueda cambiar el nombre de la columna seleccionada. 2. Que se puedan añadir columnas. Por ejemplo, empezar con una por defecto y permitir añadir más.

##Ver notas cuaderno de notas (he seguido apuntando allí mi progreso)

##15-11-2016

Tengo de momento un pie3D y un barschart3D pero que es 2D en realidad. Pero funciona en Kibana. Hay cosas a pulir en el dashboard. Después de la reunión de hoy, hitos:

- Leyenda pie3D. Arreglar resaltado - Hecho. Adrián lo ha arreglado. Eso sí, me ha dicho no usar mouseover como función personalizada porque implosiona el universo.
- Centrar siempre el pie en el div. Tanto en visualización como en el dashboard. Quitar desplazamiento lateral con botoón secundario (dejar solo rotar). Quitar dat.gui
- Arreglar conflictos en dashboard entre charts. Filtros que no se aplican, "pie" que se dibuja en la escena del barschart, etc.
- Agregar tercera dimensión barsChart3D
- Agregar alguna visualización más a la biblioteca vr_charts. Bolas creo que será lo mejor

##18-11-2016

Estoy teniendo muchos problemas a la hora de separar escenas en el dashboard. He estado con Adrián durante horas para separar dos escenas, sin Kibana, y lo hemos conseguido (ver https://github.com/adrianalonsoba/THREEDC/tree/master/demo24).

Pero en Kibana no lo consigo hacer. He actualizado 3dc.js, ya no uso initializer, las variables son locales (creo) y no debería interferir nada. Aun así, lo hacen. Hoy quiero resolver esto. Para ello:

Sin Kibana:
1. Enterarme de qué son los closures en Javascript https://developer.mozilla.org/es/docs/Web/JavaScript/Closures 
2. Cambiar nombres de funciones sin 1,2
3. Convertir variables en locales de alguna forma (corchetes??)


