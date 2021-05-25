# Ansible

we put all the hosts ip in ip.txt file with the following syntax:

```
ip  ansible user=root  ansible_ssh_pass=redhat  ansible_connection=ssh
```
ip=ip address of slave node(ip of node from which it is to be connected).
ansible user= **root** :username of your linux(in my case it is root) vm where ansible is been installed and working as the master node.
ansible_ssh_pass=redhat : password of your linux root vm
ansible_connection=ssh : type of network connection through which ansible master node will be connecting to its slave node, generally it is 'ssh' only.
 
 Example of this can be:
 ````
 192.168.1.18 ansible_user=root ansible_ssh_pass=redhat ansible_connection=ssh
 ````
 
 cmd used to see all the present nodes:
 
 ````
ansible all --list-hosts
 ````
 
 ![image](https://user-images.githubusercontent.com/64470404/119257294-3fa96080-bbe2-11eb-8c85-7700a261ed7f.png)

It'll list all the available hosts present in the node.

----------------------------------------------------

````
ansible-playbook --syntax-check web.yml
````
![image](https://user-images.githubusercontent.com/64470404/119257621-b561fc00-bbe3-11eb-832f-51446e6a932e.png)


````
ansible all -m package -a "name=httpd state=present"
````
for installing packages
![image](https://user-images.githubusercontent.com/64470404/119257707-17226600-bbe4-11eb-9006-89491244608d.png)


````
ansible all -m service -a "name=httpd state=started"
````

![image](https://user-images.githubusercontent.com/64470404/119258416-26ef7980-bbe7-11eb-863d-4e00180befe0.png)

![image](https://user-images.githubusercontent.com/64470404/119258424-2d7df100-bbe7-11eb-967d-8d24acc0cb73.png)
![image](https://user-images.githubusercontent.com/64470404/119258490-733ab980-bbe7-11eb-8089-31a01ca35dc0.png)

![image](https://user-images.githubusercontent.com/64470404/119258508-89487a00-bbe7-11eb-832d-c6d97c59691f.png)


-----------

yum config in cli

![image](https://user-images.githubusercontent.com/64470404/119259418-e21a1180-bbeb-11eb-9eab-fb9ecf404a11.png)

playbook:

````
- hosts: all
  tasks:
  - file:
      state: directory
      path: "/dvd1"
  - mount:
      src: "/dev/cdrom"
      path: "/dvd1"
      state: mounted
      fstype: "iso9660"

  - yum_repository:
      baseurl: "/dvd1/AppStream"
      name: "mydvd1"
      description: "yum for AppStream"
      gpgcheck: no

  - yum_repository:
      baseurl: "/dvd1/BaseOS"
      name: "mydvd2"
      description: "yum for BaseOS"
````

````
ansible-playbook yum.yml
````

![image](https://user-images.githubusercontent.com/64470404/119262033-67ef8a00-bbf7-11eb-85a9-02a39172e958.png)


-------------------

hta

![image](https://user-images.githubusercontent.com/64470404/119261902-fa435e00-bbf6-11eb-9d6a-693e0b6092f4.png)

````
- hosts: all
  tasks:
  - package:
      name: "httpd"

  - copy: 
      dest: "/var/www/html/index.html"
      content: "secure page!!"
 
  - replace:
      path: "/etc/httpd/conf/httpd.conf"
      regexp: "AllowOverride None"
      replace: "AllowOverride AuthConfig"

  - copy:
      dest: "/var/www/html/.htaccess"
      src: ".htaccess"

  - package:
      name: "python36"

  - pip:
      name: "passlib"

  - htpasswd:
      path: "/etc/www.passwd"
      name: "bluenova"
      password: "redhat"

  - service: 
      name: "httpd"
      state: restarted
````

---------
aws

![image](https://user-images.githubusercontent.com/64470404/119487969-a318c700-bd77-11eb-8522-6a529890ea0c.png)


