version: '3.4'

services:
  jenkins:
    build: .
    user: root
    container_name: jenkins
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./jenkins_data/:/var/jenkins_home
    ports:
      - 8080:8080
      
      
     
FROM jenkins/jenkins:2.289.2-lts-jdk11
USER root

RUN apt-get update && \
    apt-get -y install apt-transport-https \
      ca-certificates \
      curl \
      gnupg2 \
      software-properties-common && \
    curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
    add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
      $(lsb_release -cs) \
      stable" && \
   apt-get update && \
   apt-get -y install docker-ce
USER jenkins
