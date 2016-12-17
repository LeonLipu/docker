FROM oraclelinux
MAINTAINER saaadel

USER root
ENV HOME=/root
WORKDIR ${HOME}
    
ARG jdkdownloadurl=http://download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-linux-x64.tar.gz
ARG jdkdirname=jdk1.8.0_111

RUN rm -rf /opt/jdk.tmp.tar.gz && \
    curl -H "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" -k -L \
         -o /opt/jdk.tmp.tar.gz \
         "${jdkdownloadurl}" && \
    tar xzf /opt/jdk.tmp.tar.gz -C /opt && \
    rm -rf /opt/jdk.tmp.tar.gz

RUN /usr/sbin/alternatives --install /usr/bin/java java /opt/${jdkdirname}/bin/java 2 && \
    /usr/sbin/alternatives --install /usr/bin/jar jar /opt/${jdkdirname}/bin/jar 2 && \
    /usr/sbin/alternatives --install /usr/bin/javac javac /opt/${jdkdirname}/bin/javac 2 && \
    /usr/sbin/alternatives --set java /opt/${jdkdirname}/bin/java && \
    /usr/sbin/alternatives --set jar /opt/${jdkdirname}/bin/jar && \
    /usr/sbin/alternatives --set javac /opt/${jdkdirname}/bin/javac

# Generate java env variables copy
RUN /bin/echo -e "\n\
JAVA_HOME=/opt/${jdkdirname}\n\
JDK_HOME=/opt/${jdkdirname}\n\
JRE_HOME=/opt/${jdkdirname}/jre\n\
PATH=$PATH:/opt/${jdkdirname}/bin:/opt/${jdkdirname}/jre/bin\n\
" > /root/.profile.java.template && \
    /usr/bin/cat /root/.profile.java.template >> /etc/environment && \
    /usr/bin/cat /root/.profile.java.template >> /root/.profile

ENV JAVA_HOME=/opt/${jdkdirname} \
    JDK_HOME=/opt/${jdkdirname} \
    JRE_HOME=/opt/${jdkdirname} \
    PATH=$PATH:/opt/${jdkdirname}/bin:/opt/${jdkdirname}/jre/bin

RUN chown -R root:root ${HOME}
CMD ["java", "-version"]