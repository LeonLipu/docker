FROM saaadel/oraclelinux-java
MAINTAINER saaadel

ARG mavenurl=http://ftp.byfly.by/pub/apache.org/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
ARG mavendirname=apache-maven-3.3.9

ARG gradleurl=https://services.gradle.org/distributions/gradle-3.2.1-bin.zip
ARG gradledirname=gradle-3.2.1

USER root
#ENV HOME=/root
#WORKDIR ${HOME}

RUN yum install -y --setopt=tsflags=nodocs \
        git subversion \
        bash unzip && \
        yum clean all && rm -rf /var/cache/yum/*

RUN curl --silent --location https://rpm.nodesource.com/setup_7.x | bash - && \
    yum install -y --setopt=tsflags=nodocs \
        nodejs npm && \
        yum clean all && rm -rf /var/cache/yum/* && \
    npm install -g bower grunt-cli gulp-cli

RUN rm -rf /opt/maven.tmp.tar.gz && \
    curl -k -L -o /opt/maven.tmp.tar.gz \
         "${mavenurl}" && \
    tar xzf /opt/maven.tmp.tar.gz -C /opt && \
    rm -rf /opt/maven.tmp.tar.gz

RUN rm -rf /opt/gradle.tmp.zip && \
    curl -k -L -o /opt/gradle.tmp.zip \
         "${gradleurl}" && \
    unzip /opt/gradle.tmp.zip -d /opt && \
    rm -rf /opt/gradle.tmp.zip

# Generate java env variables copy
RUN /bin/echo -e "\n\
M2_HOME=/opt/${mavendirname}/\n\
GRADLE_HOME=/opt/${gradledirname}/\n\
PATH=$PATH:/opt/${mavendirname}/bin:/opt/${gradledirname}/bin\n\
" > /root/.profile.m2-gradle.template && \
    /usr/bin/cat /root/.profile.m2-gradle.template >> /etc/environment && \
    /usr/bin/cat /root/.profile.m2-gradle.template >> /root/.profile

RUN chown -R root:root ${HOME}
CMD ["mvn", "-v"]

