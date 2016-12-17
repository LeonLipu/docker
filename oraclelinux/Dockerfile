FROM oraclelinux
MAINTAINER saaadel

SHELL ["bash", "-c"]
USER root
ENV HOME=/root
WORKDIR ${HOME}

RUN /bin/echo -e "\n\
# User specific aliases and functions for safe, colors and links\n\
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

RUN /bin/echo -e "\n\
alias rm='rm -i'\n\
alias cp='cp -i'\n\
alias mv='mv -i'\n\
\n\
# User specific aliases and functions for safe, colors and links\n\
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
" > /etc/skel/.bashrc

RUN yum clean all && \
    yum update -y && \
    yum install -y --setopt=tsflags=nodocs \
        sudo which && \
        yum clean all && rm -rf /var/cache/yum/*

CMD ["bash", "--version"]

