FROM  ubuntu:trusty

MAINTAINER Larry Cai <larry.caiyu@gmail.com>

ENV GERRIT_HOME /home/gerrit
ENV GERRIT_USER gerrit
ENV GERRIT_WAR /home/gerrit/gerrit.war

RUN apt-get update

RUN useradd -m ${GERRIT_USER}
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y openjdk-7-jdk sudo git-core gitweb supervisor vim-tiny
RUN mkdir -p /var/log/supervisor

ADD http://gerrit-releases.storage.googleapis.com/gerrit-2.8.5.war /tmp/gerrit.war

ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN mkdir -p $GERRIT_HOME/gerrit
RUN mv /tmp/gerrit.war $GERRIT_WAR
RUN chown -R ${GERRIT_USER}:${GERRIT_USER} $GERRIT_HOME

USER gerrit
RUN java -jar $GERRIT_WAR init --batch -d $GERRIT_HOME/gerrit

# clobber the gerrit config. set the URL to localhost:8080
ADD gerrit.config $GERRIT_HOME/gerrit/etc/gerrit.config
ADD gitiles.jar $GERRIT_HOME/gerrit/plugins/gitiles.jar
USER root
EXPOSE 8080 29418
CMD ["/usr/sbin/service","supervisor","start"]