FROM mathieubodin/jdk8:latest
MAINTAINER Mathieu BODIN <mathieu.bodin@gmail.com>

ENV TOMCAT_MAJOR_VERSION 8
ENV TOMCAT_VERSION 8.5.4

ENV CATALINA_HOME /opt/tomcat-"${TOMCAT_VERSION}"
ENV CATALINA_PORT_HTTP 8080

ENV JPDA_TRANSPORT=dt_socket
ENV JPDA_ADDRESS=5050

ENV JAVA_TOOL_OPTIONS="-Dfile.encoding=UTF8"

ENV JAVA_OPTS="-server -Xms1024m -Xmx4096m -Xss256k -Duser.language=fr -Duser.country=FR"

ENV PATH $CATALINA_HOME/bin:$PATH

RUN curl -s -S -G https://www.apache.org/dist/tomcat/tomcat-"${TOMCAT_MAJOR_VERSION}"/v"${TOMCAT_VERSION}"/bin/apache-tomcat-"${TOMCAT_VERSION}".tar.gz -o /opt/tomcat-"${TOMCAT_VERSION}".tar.gz \
	&& mkdir -p "${CATALINA_HOME}" \
	&& tar xzf /opt/tomcat-"${TOMCAT_VERSION}".tar.gz -C "${CATALINA_HOME}" --strip-components=1 \
	&& rm /opt/tomcat-"${TOMCAT_VERSION}".tar.gz

COPY catalina.properties server.xml tomcat-users.xml "${CATALINA_HOME}/conf/"

EXPOSE $CATALINA_PORT_HTTP $JPDA_ADDRESS

VOLUME ["${CATALINA_HOME}/shared", "${CATALINA_HOME}/logs", "${CATALINA_HOME}/webapps", "${CATALINA_HOME}/work", "${CATALINA_HOME}/temp"]

CMD [ "catalina.sh", "jpda", "run" ]