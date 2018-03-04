# Mes Dockerfiles

Pour construire l'image Docker - par exemple `tomcat` - il faut utiliser la commande suivante :

```sh
docker build --rm -f tomcat/8.0/Dockerfile -t mathieubodin/tomcat:8.0 tomcat/8.0
```

Une fois construite, l'image est utilisable pour initier un `container` Docker par la commande suivante :

```bash
docker run -t -i mathieubodin/tomcat:8.0
```

## nginx

L'image est basée sur centos:7.2.1511.

Nginx est installé depuis les sources, ce qui explique la taille de l'image.

### 1.10.1

| Variable d'environnement | Valeur par défaut |
| :----------------------- | ----------------- |
| NGINX_VERSION            | 1.10.1            |
| NGINX_HOME               | /opt/nginx-1.10.1 |

## tomcat

### 8.0

La version de Tomcat installée est la [8.0.50](http://tomcat.apache.org/tomcat-8.0-doc/index.html).