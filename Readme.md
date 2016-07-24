# Mes Dockerfiles

Pour construire l'image Docker - par exemple `jdk8` - il faut utiliser la commande suivante :

```sh
docker build -t mathieubodin/jdk8:latest jdk8/
```

Une fois construite, l'image est utilisable pour initier un `container` Docker par la commande suivante :

```bash
docker run -t -i mathieubodin/jdk8:latest
```

## jdk8

TBD

## tomcat

L'image est basée sur mathieubodin/jdk8:latest.

Tomcat est configuré pour supporter des ressources partagées dans ```shared/classes``` et ```shared/lib```.

Tomcat est démaré en mode DEBUG à distance par la commande :

```bash
catalina.sh jpda run
```

### 7.0

La version de Tomcat installée est la [7.0.70](http://tomcat.apache.org/tomcat-7.0-doc/index.html), configurée comme suit :

| Variable d'environnement | Valeur par défaut                                                         |
| :---------------------- | ------------------------------------------------------------------------- |
| TOMCAT_MAJOR_VERSION    | 7                                                                         |
| TOMCAT_VERSION          | 7.0.70                                                                    |
| CATALINA_HOME           | /opt/tomcat-7.0.70                                                        |
| CATALINA_PORT_HTTP      | 8080                                                                      |
| JPDA_TRANSPORT          | dt_socket                                                                 |
| JPDA_ADDRESS            | 5050                                                                      |
| JAVA_TOOL_OPTIONS       | -Dfile.encoding=UTF8                                                      |
| JAVA_OPTS               | -server -Xms1024m -Xmx4096m -Xss256k -Duser.language=fr -Duser.country=FR |
| PATH                    | $CATALINA_HOME/bin:$PATH                                                  |

### 8.0

La version de Tomcat installée est la [8.0.36](http://tomcat.apache.org/tomcat-8.0-doc/index.html), configurée comme suit :

| Variable d'environnement | Valeur par défaut                                                         |
| :---------------------- | ------------------------------------------------------------------------- |
| TOMCAT_MAJOR_VERSION    | 8                                                                         |
| TOMCAT_VERSION          | 8.0.36                                                                    |
| CATALINA_HOME           | /opt/tomcat-8.0.36                                                        |
| CATALINA_PORT_HTTP      | 8080                                                                      |
| JPDA_TRANSPORT          | dt_socket                                                                 |
| JPDA_ADDRESS            | 5050                                                                      |
| JAVA_TOOL_OPTIONS       | -Dfile.encoding=UTF8                                                      |
| JAVA_OPTS               | -server -Xms1024m -Xmx4096m -Xss256k -Duser.language=fr -Duser.country=FR |
| PATH                    | $CATALINA_HOME/bin:$PATH                                                  |

### 8.5

L'image est basée sur mathieubodin/jdk8:latest.

La version de Tomcat installée est la [8.5.4](http://tomcat.apache.org/tomcat-8.5-doc/index.html), configurée comme suit :

| Variable d'environnement | Valeur par défaut                                                         |
| :---------------------- | ------------------------------------------------------------------------- |
| TOMCAT_MAJOR_VERSION    | 8                                                                         |
| TOMCAT_VERSION          | 8.5.4                                                                     |
| CATALINA_HOME           | /opt/tomcat-8.5.4                                                         |
| CATALINA_PORT_HTTP      | 8080                                                                      |
| JPDA_TRANSPORT          | dt_socket                                                                 |
| JPDA_ADDRESS            | 5050                                                                      |
| JAVA_TOOL_OPTIONS       | -Dfile.encoding=UTF8                                                      |
| JAVA_OPTS               | -server -Xms1024m -Xmx4096m -Xss256k -Duser.language=fr -Duser.country=FR |
| PATH                    | $CATALINA_HOME/bin:$PATH                                                  |

### Exemple d'utilisation

Pour utiliser un des ces images afin d'y déployer des war Java, il faut préparer un peu le terrain.

Soit qu'on souhaite déployer l'application *mon_application.war*, on commencera par créer quelque part le répertoire ```mon_application```. Puis on positionnera la ligne de commande dans ce répertoire.

Jouer cette commande pour créer les répertoires qui seront utilisés par le container Docker.

```bash
mkdir -p ./logs ./shared ./webapps ./temp ./work
```

Jouer cette commande pour créer le container Docker sur la base de l'image (ici 7.0.70) :

```bash
docker create -ti -p 8080:8080 -p 5050:5050 --name mon_application \
-v ./logs:/opt/tomcat-7.0.70/logs \
-v ./shared:/opt/tomcat-7.0.70/shared \
-v ./webapps:/opt/tomcat-7.0.70/webapps \
-v ./temp:/opt/tomcat-7.0.70/temp \
-v ./work:/opt/tomcat-7.0.70/work \
mathieubodin/tomcat:7.0.70
```