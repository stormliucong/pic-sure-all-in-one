FROM jenkins/jenkins:2.377

# Retrieve the proxy build-time arguments
ARG MAVEN_OPTS

ARG http_proxy
ARG https_proxy
ARG no_proxy


# Set the proxy environment variables
ENV MAVEN_OPTS=$MAVEN_OPTS
ENV http_proxy=$http_proxy
ENV https_proxy=$https_proxy
ENV no_proxy=$no_proxy

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt

COPY config.xml /var/jenkins_home/config.xml

COPY scriptApproval.xml /var/jenkins_home/scriptApproval.xml

COPY hudson.tasks.Maven.xml /var/jenkins_home/hudson.tasks.Maven.xml

USER root 

RUN echo deb http://archive.debian.org/debian stretch-backports main >> /etc/apt/sources.list

RUN apt-get update

RUN apt-get -y install apt-transport-https wget

RUN curl -fsSL https://get.docker.com | sh

RUN docker --version 

RUN wget https://www.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz -P /opt

RUN apt-get -y -t stretch-backports install openjdk-11-jdk-headless

RUN tar -xvzf /opt/apache-maven-3.6.3-bin.tar.gz -C /opt

RUN rm /opt/apache-maven-3.6.3-bin.tar.gz

RUN apt-get install jq -y

RUN apt-get -y install uuid-runtime

RUN java ${MAVEN_OPTS} -jar /opt/jenkins-plugin-manager.jar -f /usr/share/jenkins/ref/plugins.txt