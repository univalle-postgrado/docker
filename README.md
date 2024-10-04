# Ejemplos de uso con Docker y docker compose

## 1. Para crear el contenedor desde consola

`docker pull nginx:alpine`

`docker create nginx`

`docker pull mongo`

`docker create mongo`

`docker create -p 8080:80 nginx` // port mapping, donde -p significa publish

`docker create -p 80 nginx` // docker escoge el puerto

`docker ps`

`docker ps -a` // Muestra todos los contenedores

`docker create --name ejemplo1_mongo mongo`

`docker create -p 8080:80 --name ejemplo1_nginx_1 nginx`

`docker logs ejemplo1_nginx_1` // para ver los logs

`docker logs --follow ejemplo1_nginx_1` // para ver los logs en primer plano


`docker run nginx` // un comando para hacer todo en uno. 1. Verifica si la imagen existe y si no lo descarga, 2. Crea el contenedor, 3. Inicia el contenedor

`docker run -d -p 8080:80 --name ejemplo1_nginx_2 -v $(pwd)/index.html:/usr/share/nginx/html/index.html nginx:alpine` // -d es detached, en 2do plano

## 2. Para ejecutar el ejemplo 1:

`docker build -t ejemplo2_nginx:v1 .`

`docker run -d -p 8080:80 --name ejemplo2_nginx ejemplo2_nginx:v1`

## 3. Crear un balanceador con contenedores

`docker run -d -p 8081:80 --rm --name ejemplo3_1 -v $(pwd)/index.1.html:/usr/share/nginx/html/index.html nginx:alpine`

`docker run -d -p 8082:80 --rm --name ejemplo3_2 -v $(pwd)/index.2.html:/usr/share/nginx/html/index.html nginx:alpine`

`docker run -d -p 8083:80 --rm --name ejemplo3_3 -v $(pwd)/index.3.html:/usr/share/nginx/html/index.html nginx:alpine`

`docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ejemplo3_1`

`docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ejemplo3_2`

`docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ejemplo3_3`

`docker run -d -p 8080:80 --rm --name ejemplo3_0 -v $(pwd)/nginx.conf:/etc/nginx/conf.d/default.conf nginx:alpine`

## 4. Crear un balanceador con contenedores con docker compose

`docker compose up -d`

## 5. Ejemplo de Wordpress con MySQL

`docker compose up -d`

Abre tu navegador en el navegador: http://localhost:8090

## 6. Aplicación en node conectandose a MongoDB

- Creamos un contenedor de mongo y lo iniciamos

`docker create -p 27017:27017 --name ejemplo6_mongodb -e MONGO_INITDB_ROOT_USERNAME=marcelo -e MONGO_INITDB_ROOT_PASSWORD=secret amd64/mongo`

`docker start ejemplo6_mongodb`

// Si queremos que los contenedores se vean entre si y no sea necesario hacer portmapping de mongo

`docker network ls`

`docker network create ejemplo6_network`

`docker build -t ejemplo6_node:v1 .`   // -t el nombre de la imagen

`docker images`

`docker create -p 3000:3000 --name ejemplo6_node --network ejemplo6_network ejemplo6_node:v1`

`docker run -p 27017:27017 -d --name ejemplo6_mongodb -e MONGO_INITDB_ROOT_USERNAME=marcelo -e MONGO_INITDB_ROOT_PASSWORD=secret --network ejemplo6_network amd64/mongo`

`docker start ejemplo6_node`

`docker logs ejemplo6_node`

## 7. El mismo ejemplo anterior pero con docker compose

`docker compose up`

// luego ctrl+C para el contener de una manera delicada


`docker compose up -d` // en segundo plano

`docker compose down` // borra todos los contenedores

## 8. Preparando varios ambientes para trabajar (produccion y desarrollo) utilizando hot reload

`docker compose up -d`

`docker compose -f docker-compose-dev.yml up -d`

`docker logs --follow ejemplo8-node-1`

## 9. Gestor Mongo Express para administrar BDs Mongo

`docker compose up -d`

`docker compose down -v`

## 10. Gestor adminer para administrar BDs PostgreSQL

`docker compose up -d`

`docker compose down -v`

## 10. Ejemplo de conexión de expressjs con PostgreSQL con script de la estructura de la BD

`docker compose up -d`

`docker compose down -v`