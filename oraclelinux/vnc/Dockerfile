FROM oraclelinux
MAINTAINER saaadel

ARG vncpassword=
ARG newuser=user
ARG newuid=1001
ARG sudoersgroup=wheel

SHELL ["/bin/bash", "-c"]
USER root
ENV DISPLAY="" \
    HOME=/home/${newuser}
WORKDIR ${HOME}

RUN yum clean all && \
    yum update -y && \
    yum install -y --setopt=tsflags=nodocs \
        sudo \
        bash && \
        yum clean all && rm -rf /var/cache/yum/*

RUN /bin/echo -e "\n\
if [ -f ~/.bashrc ]; then\n\
   source ~/.bashrc\n\
fi\n\
" > /root/.bash_profile && \
   /bin/echo -e "\n\
alias ls='ls --color=auto'\n\
alias ll='ls -la'\n\
alias l.='ls -d .* --color=auto'\n\
\n\
alias grep='grep --color=auto'\n\
alias egrep='egrep --color=auto'\n\
alias fgrep='fgrep --color=auto'\n\
\n\
alias chown='chown --preserve-root'\n\
alias chmod='chmod --preserve-root'\n\
alias chgrp='chgrp --preserve-root'\n\
\n\
" > /root/.bashrc

RUN yum install -y --setopt=tsflags=nodocs \
        xorg-x11-fonts-* xorg-x11-server-utils xorg-x11-server-Xvfb \
        xterm \
        gnome-session \
        tigervnc-server && \
        yum clean all && rm -rf /var/cache/yum/*

RUN /bin/dbus-uuidgen --ensure

RUN useradd -u ${newuid} -r -g 0 -d ${HOME} -s /bin/bash ${newuser} && \
    passwd -d ${newuser} && \
    usermod -a -G ${sudoersgroup} ${newuser} && \
    cp /root/.bash_profile ${HOME}/.bash_profile && \
    cp /root/.bashrc ${HOME}/.bashrc

RUN ( ( [ ! -d ${HOME}/.vnc ] && mkdir -p ${HOME}/.vnc ) || true ) && \
    /bin/echo -e "#!/bin/sh \n\
        [ -r /etc/sysconfig/i18n ] && . /etc/sysconfig/i18n \n\
        export LANG \n\
        export SYSFONT \n\
        vncconfig -nowin & \n\
        unset SESSION_MANAGER \n\
        unset DBUS_SESSION_BUS_ADDRESS \n\
        unset DBUS_SESSION_ADDRESS \n\
        OS=`uname -s` \n\
        gnome-session& \n\
    " >> ${HOME}/.vnc/xstartup.template && \
    /bin/cp ${HOME}/.vnc/xstartup.template ${HOME}/.vnc/xstartup && \
    /bin/echo "/usr/bin/uxterm -geometry 166x58" >> ${HOME}/.vnc/xstartup

RUN /bin/echo "${vncpassword}" | vncpasswd -f > ${HOME}/.vnc/passwd

RUN chown -R 1001:0 ${HOME} && \
    chmod 600 ${HOME}/.vnc/passwd && \
    chmod 775 ${HOME}/.vnc/xstartup

EXPOSE 5901
LABEL io.openshift.expose-services="5901:tcp"
USER ${newuser}
ENTRYPOINT ["/usr/bin/vncserver","-fg"]
