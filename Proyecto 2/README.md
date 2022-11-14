# Info de la materia:

ST0263 - Tópicos Especiales en Telemática

# Estudiantes:

Juan Pablo Madrid Florez, jpmadridf@eafit.edu.co  
Samuel Ceballos Posada, sceballosp@eafit.edu.co  
Daniela Ximena Niño Barbosa, dxninob@eafit.edu.co

# Profesor:

Edwin Nelson Montoya Munera, emontoya@eafit.edu.co

# 1. Breve descripción de la actividad

Se realizó el despliegue de una aplicación open source LAMP con Moodle. El despliegue se realizó con maquinas de AWS en cloud, utilizando los servicios EC2, ELB, RDS y EFS.

## 1.1. Aspectos desarrollados de la actividad propuesta

- Crear un template con Moodle.
- Crear una base de datos MariaDB con el servicio de RDS.
- Crear un servidor NFS en el servicio EFS.
- Crear un grupo auto escalable en AWS.
- Crear un balanceador de carga para las instancias.
- Asignar un certificado ssl al dominio.

## 1.2. Aspectos NO desarrollados de la actividad propuesta

- Se cumplio con todo lo propuesto.

# 2. Información general

- Se usaron contenedores Docker para hacer el despliegue de las instancias.

# 3. Descripción del ambiente de desarrollo, técnico y de ejecución

## IP o nombres de dominio

Del despliegue en AWS:

- Nombre DNS: ELB-MyWebApp-258926532.us-east-1.elb.amazonaws.com
- Nombre de dominio: equipo6.tk
- Dominio con certificación SSL: https://proyecto26.equipo6.tk

## Detalles técnicos

- EC2: se usó para desplegar las máquinas virtuales.
- RDS: se usó para desplegar la base de datos.
- EFS: se usó para desplegar el servidor NFS.
- Docker: se usaron contenedores para desplegar las diferentes aplicaciones.
- Git: se usó para clonar respositorios de GitHub.
- Moodle: se usó para la creación de los CMS.

## Configuración del proyecto

### 1. Crear grupos de seguridad

Acceder a la pestaña de grupos de seguridad en el panel de la izquierda en EC2 y crear los siguientes grupos:

- Moodle-Web (HTTP, SSH):
![WEB](https://user-images.githubusercontent.com/60147093/201779418-f952a832-05ed-4c64-8a06-e77e8808aca3.PNG)
- Moodle-RDS (MySQL):
![RDS](https://user-images.githubusercontent.com/60147093/201779417-c39a8527-d25c-4a36-8068-214a1b02e786.PNG)
- Moodle-EFS (NFS):
![EFS](https://user-images.githubusercontent.com/60147093/201779411-b1b5aa4c-b519-4746-a29c-cafc0101425f.PNG)
- Moodle-ELB (HTTPS, HTTP):
![ELB](https://user-images.githubusercontent.com/60147093/201779416-e0932596-eed9-46e0-af79-f2df89339871.PNG)

### 2. Crear servidor NFS en EFS:

Acceder al servicio de EFS, dar click en crear y seguir los pasos:

- Asignar un nombre y dar click en customizar:
![1](https://user-images.githubusercontent.com/60147093/201779681-8471218e-da9f-4d26-ad93-c1e1bdbb644a.PNG)
- Quitar los grupos de seguridad existentes y asignar el de moodle-EFS creado anteriormente:
![2](https://user-images.githubusercontent.com/60147093/201779683-0550253a-3a05-46e0-baca-ebc7bb44d0cc.PNG)
- De click en crear.

### 3. Crear base de datos en RDS:

Acceder al servicio de RDS, dar click en crear y seguir los pasos de configuraion:

![WhatsApp Image 2022-11-10 at 11 39 13 AM](https://user-images.githubusercontent.com/60147093/201779904-26fd6476-2ebe-4512-998c-04bb1114678f.jpeg)
![WhatsApp Image 2022-11-10 at 11 39 32 AM](https://user-images.githubusercontent.com/60147093/201779906-756b83cd-c3ec-45f3-9316-f8bd246e1694.jpeg)
![WhatsApp Image 2022-11-10 at 11 39 52 AM](https://user-images.githubusercontent.com/60147093/201779907-31de58f1-83d0-48d9-b3d6-606e2f3b9e78.jpeg)
![WhatsApp Image 2022-11-10 at 11 40 10 AM](https://user-images.githubusercontent.com/60147093/201779909-399c3e8d-afaa-4c51-ac6e-5ec82a5558d9.jpeg)
![WhatsApp Image 2022-11-10 at 11 40 31 AM](https://user-images.githubusercontent.com/60147093/201779911-6efc968c-063a-4545-97e6-02c00315de35.jpeg)
![WhatsApp Image 2022-11-10 at 11 41 00 AM](https://user-images.githubusercontent.com/60147093/201779915-1be2fd67-32b1-4af7-9d1e-95cdb5fea96f.jpeg)
![WhatsApp Image 2022-11-10 at 11 41 38 AM](https://user-images.githubusercontent.com/60147093/201779918-858414b7-1d1a-4940-9adb-1968d420bb44.jpeg)

### 4. Crear instancia template con moodle:

Acceder al servicio de EC2 y crear una instancia ubuntu 22.04 con la siguiente configuracion:

![WhatsApp Image 2022-11-10 at 11 53 54 AM](https://user-images.githubusercontent.com/60147093/201780057-71016658-588b-4979-bb5b-d58d5a7e8905.jpeg)
![WhatsApp Image 2022-11-10 at 11 54 26 AM](https://user-images.githubusercontent.com/60147093/201780058-14c79561-4f8f-42da-85f9-a92220614db3.jpeg)
![WhatsApp Image 2022-11-10 at 11 55 26 AM](https://user-images.githubusercontent.com/60147093/201780060-696a08df-0723-4e81-b254-bacf55d13870.jpeg)
![WhatsApp Image 2022-11-10 at 11 56 13 AM](https://user-images.githubusercontent.com/60147093/201780061-ebbb7fad-023a-4d0a-ac96-8d570a261724.jpeg)

Conectarse por ssh a la instancia y seguir estos pasos:

- Instalar el cliente de MySQL:
```
sudo apt install mysql-client
```

- Crear una base de datos en RDS:

<img width="713" alt="create database" src="https://user-images.githubusercontent.com/60147093/201780181-f7eb8c9f-50de-4fbd-8439-d23b8427298e.PNG">

- Instalamos Docker, Docker-compose y Git:
```
sudo apt install docker.io -y
sudo apt install docker-compose -y
sudo apt install git -y
```

- Clonamos el GitHub del proyecto para copiar el contenedor de los Moodle:
```
git clone https://github.com/dxninob/ST0263-P2.git
sudo mkdir /home/ubuntu/moodle
cd ST0263-P2/Proyecto 2/moodle
sudo cp docker-compose.yml /home/ubuntu/moodle
```

- Ponemos a funcionar Docker:
```
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -a -G docker ubuntu
```

- Corremos el contenedor Docker:
```
cd /home/ubuntu/moodle
sudo docker-compose up --build -d
```

### 5. Crear una AMI:

En el panel de instancias, seleccionar la intancia que quiere usar para la AMI.

En el menú de Actions, Create Image y configure los siguientes parámetros:

- Image name: Web Moodle AMI
- Image description: AMI for Web Server
- Click en Create Image.

### 6. Crear un Target Group:

En el menú de EC2, en la sección de Load Balancing, escoja Target Groups y seguir los pasos:

Basic Configuration:

- Choose a target type: Instances
- Target group name: TG-MyWebApp
- Protocol: HTTP:80
- VPC: VPC-default
- Protocol version: HTTP1

Register Targets:

- No hay ninguna instancia para incluir entonces ignore este paso y de click en **create target group**.

### 7. Crear un balanceador de carga:

En el menú de EC2, escoja Load Balancers y seguir los pasos:

### 8. Crear un Launch Template:

En el menú de EC2, escoja Launch Templates y seguir los pasos:

### 9. Crear un Grupo de Auto Scaling:

En el menú de EC2, escoja Auto scaling group y seguir los pasos:

## Como un usuario lo utilizaría

El usuario solo debe acceder a la URL https://proyecto26.equipo6.tk desde cualquier browser.

## Resultados

# 5. Referencias
