Building a simple LAMP stack and deploying Application using Ansible Playbooks.
-------------------------------------------

These playbooks require Ansible 1.2.

These playbooks are meant to be a reference and starter's guide to building
Ansible Playbooks. These playbooks were tested on CentOS 6.x but for this work we modified and tested in Ubuntu 16:04 ,so we recommend
that you use Ubuntu  to test these modules.

This LAMP stack can be on a single node or multiple nodes. The inventory file
'hosts' defines the nodes in which the stacks should be configured.

        [webservers]
        web_server

        [dbservers]
        mysql_server
        
        
        
<h2>Web_server</h2>
En el servidor web se realiza la instalación del servicio httpd y ntp, los cuales funcionan como servidor web y administrador de fechas . Para Cent os se usaron algunos modulos que son especificos :

<b>yum:</b> permite el uso del manejador de repositorios de Centos. El modulo equivalente en Ubuntu es 'apt'.

<b>lineinfile:</b> funciona como la funcion sed en bash, permite el reemplazo o adicion de contenido a archivos buscando este por una expresión regular, si la encuentra realiza lo que se colocó en el atributo line.En el atributo regexp se especifica la expresion regular a buscar

<b>seboolean:</b>permite la configuración de variables en el modulo seguro SELinux cambiando su estado a true o false dependiendo de su uso

<h2>mysql_server</h2>

Para la configuración del host con el motor de base de datos se instala con Ansible el motor MySQL y se realiza el uso de modulos ya explicados y aparecen 2 nuevos modulos.

mysql_db: este modulo permite gestionar las bases de datos en el motor MySQL
mysql_user: este modulo permite gestionar los usuarios de una base de datos MySQL

estos modulos  interactuan directamente con el servicio MySQL no con el OS en el host, es decir que no necesitan una equivalencia en Ubuntu, por lo tanto su uso sera igual que si fuera Cent os .


Here the webserver would be configured on the local host and the dbserver on a
server called "mysql_server". The stack can be deployed using the following
command:

        ansible-playbook -i hosts site.yml

Once done, you can check the results by browsing to http://localhost/index.php.
You should see a simple test page and a list of databases retrieved from the
database server.
