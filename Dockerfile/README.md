<h1>Configuraciones básicas y creación de dockers</h1>

En este directorio se encuentra el archivo Dockerfile para la creación de la imagen la cual permitirá crear los contenedores de docker y el archivo authorized_keys para la Configuracion de la autenticacion por llaves en el servicio SSH de cada contedenor.

<h3>Primer paso</h3>

<b>Debes construir un docker personalizado que incluye el servidor openssh</b>

<strong>-Creación de la imagen en la que se basará el contenedor.</strong>

<b>Se ejecuta el siguiente comando :</b>

$ (sudo) docker build -t {{ nombre imagen }} .

<h3>En mi caso el {{ nombre contenedor }} será server_parcial</h3>

$ (sudo) docker build -t server_parcial .

<h3>Segundo paso, despliege</h3>

<b>Creacion de contenedores.</b>

<b>Ahora debes crear un conjunto de maquinas para el despliegue, se creará un servidor web (apache) y uno de bases de datos (mysql).</b>

<b>Nombre del contenedor web = web_server</b>

$ (sudo) docker run -d -P --name web_server -p 2221:22 -p 80:80 server_parcial

<b>Nombre del contenedor mysql = mysql_server</b>

$ (sudo) docker run -d -P --name mysql_server -p 2222:22 -p 3306:3306 server_parcial


<h3>Tercer paso, configuración de alias</h3>

<b>Opción 1:</b> edita el archivo /etc/hosts y adiciona 3 alias a localhost

127.0.0.1  web_server mysql_server

<b>Opción 2:</b> adición automática en el archivo de hosts del sistema

echo "127.0.0.1 web_server mysql_server" | sudo tee -a /etc/hosts


<h3>Adicionar las llaves ssh</h3>


<b>-Configurando nuestra llave privada.</b>

<b>Para realizar nuestra conexion nuestra llave privada debe contar con los permisos de esta forma [-xw-------] lo que equivale a 0600.</b>

chmod 0600 ../key.private

ssh -o StrictHostKeyChecking=no root@web_server -p 2221 -i ../key.private hostname
ssh -o StrictHostKeyChecking=no root@mysql_server -p 2222 -i ../key.private hostname

<h3>Cuarto paso, confirmación</h3>

<b>Realiza una prueba de conexión a las maquinas que se crearon recientemente, en el paso anterior se crearon 2 maquinas con el puerto 2221 y 2222 abiertos para conexión:</b>

ssh root@web_server -p 2221 -i ../key.private
ssh root@mysql_server -p 2222 -i ../key.private

<b>Si la conexión se establece, ya está listo para seguir con ansible.</b>
