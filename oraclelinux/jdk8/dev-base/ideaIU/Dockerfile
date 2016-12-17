FROM saaadel/oraclelinux-java-dev-base
MAINTAINER saaadel

USER root
ENV DISPLAY=""
WORKDIR ${HOME}

# From https://www.jetbrains.com/idea/download/download-thanks.html?platform=linux
ARG ideaurl=https://download.jetbrains.com/idea/ideaIU-2016.3.1.tar.gz
ARG ideadirname=idea-IU-163.9166.29

RUN yum install -y --setopt=tsflags=nodocs \
        which && \
        yum clean all && rm -rf /var/cache/yum/*

RUN rm -rf /opt/ideaIU.tmp.tar.gz && \
    curl -k -L -o /opt/ideaIU.tmp.tar.gz \
         "${ideaurl}" && \
    tar xzf /opt/ideaIU.tmp.tar.gz -C /opt && \
    rm -rf /opt/ideaIU.tmp.tar.gz
    
ENTRYPOINT ["/opt/${ideadirname}/bin/idea.sh"]
