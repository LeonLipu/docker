# saaadel/oraclelinux-java

FROM saaadel/oraclelinux-bash
MAINTAINER saaadel

# Java tar.gz URL and folder name inside
ARG javaurl=http://download.oracle.com/otn-pub/java/jdk/8u112-b15/jdk-8u112-linux-x64.tar.gz
ARG javadirname=jdk1.8.0_112

# Download and set up as default (no pipe for tar, because curl follows redirects)
RUN rm -rf /tmp/curl.tmp && \
    curl -o /tmp/curl.tmp \
    -jH "Cookie: oraclelicense=accept-securebackup-cookie" \
    -kL "${javaurl}" && \
    tar xzf /tmp/curl.tmp -C /opt && \
    rm -rf /tmp/curl.tmp && \
    /usr/sbin/alternatives --install /usr/bin/java java /opt/${javadirname}/bin/java 2 && \
    /usr/sbin/alternatives --install /usr/bin/jar jar /opt/${javadirname}/bin/jar 2 && \
    /usr/sbin/alternatives --install /usr/bin/javac javac /opt/${javadirname}/bin/javac 2 && \
    /usr/sbin/alternatives --set java /opt/${javadirname}/bin/java && \
    /usr/sbin/alternatives --set jar /opt/${javadirname}/bin/jar && \
    /usr/sbin/alternatives --set javac /opt/${javadirname}/bin/javac && \
    /bin/echo -e "\n\
JAVA_HOME=/opt/${javadirname}\n\
JDK_HOME=/opt/${javadirname}\n\
JRE_HOME=/opt/${javadirname}/jre\n\
export PATH=$PATH:/opt/${javadirname}/bin:/opt/${javadirname}/jre/bin\n\
" | tee -a /root/.profile /etc/skel/.profile

# Set up to use it in the Dockerfile
ENV JAVA_HOME=/opt/${javadirname} \
    JDK_HOME=/opt/${javadirname} \
    JRE_HOME=/opt/${javadirname}/jre \
    PATH=$PATH:/opt/${javadirname}/bin:/opt/${javadirname}/jre/bin

CMD ["java", "-version"]

