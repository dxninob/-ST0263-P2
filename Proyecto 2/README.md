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

- Moodle-Web:
- Moodle-RDS:
- Moodle-EFS:
- Moodle-ELB:

### 2. Crear servidor NFS en EFS:

Acceder al servicio de EFS, dar click en crear y seguir los pasos:

- Asignar un nombre y dar click en customizar:
- Quitar los grupos de seguridad existentes y asignar el de moodle-EFS creado anteriormente:
- De click en crear.

### 3. Crear base de datos en RDS:

Acceder al servicio de RDS, dar click en crear y seguir los pasos de configuraion:

### 4. Crear instancia template con moodle:

Acceder al servicio de EC2 y crear una instancia ubuntu 22.04 con la siguiente configuracion:

Conectarse por ssh a la instancia y seguir estos pasos:

- Instalar el cliente de MySQL:

```
sudo apt install mysql-client
```

- Crear una base de datos en RDS:

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
