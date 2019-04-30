FROM debian:stretch

MAINTAINER keopx <keopx@keopx.net>

ENV DEBIAN_FRONTEND=noninteractive

# Locale
ENV INITRD Noc
ENV LOCALE es_ES.UTF-8
ENV LANGUAGE es_ES.UTF-8
ENV LC_ALL es_ES.UTF-8
ENV LANG es_ES.UTF-8

# Set repositories
RUN \
   echo "deb http://ftp.de.debian.org/debian/ stretch main non-free contrib" > /etc/apt/sources.list && \
   echo "deb-src http://ftp.de.debian.org/debian/ stretch main non-free contrib" >> /etc/apt/sources.list && \
   echo "deb http://security.debian.org/ stretch/updates main contrib non-free" >> /etc/apt/sources.list && \
   echo "deb-src http://security.debian.org/ stretch/updates main contrib non-free" >> /etc/apt/sources.list && \
   # Update repositories cache and distribution
   apt-get -qq update && apt-get -qqy upgrade && \
   apt-get -yqq install apt-transport-https ca-certificates python3-software-properties gnupg2 locales openjdk-8-jre wget unzip && \
   sed -i -e "s/# $LOCALE/$LOCALE/" /etc/locale.gen && \
   echo "LANG=$LOCALE">/etc/default/locale && \
   dpkg-reconfigure --frontend=noninteractive locales && \
   update-locale LANG=$LOCALE && \
   # Install jenkins.
   wget -qO - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add - && \
   echo 'deb http://pkg.jenkins-ci.org/debian binary/' \
   | tee /etc/apt/sources.list.d/jenkins.list && \
   apt-get update && \
   apt-get install --no-install-recommends -y jenkins && \
   apt-get clean && \
   rm -rf /var/lib/apt/lists/* && \
   update-rc.d -f jenkins disable  && \
   # Cleanup some things.
   apt-get -q autoclean && \
   rm -rf /var/lib/apt/lists/*

ENV JENKINS_HOME /var/jenkins

EXPOSE 8080 50000

CMD ["/usr/bin/java", "-jar", "/usr/share/jenkins/jenkins.war"]
