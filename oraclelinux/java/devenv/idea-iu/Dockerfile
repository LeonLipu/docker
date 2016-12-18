# saaadel/oraclelinux-idea-iu

FROM saaadel/oraclelinux-devenv
MAINTAINER saaadel

# From https://www.jetbrains.com/idea/download/download-thanks.html?platform=linux
ARG ideaurl=https://download.jetbrains.com/idea/ideaIU-2016.3.1.tar.gz
ARG ideadirname=idea-IU-163.9166.29

RUN rm -rf /tmp/ideaIU.tmp && \
    curl -jkL -o /tmp/ideaIU.tmp \
         "${ideaurl}" && \
    tar xzf /tmp/ideaIU.tmp -C /opt && \
    rm -rf /tmp/ideaIU.tmp && \

CMD /opt/${ideadirname}/bin/idea.sh