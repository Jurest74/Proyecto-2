# Proyecto-2

Mi página -> https://www.proyectosintegrador.com/
Dirección ip -> 3.87.34.23


<h3>Instalación y ejecución</h3>

<h5>Pasos para crear una página web en wordpress usando Docker Compose en una máquina de linux.</h5> 

Una vez creada la instancia de Google Cloud o AWS debebemos conectarnos por medio de ssh y seguir detalladamente los siguientes comandos.
En su máquina deberá habilitar en el grupo de seguridad el puerto 22 para la conexión mediante ssh y el puerto 80 para la conexión por su ip pública.

- ssh -i "miclave.pem" ec2-user@ec2-3-87-34-23.compute-1.amazonaws.com  // ejemplo para una máquina AWS.

- yum update // Realizamos primero una actualización.

-  sudo yum install docker -y  // Instalamos docker.

- docker --version  // Validamos la instación

- sudo service docker start  // Levantamos el servicio de docker

- sudo usermod -a -g docker ec2-user

Realizamos la instalación de la última versión de Docker Compose.
- sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

- mkdir www  // Creamos los directorios para nuestro proyecto

- mkdir wordpress

- cat > docker-compose.yml   //  Creamos y editamos nuestro archivo de configuración

<h5>Dentro del archivo pegamos el siguiente fragmento:</h5>

wordpress:
    image: wordpress
    links:
     - mariadb:mysql
    environment:
     - WORDPRESS_DB_PASSWORD=password
     - WORDPRESS_DB_USER=root
    ports:
     - "80:80"
    volumes:
     - ./html:/var/www/html
mariadb:
    image: mariadb
    environment:
     - MYSQL_ROOT_PASSWORD=password
     - MYSQL_DATABASE=wordpress
    volumes:
     - ./database:/var/lib/mysql

<h5>Finalmente creamos nuestro proyecto</h5>

- docker-compose up -d

Una vez finalizado el comando anterior deberá ir a al navegador y escribir la dirección ip pública de su máquina, vera una pantalla como la siguiente:

![image](https://user-images.githubusercontent.com/43093044/135964235-87f0fc7d-e295-435b-bb73-a320deb016ac.png)

Deberá proceder con la instalación y configuración de Wordpress.

<h3>Configuración registros DNS</h3>
Una vez tienes un dominio debes configurar para que este apunte a la dirección ip de tu máquina, en nuestro caso tenemos el dominio en SiteGround, donde debemos configurar los servidores de nombres.
![image](https://user-images.githubusercontent.com/43093044/135964815-a3b19802-88dd-4dc2-ad71-6109a1c30925.png)

Nuestra configuración de DNS y está cloudflare:

![image](https://user-images.githubusercontent.com/43093044/135964594-e288f85c-872b-4080-89a2-b3702dc2f25a.png)

<h3>Configuración Certificado SSL</h3>

En nuestro caso dentro de cloudFlare debemos ir a SSL/TLS

![image](https://user-images.githubusercontent.com/43093044/135965071-c91e70a6-e73e-4601-b0f3-aeefc801b24d.png)

Y seleccionamos la opción "Flexible".

![image](https://user-images.githubusercontent.com/43093044/135965182-259bd97e-bf4b-4b7a-ab42-d9e6f6138508.png)


<h3>Cambio dirección en Worpress</h3>


Debemos ir a Wordpress y en Ajustes -> Generales cambiar nuestras URL por las de nuestro dominio

![image](https://user-images.githubusercontent.com/43093044/135965411-8a98c9f8-09fb-4bc2-894d-8cb3d8730c8c.png)

<H3>Y LISTO !!! TENEMOS UNA PÁGINA WEB CON DOMINIO Y CERTIFICADO SSL</H3>

![image](https://user-images.githubusercontent.com/43093044/135965687-a9cde001-b476-4933-94f5-6e54bf92f370.png)


<h3>Referencias</h3>

- https://docs.docker.com/engine/install/
- https://upcloud.com/community/tutorials/deploy-wordpress-with-docker-compose/
- https://www.youtube.com/watch?v=S0r0FecM0ik&ab_channel=LOSMEJORESVIDEOSCORTOS
- https://www.youtube.com/watch?v=M2ToVR-CZtg&ab_channel=Inmagic
- https://dash.cloudflare.com/
- https://my.siteground.com/

 
