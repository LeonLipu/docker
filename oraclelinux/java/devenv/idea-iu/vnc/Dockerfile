# saaadel/oraclelinux-idea-ui-vnc

FROM saaadel/oraclelinux-idea-iu
MAINTAINER saaadel

ARG vncpassword=
ARG ideadirname=idea-IU-163.9166.29

ARG user=user
ARG uid=1001
ARG gid=0

ARG sudoersgroup=wheel

ENV DISPLAY=":1" \
    HOME=/home/${user}
WORKDIR ${HOME}

RUN yum groupinstall -y --setopt=tsflags=nodocs \
        "X Window System" "Fonts" && \
    yum install -y --setopt=tsflags=nodocs \
        xterm \
        tigervnc-server \
        gnome-session metacity \
        mc && \
    yum clean all && rm -rf /var/cache/yum/*

RUN /bin/dbus-uuidgen --ensure

RUN useradd -u ${uid} -r -g ${gid} -d ${HOME} -s /bin/bash ${user} && \
    passwd -d ${user} && \
    usermod -a -G ${sudoersgroup} ${user} && \
    [ ! -d ${HOME}/.vnc ] && mkdir -p ${HOME}/.vnc

RUN /bin/echo -e "#!/bin/sh\n\
\n\
\x23 unset DBUS_SESSION_BUS_ADDRESS\n\
\n\
\x23 Run it before for clipboard support\n\
[ -x  /usr/bin/vncconfig ] && /usr/bin/vncconfig -iconic &\n\
\n\
\x23 Uncomment the following two lines for normal desktop:\n\
unset SESSION_MANAGER\n\
\x23 exec /etc/X11/xinit/xinitrc\n\
\n\
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup\n\
[ -r \$HOME/.Xresources ] && xrdb \$HOME/.Xresources\n\
\n\
[ -x /usr/bin/xsetroot ] && /usr/bin/xsetroot -solid '#222E45'\n\
[ -x /usr/bin/xclock ] && /usr/bin/xclock -geometry 100x100-5+5 &\n\
[ -x /usr/bin/xterm ] && xterm -geometry 132x44-10-10 -ls -title \"\$VNCDESKTOP Desktop\" &\n\
[ -x /usr/bin/metacity ] && /usr/bin/metacity &\n\
if test -z \"\$DBUS_SESSION_BUS_ADDRESS\" ; then\n\
	eval \`dbus-launch --sh-syntax -–exit-with-session\`\n\
	echo \"D-BUS per-session daemon address is: \\n\
	\$DBUS_SESSION_BUS_ADDRESS\"\n\
fi\n\
/opt/${ideadirname}/bin/idea.sh &\n\
exec sudo /usr/bin/gnome-session\n\
" >> ${HOME}/.vnc/xstartup

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
ENTRYPOINT ["/usr/bin/vncserver", "-fg"]

