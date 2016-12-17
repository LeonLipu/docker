FROM oraclelinux
MAINTAINER saaadel

USER root
ENV DISPLAY="" \
    HOME=/root
    
ARG jdktarfilename=jdk-8u111-linux-x64.tar.gz
ARG jdkdownloadurl=http://download.oracle.com/otn-pub/java/jdk/8u111-b14/${jdktarfilename}
ARG jdkdirname=jdk1.8.0_111

RUN wget --no-cookies --no-check-certificate \
         --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
         --directory-prefix=/opt/ \
         "${jdkdownloadurl}" && \
    tar xzf /opt/${jdktarfilename} -C /opt && \
    rm -rf /opt/${jdktarfilename}

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
   /usr/bin/cat /root/.profile.java.template >> /root/.profile && \
   /usr/bin/cat /root/.profile.java.template >> /etc/environment

RUN chown -R root:root ${HOME}
CMD ["java", "-version"]
