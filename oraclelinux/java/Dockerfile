# saaadel/oraclelinux-java

FROM saaadel/oraclelinux-bash
MAINTAINER saaadel

# Java tar.gz URL and folder name inside
ARG javaurl=http://download.oracle.com/otn/java/jdk/7u80-b15/jre-7u80-linux-x64.tar.gz
ARG javadirname=jre1.7.0_80

# Download and set up as default (no pipe for tar, because curl follows redirects)
RUN rm -rf /tmp/curl.tmp && \
    curl -o /tmp/curl.tmp \
    -H "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2Ftechnetwork%2Fjava%2Fjavase%2Fdownloads%2Fjava-archive-downloads-javase7-521261.html; oraclelicense=accept-securebackup-cookie" \
    -k -L "${javaurl}" && \
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
JRE_HOME=/opt/${javadirname}\n\
export PATH=$PATH:/opt/${javadirname}/bin\n\
" | tee -a /root/.profile /etc/skel/.profile

# Set up to use it in the Dockerfile
ENV JAVA_HOME=/opt/${javadirname} \
    JRE_HOME=/opt/${javadirname} \
    PATH=$PATH:/opt/${javadirname}/bin

CMD ["java", "-version"]

