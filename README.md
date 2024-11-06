# Instalar servicios principales webServer "nginx" servidor de aplicaciones "tomcat"
> Actualizar equipo
```
sudo apt-get update
sudo apt-get upgrade
```
> Instalar nodejs
```
sudo apt install nodejs
```
> Comprovar nodejs
```
node -v
```
> Instalar npm
```
sudo apt install npm
```
> Instalar Ngix
```
sudo apt install nginx
```
> Iniciar y habilitar Ngix
```
sudo systemctl start nginx
sudo systemctl enable nginx
```
> Instalar Java 11-jre
```
sudo apt install openjdk-11-jre-headless
```
> Instalar Tomcat
```
sudo apt-cache search tomcat
sudo apt install tomcat9 tomcat9-admin
```
> Escuchar y cambiar puerto
```
ss -ltn
sudo ufw allow from any to any port 8080 proto tcp
```


