# Info de la materia:
ST0263 - Tópicos Especiales en Telemática
# Estudiantes:
Juan Pablo Madrid Florez, jpmadridf@eafit.edu.co  
Samuel Ceballos Posada, sceballosp@eafit.edu.co  
Daniela Ximena Niño Barbosa, dxninob@eafit.edu.co
# Profesor:
Edwin Nelson Montoya Munera, emontoya@eafit.edu.co


# 1. Breve descripción de la actividad
Realizar una aplicación monolítica no escalable desplegada en AWS, con nombre de dominio y certificado SSL válido.  
Realizar una aplicación monolítica no escalable desplegada en DCA, con nombre de dominio y certificado SSL válido.

## 1.1. Aspectos desarrollados de la actividad propuesta
- Crear una instancia de balanceador de cargas con un dominio y certificado SSL.
- Crear dos instancias Moodle.
- Crear una instancia de bases de datos MySQL.
- Crear una instancia de archivos distribuidos en NFS.
- Hacer el despliegue del ambiente de producción en AWS.
- Hacer el despliegue del ambiente de pruebas en el DCA.

## 1.2. Aspectos NO desarrollados de la actividad propuesta
- En el despliegue con el DCA, hay contenido que no despacha por https sino por http.


# 2. Información general
Se usaron contenedores Docker para hacer el despliegue de las diferentes aplicaciones.  

Se desplegó la siguiente arquitectura:  

<img width="406" alt="inf" src="https://user-images.githubusercontent.com/60080916/200102519-d1196b21-6d1c-4a92-9e96-82052237df97.PNG">

# 3. Descripción del ambiente de desarrollo, técnico y de ejecución

## IP o nombres de dominio
Del despliegue en AWS:
- IP: 44.212.162.247
- Nombre de dominio: proyecto26.equipo6.tk
- Dominio con certificación SSL: https://proyecto26.equipo6.tk

Del despliegue en el DCA:
- IP elástica: 192.168.10.161
- Nombre de dominio: proyecto26.dis.eafit.edu.co
- Dominio con certificación SSL: https://proyecto26.dis.eafit.edu.co

## Detalles técnicos
- AWS: se usó para desplegar las máquinas virtuales.
- DCA: se usó el Data Center Administrator para hacer uso de las máquinas virtuales.
- Docker: se usaron contenedores para desplegar las diferentes aplicaciones.
- Cerbot: se usó para asiganar un certificado SSL válido.
- Let's Encrypt: se usó para asiganar un certificado SSL válido.
- Nginx: se usó como servidor web HTTP.
- NFS kernel server: se usó para hacer el servidor NFS.
- NFS common: se usó para vincular los wordpress con el servidor NFS.
- Git: se usó para clonar respositorios de GitHub.
- Moodle: se usó para la creación de los CMS.

## Configuración del proyecto
A las maquinas virtuales del DCA, no se les debe hacer cambios, estas ya están configuradas por el profesor.  Para el despligue en AWS nos debemos encargar de crear y configurar las maquinas virtuales y configurar el dominio con los pasos explicados a contiuación.  

Se deben crear cinco maquinas virtuales en AWS.  

<img width="714" alt="Instancias" src="https://user-images.githubusercontent.com/60080916/200099650-aa1e5758-04ad-4fad-b43c-7443169f3320.PNG">

Al la instancia del balanceador de cargas se le debe asignar una dirección IP elástica.  

<img width="690" alt="2" src="https://user-images.githubusercontent.com/60080916/200099715-71305ea4-5f20-4767-a22f-3d5e1699a40e.PNG">

Para la instancia de la base de datos se debe abrir el puerto 3306 y para la instancia del NFS se debe abrir el puerto 2049.  

<img width="836" alt="3" src="https://user-images.githubusercontent.com/60080916/200099843-67ae37d9-1346-4dcb-92c0-ffbbe4032aa2.PNG">
<img width="829" alt="4" src="https://user-images.githubusercontent.com/60080916/200099842-2e88ebb6-c895-4730-a43c-e307a042d047.PNG">

Se configuran los servidores de nombre de dominio en Freenom.  

![WhatsApp Image 2022-11-04 at 11 03 41 PM](https://user-images.githubusercontent.com/60080916/200099986-8e664599-6217-4141-aacc-0e02b6ef3a87.jpeg)

Se configura el DNS en Cloudflare.  

![WhatsApp Image 2022-11-04 at 11 02 13 PM](https://user-images.githubusercontent.com/60080916/200099967-c7ae8ed2-a7a3-4a7b-a501-22e45bfcee65.jpeg)

Se configura el dominio para especificar que el certificado SSL viene asignado en el servidor nginx.  

![WhatsApp Image 2022-11-04 at 11 02 43 PM](https://user-images.githubusercontent.com/60080916/200100042-447c8792-da70-484e-bbc3-ee506e506743.jpeg)

## Lanzar el servidor
Para lanzar los servidores en el despligue de AWS, debemos ingresar a las maquinas virtuales con SSH y lanzar los contenedores como se explica posteriormente.  

<img width="595" alt="5" src="https://user-images.githubusercontent.com/60080916/200100223-18887ae7-1192-4b20-a455-92d86d3135c4.PNG">

Para lanzar los servidores en el despligue del DCA, debemos ingresar a las maquinas virtuales con SSH y lanzar los contenedores como se explica posteriormente.  Este es el comando para conectarse a las maquinas:
```
ssh userdca@ip
```

## Detalles de desarrollo
En el siguiente desarrollo los comandos ejecutados son los usados para el despliegue de AWS, estos mismos comandos son los usados para el despligue en el DCA, solo hace falta cambiar los nombres de usuario y direcciones IP para que funcione correctamente.  

### 1. Balanceador de cargas

### 2. Servidor de base de datos
Instalamos Docker, Docker-compose y Git:
```
sudo apt update
sudo apt install docker.io -y
sudo apt install docker-compose -y
sudo apt install git -y
```

Clonamos el GitHub del proyecto para copiar el contenedor de la base de datos:
```
git clone https://github.com/dxninob/ST0263-P2.git
sudo mkdir /home/ubuntu/mysql
cd ST0263-P2/mysql
sudo cp docker-compose.yml /home/ubuntu/mysql
```

Ponemos a funcionar Docker:
```
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -a -G docker ubuntu
```

Corremos el contenedor Docker:
```
cd /home/ubuntu/mysql
sudo docker-compose up --build -d
```

### 3. Servidor NFS
Instalamos nfs-kernel-server y ufw:
```
sudo apt update
sudo apt install nfs-kernel-server
sudo apt install ufw
```

Crear carpeta donde quedarán almacenados los archivos en el servidor NFS:
```
sudo mkdir -p /mnt/nfs_share
```

Cambiamos los permisos de la carpeta para que un cliente pueda acceder leer y escribir en esta:
```
sudo chown -R nobody:nogroup /mnt/nfs_share/
sudo chmod 777 /mnt/nfs_share/
```

Ingresamos al archivo /etc/exports:
```
sudo nano /etc/exports
```

Agregamos la siguiente línea en el archivo:
```
/mnt/nfs_share 172.31.0.0/16(rw,sync,no_subtree_check,no_root_squash)
```

Exportamos el directorio compartido del NFS y reiniciamos el servidor NFS:
```
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

Actualizamos las reglas de firewall:
```
sudo ufw allow from 172.31.0.0/16 to any port nfs
sudo ufw allow 22
sudo ufw enable
sudo ufw status
```

### 4. Servidores de Moodle
Estos comandos se deben ejecutar en las dos instancias que deben contener Moodle.  

Instalamos nfs-common:
```
sudo apt update
sudo apt install nfs-common -y
```

Ingresamos al archivo /etc/fstab:
```
sudo nano /etc/fstab
```

Ingresamos esta línea para montar la carpeta compartida con el servidor NFS:
```
172.31.19.234:/mnt/nfs_share /var/www/html nfs auto 0 0
```

Con este comando podemos ver que sí haya quedado montada la carpeta correctamente:
```
sudo mount
```

En este punto, hay que reiniciar la maquina virtual del Moodle, en AWS esta se puede reiniciar desde la plataforma y para el DCA se puede usar este comando ```sudo shutdown -r```.  Luego de hacer esto, la conexión con el servior NFS ha quedado lista.  Los siguientes comandos son para correr Moodle en las maquinas virtuales.  

Instalamos Docker, Docker-compose y Git:
```
sudo apt install docker.io -y
sudo apt install docker-compose -y
sudo apt install git -y
```

Clonamos el GitHub del proyecto para copiar el contenedor de los Moodle:
```
git clone https://github.com/dxninob/ST0263-P2.git
sudo mkdir /home/ubuntu/moodle
cd ST0263-P2/moodle
sudo cp docker-compose.yml /home/ubuntu/moodle
```

Ponemos a funcionar Docker:
```
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -a -G docker ubuntu
```

Corremos el contenedor Docker:
```
cd /home/ubuntu/moodle
sudo docker-compose up --build -d
```


## Como un usuario lo utilizaría
El usuario solo debe acceder a la URL https://proyecto26.equipo6.tk desde cualquier browser.


## Resultados
### Despliegue en AWS
Prueba la instancia "Moodle 1"  

<img width="960" alt="m1" src="https://user-images.githubusercontent.com/60080916/200104777-648ddc1a-c248-4c2d-ab79-7901ea5769e3.PNG">

Prueba la instancia "Moodle 2"  

<img width="960" alt="m2" src="https://user-images.githubusercontent.com/60080916/200104776-d8f51474-396e-4d69-853a-cd5f631834eb.PNG">

Prueba del dominio que hace uso de la instancia "Load Balancer"  

<img width="960" alt="lb" src="https://user-images.githubusercontent.com/60080916/200104775-7b5b6bfb-a97f-4ebe-9d91-b742ef6f052a.PNG">

Certificado SSL de la página  

<img width="407" alt="ssl" src="https://user-images.githubusercontent.com/60080916/200104774-bb72398c-f493-4eed-9bee-b1a7f78042e0.PNG">

### Despliegue en el DCA
Prueba la instancia "Moodle 1"  

<img width="960" alt="dcam1" src="https://user-images.githubusercontent.com/60080916/200104952-66fe113c-e0c1-47f8-99f0-9235c84826fe.PNG">

Prueba la instancia "Moodle 2"  

<img width="960" alt="dcam2" src="https://user-images.githubusercontent.com/60080916/200104950-39222094-9509-43e3-84bd-f840bd02abbb.PNG">

Prueba del dominio que hace uso de la instancia "Load Balancer"  

<img width="960" alt="lbdca" src="https://user-images.githubusercontent.com/60080916/200107105-ed1e400c-515e-4df2-9fda-2cf4de5f3640.PNG">

Certificado SSL de la página  

<img width="408" alt="ssl" src="https://user-images.githubusercontent.com/60080916/200107103-1fa501b0-f0be-43fa-b66d-7f0dcfdc9856.PNG">


# 5. Referencias
- https://github.com/st0263eafit/st0263-2022-2/tree/main/docker-nginx-wordpress-ssl-letsencrypt
- https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-setup-an-Nginx-load-balancer-example
- https://linuxhint.com/install-and-configure-nfs-server-ubuntu-22-04/
- https://hub.docker.com/r/bitnami/moodle
