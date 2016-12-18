# saaadel/oraclelinux-devenv

FROM saaadel/oraclelinux-java
MAINTAINER saaadel

ARG mavenurl=http://ftp.byfly.by/pub/apache.org/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
ARG mavendirname=apache-maven-3.3.9

ARG gradleurl=https://services.gradle.org/distributions/gradle-3.2.1-bin.zip
ARG gradledirname=gradle-3.2.1

ARG nodejsurl=https://rpm.nodesource.com/setup_7.x

# Download and set up env vars (no pipe for tar, because curl follows redirects)
RUN curl --silent --location "${nodejsurl}" | bash - && \
    yum install -y --setopt=tsflags=nodocs \
        git subversion \
        unzip \
        nodejs npm && \
        yum clean all && rm -rf /var/cache/yum/* && \
    npm install -g bower grunt-cli gulp-cli && \
    rm -rf /opt/maven.tmp.tar.gz && \
    curl -jkL -o /opt/maven.tmp.tar.gz \
         "${mavenurl}" && \
    tar xzf /opt/maven.tmp.tar.gz -C /opt && \
    rm -rf /opt/maven.tmp.tar.gz && \
    rm -rf /opt/gradle.tmp.zip && \
    curl -jkL -o /opt/gradle.tmp.zip \
         "${gradleurl}" && \
    unzip /opt/gradle.tmp.zip -d /opt && \
    rm -rf /opt/gradle.tmp.zip && \
    /bin/echo -e "\n\
M2_HOME=/opt/${mavendirname}/\n\
GRADLE_HOME=/opt/${gradledirname}/\n\
export PATH=$PATH:/opt/${mavendirname}/bin:/opt/${gradledirname}/bin\n\
" | tee -a /root/.profile /etc/skel/.profile

# Set up to use it in the Dockerfile
ENV M2_HOME=/opt/${mavendirname} \
    GRADLE_HOME=/opt/${gradledirname} \
    PATH=$PATH:/opt/${mavendirname}/bin:/opt/${gradledirname}/bin

CMD ["mvn", "-v"]

