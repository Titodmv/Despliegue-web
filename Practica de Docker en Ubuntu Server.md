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
## Hacer build desde la carpeta del proyecto
```
sudo docker build -t <nombre de la imagen> .
```
## Correr la imagen
```
sudo docker run -p <puertoMaquina>:<puertoDelProyecto> <imagen del proyecto>
```
## Comandos especiales para docker
### Eliminar todos los procesos de docker
```
sudo docker rm -f $(sudo docker ps -aq)
```
### Creear una red de contenedores dockers
```
sudo docker network create <nombre_red>
```
```
sudo docker run --network my_network -p 3000:3000 -d frontend_image
sudo docker run --network my_network -p 5000:5000 -d jsondb_image
```
### Eliminar todas las redes
```
sudo docker network prune -f
```

### Eliminar todas las imagenes
```
sudo docker rmi -f $(sudo docker images -q)
```

