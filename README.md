# Practica-6-Docker
Practica 6 de la asignatura de despliegue

1.Hacemos pull a las imagenes que vamos a usar tanto para la base de datos como para la moodle
```
docker pull bitnami/mariadb
docker pull bitnami/moodle
```

2.Creamos una red en docker para interconectar los dos contenedores que vamos a crear
```
docker network create moodle-network
```

3.Creamos un volumen nuevo para cada uno de los contenedores
```
docker volume create --name mariadb_data
docker volume create --name moodle_data
```

4.Creamos primero el contenedor de mariadb con el siguiente comando
```
docker run -d --name mariadb \
  --env ALLOW_EMPTY_PASSWORD=yes \
  --env MARIADB_USER=moodle \
  --env MARIADB_PASSWORD=moodlefernando \
  --env MARIADB_DATABASE=moodle \
  --network moodle-network \
  --volume mariadb_data:/bitnami/mariadb \
  bitnami/mariadb:latest
```

5.Finalmente creamos el de moodle también, ahora deben estar los dos en ejecucion
```
docker run -d --name moodle \
  -p 8080:8080 -p 8443:8443 \
  --env ALLOW_EMPTY_PASSWORD=yes \
  --env MOODLE_DATABASE_USER=moodle \
  --env MOODLE_DATABASE_PASSWORD=moodlefernando \
  --env MOODLE_DATABASE_NAME=moodle \
  --network moodle-network \
  --volume moodle_data:/bitnami/moodle \
  bitnami/moodle:latest
```
![Contenedores en ejecución](https://i.imgur.com/jWQqlKa.png)
![Acceso al back-office](https://i.imgur.com/FIUBHtQ.png)
![Acceso al front-office](https://i.imgur.com/7XObEtf.png)

# Creación de imagenes de los contenedores
![Imagenes creadas](https://i.imgur.com/JuedYAU.png)
Guardamos las imagenes de modo que podamos subirlas a github (probablemente no deje por el limite de 100mb)
![Guardar imagenes como tar](https://i.imgur.com/24FFgNu.png)
