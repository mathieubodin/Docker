FROM alpine:latest as prepare-install

LABEL MAINTAINER="mathieu.bodin@gmail.com"

RUN apk add --no-cache --virtual=build-dependencies curl unzip

RUN mkdir -p /tomcat

# Install Java
RUN curl -L -S -s -b "oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u161-b12/2f38c3b165be4555a1fa6e98c45e0808/jdk-8u161-linux-x64.tar.gz" -o /jdk-8u161-linux-x64.tar.gz
RUN tar xzf /jdk-8u161-linux-x64.tar.gz -C /tomcat

RUN curl -L -S -s -b "oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip" -o /jce_policy-8.zip
RUN unzip -jo -d /tomcat/jdk1.8.0_161/jre/lib/security /jce_policy-8.zip

# Install Tomcat
RUN curl -s -S -G https://www.apache.org/dist/tomcat/tomcat-8/v8.0.50/bin/apache-tomcat-8.0.50.tar.gz -o /apache-tomcat-8.0.50.tar.gz
RUN tar xzf /apache-tomcat-8.0.50.tar.gz -C /tomcat

# Configure Tomcat
COPY tomcat-users.xml /tomcat/apache-tomcat-8.0.50/conf

# Clean up
RUN apk del --no-cache build-dependencies
RUN rm /jdk-8u161-linux-x64.tar.gz /apache-tomcat-8.0.50.tar.gz /jce_policy-8.zip
RUN rm -rf /tomcat/jdk1.8.0_161/*src.zip \
           /tomcat/jdk1.8.0_161/lib/missioncontrol \
           /tomcat/jdk1.8.0_161/lib/visualvm \
           /tomcat/jdk1.8.0_161/lib/*javafx* \
           /tomcat/jdk1.8.0_161/jre/lib/security/README.txt \
           /tomcat/jdk1.8.0_161/jre/lib/plugin.jar \
           /tomcat/jdk1.8.0_161/jre/lib/ext/jfxrt.jar \
           /tomcat/jdk1.8.0_161/jre/bin/javaws \
           /tomcat/jdk1.8.0_161/jre/lib/javaws.jar \
           /tomcat/jdk1.8.0_161/jre/lib/desktop \
           /tomcat/jdk1.8.0_161/jre/plugin \
           /tomcat/jdk1.8.0_161/jre/lib/deploy* \
           /tomcat/jdk1.8.0_161/jre/lib/*javafx* \
           /tomcat/jdk1.8.0_161/jre/lib/*jfx* \
           /tomcat/jdk1.8.0_161/jre/lib/amd64/libdecora_sse.so \
           /tomcat/jdk1.8.0_161/jre/lib/amd64/libprism_*.so \
           /tomcat/jdk1.8.0_161/jre/lib/amd64/libfxplugins.so \
           /tomcat/jdk1.8.0_161/jre/lib/amd64/libglass.so \
           /tomcat/jdk1.8.0_161/jre/lib/amd64/libgstreamer-lite.so \
           /tomcat/jdk1.8.0_161/jre/lib/amd64/libjavafx*.so \
           /tomcat/jdk1.8.0_161/jre/lib/amd64/libjfx*.so \
           /tomcat/jdk1.8.0_161/jre/bin/jjs \
           /tomcat/jdk1.8.0_161/jre/bin/keytool \
           /tomcat/jdk1.8.0_161/jre/bin/orbd \
           /tomcat/jdk1.8.0_161/jre/bin/pack200 \
           /tomcat/jdk1.8.0_161/jre/bin/policytool \
           /tomcat/jdk1.8.0_161/jre/bin/rmid \
           /tomcat/jdk1.8.0_161/jre/bin/rmiregistry \
           /tomcat/jdk1.8.0_161/jre/bin/servertool \
           /tomcat/jdk1.8.0_161/jre/bin/tnameserv \
           /tomcat/jdk1.8.0_161/jre/bin/unpack200 \
           /tomcat/jdk1.8.0_161/jre/lib/ext/nashorn.jar \
           /tomcat/jdk1.8.0_161/jre/lib/jfr.jar \
           /tomcat/jdk1.8.0_161/jre/lib/jfr \
           /tomcat/jdk1.8.0_161/jre/lib/oblique-fonts \
           /tomcat/apache-tomcat-8.0.50/bin/*.bat \
           /tomcat/apache-tomcat-8.0.50/webapps/docs \
           /tomcat/apache-tomcat-8.0.50/webapps/ROOT \
           /tomcat/apache-tomcat-8.0.50/webapps/examples \
           /tomcat/apache-tomcat-8.0.50/webapps/host-manager

FROM gcr.io/distroless/base

COPY --from=prepare-install /tomcat /

EXPOSE 8000 8005 8009 8080

ENTRYPOINT [ "/jdk1.8.0_161/bin/java", "-Djava.util.logging.config.file=/apache-tomcat-8.0.50/conf/logging.properties",	"-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager", "-Djdk.tls.ephemeralDHKeySize=2048","-Djava.protocol.handler.pkgs=org.apache.catalina.webresources", "-agentlib:jdwp=transport=dt_socket,address=127.0.0.1:8000,server=y,suspend=n", "-classpath", "/apache-tomcat-8.0.50/bin/bootstrap.jar:/apache-tomcat-8.0.50/bin/tomcat-juli.jar","-Dcatalina.base=/apache-tomcat-8.0.50", "-Dcatalina.home=/apache-tomcat-8.0.50", "-Dcatalina.admin.password=","-Djava.io.tmpdir=/apache-tomcat-8.0.50/temp", "org.apache.catalina.startup.Bootstrap","start" ]