FROM centos:7.2.1511
MAINTAINER Mathieu BODIN <mathieu.bodin@gmail.com>

ENV JAVA_VERSION 8
ENV JAVA_UPDATE 91
ENV JAVA_BUILD 14

ENV JAVA_HOME "/opt/jdk1.${JAVA_VERSION}.0_${JAVA_UPDATE}"
ENV JRE_HOME "${JAVA_HOME}/jre"

RUN yum install wget -y -q\
	&& wget -q -O /opt/jdk-"${JAVA_VERSION}"u"${JAVA_UPDATE}".tar.gz --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz"\
	&& tar xzf /opt/jdk-"${JAVA_VERSION}"u"${JAVA_UPDATE}".tar.gz -C /opt\
	&& rm /opt/jdk-"${JAVA_VERSION}"u"${JAVA_UPDATE}".tar.gz

RUN alternatives --install /usr/bin/java java "${JAVA_HOME}"/bin/java 2\
	&& alternatives --install /usr/bin/jar jar "${JAVA_HOME}"/bin/jar 2\
	&& alternatives --install /usr/bin/javac javac "${JAVA_HOME}"/bin/javac 2\
	&& alternatives --set java "${JAVA_HOME}"/bin/java\
	&& alternatives --set jar "${JAVA_HOME}"/bin/jar\
	&& alternatives --set javac "${JAVA_HOME}"/bin/javac

RUN echo "export PATH=$PATH:${JAVA_HOME}/bin:${JRE_HOME}/bin" >> /etc/environment

CMD ["/bin/bash"]