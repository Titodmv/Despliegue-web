# Prueba
## Instalar Docker
Información sacada de la pagina oficial de Docker para ubuntu **https://docs.docker.com/engine/install/ubuntu/**
> Añadir la key oficial de docker
```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
> Añadir al repositorio apt
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
> Instalar docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
> Comporbar la instalación
```
sudo docker -v
```
## Clonar un repositorio
> Usar git clone
```
git clone <url> <ruta_destino_del_equipo>
```
## Crear un dockerfile en la carpeta base del proyecto
```
#Importar imagen base del proyecto
FROM alphine:latest

#Comandos que se ejecutaran, necesarios para que funcione react
RUN apk add --no-cache nodejs npm

#Directorio de trabajo que se crear en el contenedor docker
WORKDIR /aplicacion

#Copiamos el json para instalar las dependencias y el lock en caso de que exista
COPY package.json package-lock.json ./

#Instalar las dependencias js
RUN npm install

#Copiar el projecto en la base del proyecto
COPY . .

#Puerto que usara el contenedor docker
EXPOSE 3000

#Encender el proyecto
CMD [ "npm", "start" ]
```
## Hacer build desde la carpeta del proyecto (este paso ahora no es necesario hacerlo)
```
sudo docker build -t <nombre que le daremos a la imagen> .
```
## Confirmar que la imagen se a creado (este paso ahora no es necesario hacerlo)
```
sudo docker images
```
## Correr la imagen (este paso ahora no es necesario hacerlo)
```
sudo docker run -p <puertoMaquina>:<puertoDelProyecto> <imagen del proyecto> 
```
## Crear un docker-compose.yaml en la carpeta donde se encuentren los dos proyectos
Recordad que se ha de respetar los espacios y tabulaciones, en caso contrario no funcionara el compose
```
#La version no es necesaria declararla.
#Primero declaramos los servicios que se van a levantar
services:
  react:
    build:
      #Indicamos la carpeta donde se encuentra nuestro proyecto de react
      context: ./react
      #Indicamos como se llama el dockerfile del proyecto react
      dockerfile: Dockerfile
    #Le damos un nombre al proyecto
    container_name: react
    #Le indicamos el puerto que va a usar el contenedor de react
    ports:
      - "3000:3000"
    # Como nuestro proyecto de react depende del de node para su funcionamiento corerecto
    # le decimos que tiene que encender antes el servicio de node
    depends_on:
      - node
    # Esto aun no se que hace, por investigar
    stdin_open: true
    # Indicamos a la red que se conectara
    networks:
      - mi_red

  node:
    build: 
      context: ./node
      dockerfile: Dockerfile
    container_name: node
    ports:
      - "5000:5000"
    networks:
      - mi_red
#Ahora va a crear la red mi_red de tipo puente para que los dos proyectos se puedan conectar entre si
networks:
  mi_red:
    driver: bridge
```
## Levantar el docker compose
-d es para que se ejecute en segundo plano para poder seguir usando la consola
```
sudo docker compose up -d
```
Una vez hecho esto ya podemos acceder al servicio introduciendo la ip del servidor y el puerto de 3000 que es el de react
## Comandos especiales para docker
> Eliminar todos los procesos de docker
```
sudo docker rm -f $(sudo docker ps -aq)
```
> Creear una red de contenedores dockers
```
sudo docker network create <nombre_red>
```
> Encender un contenedor
```
sudo docker run --network my_network -p 3000:3000 -d frontend_image
sudo docker run --network my_network -p 5000:5000 -d jsondb_image
```
> Eliminar todas las redes
```
sudo docker network prune -f
```

> Eliminar todas las imagenes
```
sudo docker rmi -f $(sudo docker images -q)
```
