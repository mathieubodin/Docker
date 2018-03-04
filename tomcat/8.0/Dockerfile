FROM alpine:latest as prepare-install

LABEL MAINTAINER="mathieu.bodin@gmail.com"

RUN apk add --no-cache curl

RUN mkdir -p /tomcat

# Install Java
RUN curl -L -S -s -b "oraclelicense=a" "http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.tar.gz" -o /jdk-8u161-linux-x64.tar.gz
RUN tar xzf /jdk-8u161-linux-x64.tar.gz -C /tomcat

# Install Tomcat
RUN curl -s -S -G https://www.apache.org/dist/tomcat/tomcat-8/v8.0.50/bin/apache-tomcat-8.0.50.tar.gz -o /apache-tomcat-8.0.50.tar.gz
RUN tar xzf /apache-tomcat-8.0.50.tar.gz -C /tomcat

# Configure Tomcat
COPY tomcat-users.xml /tomcat/apache-tomcat-8.0.50/conf

# Clean up
RUN apk del --no-cache curl
RUN rm /jdk-8u161-linux-x64.tar.gz
RUN rm /apache-tomcat-8.0.50.tar.gz
RUN rm /tomcat/apache-tomcat-8.0.50/bin/*.bat
RUN rm -rf /tomcat/apache-tomcat-8.0.50/webapps/docs
RUN rm -rf /tomcat/apache-tomcat-8.0.50/webapps/ROOT
RUN rm -rf /tomcat/apache-tomcat-8.0.50/webapps/examples
RUN rm -rf /tomcat/apache-tomcat-8.0.50/webapps/host-manager

FROM gcr.io/distroless/base

COPY --from=prepare-install /tomcat /

EXPOSE 8000 8005 8009 8080

ENTRYPOINT [ "/jdk1.8.0_161/bin/java", "-Djava.util.logging.config.file=/apache-tomcat-8.0.50/conf/logging.properties",	"-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager", "-Djdk.tls.ephemeralDHKeySize=2048","-Djava.protocol.handler.pkgs=org.apache.catalina.webresources", "-agentlib:jdwp=transport=dt_socket,address=127.0.0.1:8000,server=y,suspend=n", "-classpath", "/apache-tomcat-8.0.50/bin/bootstrap.jar:/apache-tomcat-8.0.50/bin/tomcat-juli.jar","-Dcatalina.base=/apache-tomcat-8.0.50", "-Dcatalina.home=/apache-tomcat-8.0.50", "-Dcatalina.admin.password=","-Djava.io.tmpdir=/apache-tomcat-8.0.50/temp", "org.apache.catalina.startup.Bootstrap","start" ]