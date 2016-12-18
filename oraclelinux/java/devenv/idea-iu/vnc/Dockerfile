# saaadel/oraclelinux-idea-ui-vnc

FROM saaadel/oraclelinux-idea-iu
MAINTAINER saaadel

ARG vncpassword=

ARG user=user
ARG uid=1001
ARG gid=0

ARG sudoersgroup=wheel

ENV DISPLAY=":1" \
    HOME=/home/${user}
WORKDIR ${HOME}

RUN yum install -y --setopt=tsflags=nodocs \
        xorg-x11-server-utils xorg-x11-server-Xorg \
        xorg-x11-fonts-* \
        gnu-free-fonts-common \
        gnu-free-mono-fonts \
        gnu-free-sans-fonts \
        gnu-free-serif-fonts \
        google-noto-fonts-common \
        google-noto-sans-ui-fonts \
        google-noto-serif-fonts \
        google-noto-sans-fonts \
        dejavu-fonts-common \
        dejavu-sans-fonts \
        dejavu-sans-mono-fonts \
        dejavu-serif-fonts \
        xterm \
        gnome-session \
        tigervnc-server && \
        yum clean all && rm -rf /var/cache/yum/*

RUN /bin/dbus-uuidgen --ensure

RUN useradd -u ${uid} -r -g ${gid} -d ${HOME} -s /bin/bash ${user} && \
    passwd -d ${user} && \
    usermod -a -G ${sudoersgroup} ${user} && \
    [ ! -d ${HOME}/.vnc ] && mkdir -p ${HOME}/.vnc

RUN /bin/echo -e "#!/bin/sh\n\
# Uncomment the following two lines for normal desktop:\n\
unset SESSION_MANAGER\n\
unset DBUS_SESSION_BUS_ADDRESS\n\
exec /etc/X11/xinit/xinitrc\n\
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup\n\
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources\n\
xsetroot -solid grey\n\
vncconfig -iconic &\n\
xterm -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &\n\
twm &\n\
exec gnome-session\n\
" >> ${HOME}/.vnc/xstartup && \
    /bin/echo "/opt/${ideadirname}/bin/idea.sh" >> ${HOME}/.vnc/xstartup

RUN /bin/echo "${vncpassword}" | vncpasswd -f > ${HOME}/.vnc/passwd

RUN chown -R ${uid}:${gid} ${HOME} && \
    chmod 600 ${HOME}/.vnc/passwd && \
    chmod 775 ${HOME}/.vnc/xstartup && \
    mkdir /mnt/external-dir && \
    chmod 777 /mnt/external-dir

VOLUME /mnt/external-dir

EXPOSE 5901
LABEL io.openshift.expose-services="5901:tcp"
USER ${user}
CMD []
ENTRYPOINT ["/usr/bin/vncserver"]