# Ejercicio 2
## Generación de las imágenes
1. Creamos un fichero `docker-compose.yml` con los requisitos especificados para crear el proyecto.
2. Como tenemos que crear imágenes personalizadas a partir de los Dockerfile creados en el ejercicio 1, lanzamos el siguiente comando para que docker-compose las crea
    ```
    docker-compose build
    ```

## Levantamiento de contenedores
1. Para levantar los contenedores mediante docker-compose, lanzamos el siguiente comando desde el directorio donde se encuentre nuestro `docker-compose.yml`. 
    ```
    docker-compose --project-name lemoncode-docker up --build -d
    ```
    Se le pueden añadir parámetros específicos para que además de levantar los contenedores, haga más cosas:
    - Añadir `--build` para que cada vez que levantemos nuestros contenedores, se genere nuevas imágenes de nuestro proyecto.
    - Añadir `-d` para que docker-compose se ejecute en segundo plano.
    - Añadir `&` al final del comando para que la ejecución de docker-compose sea verbosa, además de permitirnos usar el terminal al finalizar la ejecución de docker-compose.
2. Comprobamos que nuestros contenedores se han levantado correctamente.
    ```
    docker-compose -p lemoncode-docker ps
    ```
3. Utilizamos nuestro proyecto levantado desde la dirección de localhost:3000 cuanto queramos. Cuando hayamos acabado, paramos los contenedores.
    ```
    docker-compose -p lemoncode-docker stop
    ```
4. Y si quisieramos eliminarnos una vez parados.
    ```
    docker-compose -p lemoncode-docker rm
    ```
    > De manera opcional, si queremos parar y eliminar todo de una vez podríamos usar
    > ```
    > docker-compose -p lemoncode-docker down
    > ```
