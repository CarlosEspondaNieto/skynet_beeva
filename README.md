# skynet_beeva


# Requisitos

Instalar elasticsearch en su versión 5.2.2
Instalar python en su versión 3.6
Instalar apache en su versión 2.4.6

## Instrucciones FRONT-END
	*Se usó la versión de apache predeterminada que trae centos 7: Apache/2.4.6 (CentOS)
	*Importante:Los siguientes pasos se hicieron como root.
	*inicializar el servicio apache con el comando:
	
**systemctl start httpd**

	*En la carpeta /var/www/html/ clonar el repositorio “skynet_beeva”.
	*comando:*
**git clone https://github.com/drefk99/skynet_beeva.git **

	*en ese directorio skynet_beeva crear el virtualenv con el comando
**virtualenv -p python3.6 env **
	
	*activar el virtualenv*
**source env/bin/activate**
	
	*A continuación  instalar el archivo requirements.txt que instala los paquetes necesarios de los scripts:
**pip install -r requirements.txt**

    *En al carpeta “data” contiene los json de elasticsearch el cual manda los datos historios y además se
    recuperan los json de python de los datos mas relevantes obtenidos del día y además contiene el JavaScript de
    las graficas en donde se obtiene los datos de los json para el manejo de las graficas.

    *En “pages” contiene los html utilizados, la parte visual.

    *En las carpetas “dist” y “vendor” contiene los css y el framework boostrap para facilitar el manejo de la
    parte visual.

# Descarga de historico

Para descargar los archivos historicos se creo un script en python que se encargará de descargar los archivos
del dia con base a la fecha actual de forma automatica. Basta con ejecutar el achivo auto_todo.sh que se 
encuentra en la carpeta "python-scripts-historico".

Este archivo toma la fecha actual del sistema y descarga todos los tweets del día con el horario del 
meridiano de greenwich.

Es importante tener en cuenta que por default tiene el localhost como host, si se desea cambiar esto puede editar
el archivo charls.py. En este archivo es necesario agregar las credenciales para acceder a la API de Twitter. 

Si desea unicamente ejecutar el archivo charls que contiene la API que descarga los tweets necesita tres argumentos 
para su ejecución como son el banco, la fecha de inicio y la fecha de termino en formato "aaaa/mm/dd", como se 
puede observar acontinuación.

**python charls.py "banco" "fecha_ini" "fecha_fin"**

Si se desea consultar el contenido de la base de datos solo requiere el nombre del banco y la fecha (aaaa-mm-dd), y 
sustituir los valores entre comillas en los siguientes comandos:

Terminal
**curl -XGET "ip_o_host:9200/skynet_beeva/nombre_banco/aaaa-mm-dd?pretty=true"**

Navegador
ip_o_host:9200/skynet_beeva/nombre_banco/aaaa-mm-dd?pretty=true

# Scripts de Python para el análisis



* En el script de extract.py es necesario agregar las credenciales para acceder a la API de Twitter. 

* El script test-naives-bayes.py es para crear un clasificador a partir de un dataset con una columna con textos y otra con su etiqueta. En la misma carpeta en el archivo pickle ya esta creado un clasificador y un diccionario necesario para el script load_classifier.py.

	* Para el escript extract.py son necesarios tres argumentos
	* El nombre del banco para búsqueda ej: Bancomer (*nota: puedes poner cualquier nombre de banco que opere en México) 
	* Fecha de inicio de búsqueda ej: 2017-03-18
	* Fecha de termino de búsqueda ej: 2017-03-19

* en el caso de Santander, se debe utilizar la cadena "Banco Santander" porque el término Santander tiene varios significados. 
* El script subida_predi.py es para subir la información a la base de datos, necesita dos argumentos el primero es el doc_type, que para nuestro caso es el nombre del banco y el segundo es el id o el nombre del archivo json a subir.

* El script embed_test.py junta la parte de extracción(extract.py), análisis(load_classifier.py) y envio a la base de datos de Elasticsearch(subida_predi.py) tiene como argumentos el nombre del banco la fecha de inicio y fecha de término, como el script extract.py.

* El auto_todo.sh es para correr el script test_embed.py de manera continua con crontab, se puede configurar la ruta del script y el intervalo de tiempo








# Ingreso

Para ingresar al sistema desde el navegador se necesita ingresar a la dirección 
http://ip_o_host/skynet_beeva/pages

*Nota: Si es necesario declarar en el crontab es importnate revisar y cambiar las rutas de los archivos que ahi se señalan por
  las rutas absolutas:
  *Carpea python-scripts-analisis
    *subida_predi.py
    *load_clasifier.py
    *extract.py
  *Carpeta python-scripts-historico
    *auto_todo.sh
    *charls.py
    
Tambien es importante contemplar todos los archivos requirements.txt que estan en analisis e historico.
  
