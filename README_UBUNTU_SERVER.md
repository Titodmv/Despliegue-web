# Prueba
## Instalar Docker
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
git clone <url> <ruta>
```
## Crear un dockerfile
```
#Importar imagen base del proyecto
FROM alpine:latest

#Comandos que se ejecutaran, necesarios para que funcione react
RUN sudo apt install node
RUN sudo apt install npm

#Directorio de trabajo
WORKDIR /aplicacion

#Copiamos el json para instalar las dependencias
COPY package.json ./

#Instalar las dependencias js
RUN npm install

#Copiar el projecto en la base del proyecto
COPY . .

#Puerto
EXPOSE 3000

#Encender el proyecto
CMD [ "npm", "start" ]
```
