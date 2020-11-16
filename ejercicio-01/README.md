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
    docker run --name database \
    --network lemoncode-challenge \
    -p 27017:27017 \
    --mount source=lemoncode-data,target=/data/db \
    -d mongo
    ```
3. Para asegurarnos que se ha creado correctamente, abrimos MongoDB Compass y probamos la siguiente conexión:
    ```
    mongodb://localhost:27017 
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
    $ docker -d run --name api \
    -p 5000:80 \
    --network lemoncode-challenge \
    -e MONGO_URI=mongodb://database:27017  \
    backend-aspnet
    ```
3. Para comprobar que hemos tenido éxito, comprobamos que existe el contenedor en la network de `lemoncode-challenge`:
    ```
    docker network inspect lemoncode-challenge
    ```
4. Deberiamos poder acceder a la api directamente desde el navegador desde la dirección https://localhost:5000/api/topics
## Frontend
1. Hacemos una build del Dockerfile que hemos creado en la carpeta de frontend y creamos un contenedor con él. Partiendo del directorio de backend en el que nos quedamos:
    ```
    $ cd ../../frontend
    $ docker build -t frontend-react .
    $ docker run -itd --name ui \
    -p 3000:80 \
    -e REACT_APP_API_URL=https://localhost:5000/api/topics \
    frontend-react
    ```
2. Para comprobar que nuestro frontend está corriendo sin problemas, tenemos que averiguar su ip. El cual se puede obtener inspeccionando la red de `lemoncode-challenge` con los filtrados oportunos:
    ```
    docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ui
    ```
3. Ponemos la IP obtenida en el navegador y debería salir la página funcionando.