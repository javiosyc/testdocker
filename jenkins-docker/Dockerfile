FROM java:openjdk-7u65-jdk
MAINTAINER javio "javiosyc@gmail.com"
RUN apt-get update && apt-get install -y wget git curl zip vim-tiny && rm -rf /var/lib/apt/lists/*

RUN mkdir /usr/share/jenkins/
RUN useradd -d /home/jenkins -m -s /bin/bash jenkins

COPY init.groovy /tmp/WEB-INF/init.groovy.d/tcp-slave-angent-port.groovy
RUN curl -L http://mirrors.jenkins-ci.org/war-stable/latest/jenkins.war -o /usr/share/jenkins/jenkins.war \
  && cd /tmp && zip -g /usr/share/jenkins/jenkins.war WEB-INF/init.groovy.d/tcp-slave-angent-port.groovy && rm -rf /tmp/WEB-INF

ENV JENKINS_HOME /var/jenkins_home
RUN usermod -m -d "$JENKINS_HOME" jenkins && chown -R jenkins "$JENKINS_HOME"
#VOLUME /var/jenkins_home

USER jenkins

COPY jenkins.sh /usr/local/bin/jenkins.sh

#download git plugins
RUN mkdir -p $JENKINS_HOME/plugins
RUN for plugin in scm-api git-client git ;\
    do curl -sf -o $JENKINS_HOME/plugins/${plugin}.hpi \
       -L http://mirrors.jenkins-ci.org/plugins/${plugin}/latest/${plugin}.hpi ; done

ENTRYPOINT ["/usr/local/bin/jenkins.sh"]

# for main web interface:
EXPOSE 8080

# will be used by attached slave agents:
EXPOSE 50000
