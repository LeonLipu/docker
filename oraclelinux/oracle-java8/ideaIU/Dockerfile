FROM saaadel/oraclelinux-vnc
MAINTAINER saaadel

# TODO replace wget with curl
# TODO add windows manager

USER root
ENV DISPLAY=""
WORKDIR ${HOME}

ARG jdktarfilename=jdk-8u111-linux-x64.tar.gz
ARG jdkdownloadurl=http://download.oracle.com/otn-pub/java/jdk/8u111-b14/${jdktarfilename}
ARG jdkdirname=jdk1.8.0_111

# From https://www.jetbrains.com/idea/download/download-thanks.html?platform=linux
ARG ideatarfilename=ideaIU-2016.3.1.tar.gz
ARG ideaurl=https://download.jetbrains.com/idea/${ideatarfilename}
ARG ideadirname=idea-IU-163.9166.29

RUN yum install -y --setopt=tsflags=nodocs \
        wget \
        which && \
        yum clean all && rm -rf /var/cache/yum/*

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
   /usr/bin/cat /root/.profile.java.template >> /etc/environment && \
   /usr/bin/cat /root/.profile.java.template >> ${HOME}/.profile

RUN wget --no-cookies --no-check-certificate \
         --directory-prefix=/opt/ \
         "${ideaurl}" && \
    tar xzf /opt/${ideatarfilename} -C /opt && \
    rm -rf /opt/${ideatarfilename}
    
RUN /bin/cp ${HOME}/.vnc/xstartup.template ${HOME}/.vnc/xstartup && \
    /bin/echo "/opt/${ideadirname}/bin/idea.sh" >> ${HOME}/.vnc/xstartup

RUN chown -R 1001:0 ${HOME} && \
    chmod 600 ${HOME}/.vnc/passwd && \
    chmod 775 ${HOME}/.vnc/xstartup

EXPOSE 5901
LABEL io.openshift.expose-services="5901:tcp"
USER ${newuser}
ENTRYPOINT ["/usr/bin/vncserver","-fg"]
