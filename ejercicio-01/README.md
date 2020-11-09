# Ejercicio 1
## Primeros pasos
1. Creamos la network
    ```
    docker network create lemoncode-challenge
    ```
## Base de datos
1. Creamos un volumen para la base de datos
    ```
    docker volume create lemoncode-data
    ```
2. Creamos contenedor de MongoDB para la base de datos
    ```
    docker run --name mongodb \
    -p 27017:27017 \
    --network lemoncode-challenge \
    --mount source=lemoncode-data,target=/data/db \
    -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
    -e MONGO_INITDB_ROOT_PASSWORD=secret \
    -d mongo
    ```
3. Para asegurarnos que se ha creado correctamente, abrimos MongoDB Compass y probamos la siguiente conexión:
    ```
    mongodb://mongoadmin:secret@localhost:27017 
    ```
## Backend
1. Pulleamos la imagen de backend a utilizar
    ```
    docker pull mcr.microsoft.com/dotnet/core/sdk:3.1
    ```
2. Hacemos una build del Dockerfile que hemos creado en la carpeta de backend y creamos un contenedor con él
    ```
    $ cd backend/backend
    $ docker build -t backend-aspnet .
    $ docker run -d -p 5000:80 --network lemoncode-challenge --name api backend-aspnet
    ```
3. Para comprobar que hemos tenido éxito, comprobamos que existe el contenedor en la network de `lemoncode-challenge`:
    ```
    docker network inspect lemoncode-challenge
    ```
## Frontend
1. Creamos un Dockerfile, en este caso con multistage, para nuestro front y creamos un contenedor con él. Partiendo del directorio de backend en el que nos quedamos:
    ```
    $ cd ../../frontend
    $ docker build -t frontend-lemoncode-challenge .
    $ docker run -d -p 3000:80 --network lemoncode-challenge --name frontend frontend-lemoncode-challenge
    ```