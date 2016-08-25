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


##25-08-2016

* Hacer un plugin más sofisticado que el que tengo, con datos de elasticSearch
* 
