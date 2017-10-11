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

Here the webserver would be configured on the local host and the dbserver on a
server called "mysql_server". The stack can be deployed using the following
command:

        ansible-playbook -i hosts site.yml

Once done, you can check the results by browsing to http://localhost/index.php.
You should see a simple test page and a list of databases retrieved from the
database server.
