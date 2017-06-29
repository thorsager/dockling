FROM ubuntu:16.04
MAINTAINER Michael Thorsager, github.com/thorsager

RUN apt-get update \
	&& apt-get -y install unzip curl \
	&& apt-get -y install openjdk-8-jdk \ 
	&& apt-get -y clean \
	&& apt-get -y autoclean \
	&& update-alternatives --config java \
	&& update-alternatives --config javac

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
