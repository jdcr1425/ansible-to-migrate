En este directorio se encuentra el archivo Dockerfile para la creación de la imagen la cual permitirá crear los contenedores de docker y el archivo authorized_keys para la Configuracion de la autenticacion por llaves en el servicio SSH de cada contedenor.

Configuraciones básicas y creación de dockers de prueba

Primer paso

Debes construir un docker personalizado que incluye el servidor openssh

-Creación de la imagen en la que se basará el contenedor.

Se ejecuta el siguiente comando :

$ (sudo) docker build -t {{ nombre imagen }} .

En mi caso el {{ nombre contenedor }} será server_parcial

$ (sudo) docker build -t server_parcial .

Segundo paso, despliege

Creacion de contenedores.

Ahora debes crear un conjunto de maquinas para el despliegue, se creará un servidor web (apache) y uno de bases de datos (mysql).

Nombre del contenedor web = web_server

$ (sudo) docker run -d -P --name web_server -p 2221:22 -p 80:80 server_parcial

Nombre del contenedor mysql = mysql_server

$ (sudo) docker run -d -P --name mysql_server -p 2222:22 -p 3306:3306 server_parcial


Tercer paso, configuración de alias

Opción 1: edita el archivo /etc/hosts y adiciona 3 alias a localhost

127.0.0.1  web_server mysql_server

Opción 2: adición automática en el archivo de hosts del sistema

echo "127.0.0.1 web_server mysql_server" | sudo tee -a /etc/hosts


Adicionar las llaves ssh


Configurando nuestra llave privada.

Para realizar nuestra conexion nuestra llave privada debe contar con los permisos de esta forma [-xw-------] lo que equivale a 0600.

chmod 0600 ../key.private

ssh -o StrictHostKeyChecking=no root@web_server -p 2221 -i ../key.private hostname
ssh -o StrictHostKeyChecking=no root@mysql_server -p 2222 -i ../key.private hostname

Cuarto paso, confirmación

Realiza una prueba de conexión a las maquinas que se crearon recientemente, en el paso anterior se crearon 2 maquinas con el puerto 2221 y 2222 abiertos para conexión:

ssh root@web_server -p 2221 -i ../key.private
ssh root@mysql_server -p 2222 -i ../key.private

Si la conexión se establece, ya está listo para seguir con ansible.
