15-06-2015

He instalado un servidor Django. Usa bootstrap y angularjs. Creo que uso HTML5...

[img1] 

He instalado la biblioteca de Adrián y he estado jugando con ella creando gráficos. 

Cd threesome/
vio@lenovo:~$ python -m SimpleHTTPServer
localhost:8000

para cargar la versión adecuada de node.js (si no no funciona kibana ni elasticSearch)
source ../nvm/nvm.sh
bin/elasticsearch
bin/kibana

He estado instalando kibana 5 development y he mirado documentación y videos sobre cómo hacer plugins. Este es de los mejores tutoriales que he encontrado:

https://www.timroes.de/2015/12/02/writing-kibana-4-plugins-basics/

Todavía no he conseguido hacer un plugin “hola mundo”, pero estoy a punto.

He entendido la estructura de crossfilter. Adrián la usa en su código para pintar las gráficas. Mi misión al final es sustituir crossfilter por elasticSearch. Para ello tengo que colaborar mucho con Adrián cambiando o aislando crossfilter de las partes donde pinta, para luego hacerlo con elasticSearch.


20-06-2015

He estado mirando más documentación de Kibana
Al hacer git pull, se ha roto todo y kibana ya no funciona al ejecutarlo. Era cuestión de actualizar node.js:

source ../nvm/nvm.sh
nvm install "$(cat .node-version)"
npm install

He instalado el plugin timelion
vio@lenovo:~/kibanadev$ ./bin/kibana-plugin install https://download.elastic.co/kibana/timelion/timelion-5.0.0-alpha3.zip


21-06-2016

He conseguido crear mi primer plugin – el del reloj. No parece que haga nada pero al menos sale en visualizaciones.

Estoy peleando con kibana –dev que parece que no funciona

He hablado con Jesús y me ha dicho que reporte el error, si es que es un error. Pero más importante, me ha recomendado crear un .md en github con notas de lo que hago.

.MD Creado y solucionado el “bug”. Resulta que tenía que cambiar una variable solamente en mi sistema (ver github/Notes)


22-06-2016

Objetivos:

Actualizar proyecto Adrián
Poner los dos paneles de Adrián en un div en mi servidor Django
Ordenar y arreglar github
Hacer un plugin con una consulta sencilla a elasticSearch
