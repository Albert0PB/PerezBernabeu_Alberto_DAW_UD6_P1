# Práctica 6.1 - Dockerización del despliegue de una aplicación con Node.js

## Despliegue con Docker

Para realizar esta práctica de despliegue con Docker será 
necesario tener disponible una aplicación que desplegar en primer lugar.
Para ello, clonaré un repositorio de GitHub ejecutando:

```console
git clone https://github.com/raul-profesor/DAW_practica_6.1_2024.git
```

Este repositorio ya contiene un documento Dockerfile, pero es necesario 
modificar su contenido para poder realizar el despliegue correctamente. 

En este caso, el contenido de este archivo será:

```console
FROM node:18.16.0-alpine3.17
RUN  mkdir -p /opt/app
WORKDIR /opt/app
COPY src/package.json src/package-lock.json .
RUN npm install
COPY src/ .
EXPOSE 3000
CMD [ "npm", "start"]
```

|Comando|Explicación|
|-------|-----------|
|FROM node:18.16.0-alpine3.17|Indica que las imágenes base de esta aplicación serán la imágenes oficiales de Node y Alpine Linux en Docker Hub|
|RUN  mkdir -p /opt/app|Se crea el directorio /opt/app dentro de una nueva capa de la imagen base que estamos utilizando|
|WORKDIR /opt/app|Establece '/opt/app/ como el directorio de trabajo a utilizar. Todas las instrucciones se ejecutarán desde esta ruta a partir de ahora|
|COPY src/package.json src/package-lock.json .|Se copian estos archivos de la máquina local a la imagen de Docker|
|RUN npm install|Se ejecuta 'npm install' dentro de la imagen de Docker|
|COPY src/ .|Se copian el resto de contenidos de src/ (de la máquina local) a la imagen de Docker|
|EXPOSE 3000|Indica que el contenedor, en tiempo de ejecución, debe 'escuchar' en el puerto 3000|
|CMD [ "npm", "start"]|Indica que el contenedor, al ser arrancado, debe ejecutar 'npm start', lo que hará que arranque la ejecución de la aplicación|


Con nuestro archivo Dockerfile preparado, podemos 'construir' la imagen de 
Docker. Para ello sólo hay que ejecutar: 

```console
docker build -t librodirecciones .
```

Para ejecutar la aplicación 'dockerizada' y comprobar su buen funcionamiento, 
hay que ejecutar: 

```console
$ docker run -p 3000:3000 -d librodirecciones
```

Si accedemos al puerto 3000, tendremos acceso a una referencia de la API 
de la aplicación: 

![Referencia API](./images/referencia_api.png)

Pero aún no será posible hacer uso completo de la aplicación, ya que no 
hay configurado un sistema de persistencia de datos.

## Docker Compose


