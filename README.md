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
<br>
Efectivamente, no nos deja subirlas:
```
root@iescamas:/home/fpalm/Practica-6-Docker# git commit -m "Subidas las imagenes de docker"
[main 2607111] Subidas las imagenes de docker
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 mariadb_fernando.tar.gz
 create mode 100644 moodle_fernando.tar.gz
root@iescamas:/home/fpalm/Practica-6-Docker# git push
  Username for 'https://github.com': ********************
  Password for 'https://*******@github.com':
  Enumerating objects: 5, done.
  Counting objects: 100% (5/5), done.
  warning: suboptimal pack - out of memory
  Compressing objects: 100% (4/4), done.
  error: RPC failed; curl 92 HTTP/2 stream 0 was not closed cleanly: CANCEL (err 8)
  fatal: the remote end hung up unexpectedly
  Writing objects: 100% (4/4), 396.85 MiB | 14.50 MiB/s, done.
  Total 4 (delta 0), reused 0 (delta 0)
  fatal: the remote end hung up unexpectedly
  Everything up-to-date
root@iescamas:/home/fpalm/Practica-6-Docker#
```
