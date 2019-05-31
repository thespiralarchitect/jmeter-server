FROM openjdk:8-jre-slim

ARG JMETER_VERSION="5.0"
ARG JMETER_PM_VERSION="1.3"
ARG CMDRUNNER_VERSION="2.2"

ENV JMETER_HOME /jmeter/jmeter/

# INSTALL PRE-REQ
RUN apt-get update && \
    apt-get -y install wget 

# INSTALL JMETER BASE 
RUN mkdir /jmeter
WORKDIR /jmeter

RUN wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-$JMETER_VERSION.tgz && \
    tar -xzf apache-jmeter-$JMETER_VERSION.tgz && \
    rm apache-jmeter-$JMETER_VERSION.tgz && \
    mv apache-jmeter-$JMETER_VERSION jmeter

RUN wget https://search.maven.org/remotecontent?filepath=kg/apc/jmeter-plugins-manager/$JMETER_PM_VERSION/jmeter-plugins-manager-$JMETER_PM_VERSION.jar -O jmeter-plugins-manager-$JMETER_PM_VERSION.jar && \
    mv jmeter-plugins-manager-$JMETER_PM_VERSION.jar jmeter/lib/ext

RUN wget http://search.maven.org/remotecontent?filepath=kg/apc/cmdrunner/$CMDRUNNER_VERSION/cmdrunner-$CMDRUNNER_VERSION.jar -O cmdrunner-$CMDRUNNER_VERSION.jar && \
    mv cmdrunner-$CMDRUNNER_VERSION.jar jmeter/lib

RUN java -cp jmeter/lib/ext/jmeter-plugins-manager-$JMETER_PM_VERSION.jar org.jmeterplugins.repository.PluginManagerCMDInstaller

RUN jmeter/bin/PluginsManagerCMD.sh install bzm-random-csv

RUN jmeter/bin/PluginsManagerCMD.sh install jpgc-perfmon

RUN jmeter/bin/PluginsManagerCMD.sh install jpgc-casutg

RUN jmeter/bin/PluginsManagerCMD.sh install bzm-parallel

RUN jmeter/bin/PluginsManagerCMD.sh install jpgc-rotating-listener

RUN wget https://github.com/emqx/mqtt-jmeter/releases/download/1.0.1/mqtt-xmeter-1.0.1-jar-with-dependencies.jar && mv mqtt-xmeter-1.0.1-jar-with-dependencies.jar jmeter/lib/ext

WORKDIR $JMETER_HOME

COPY user.properties bin/user.properties

EXPOSE 60000 1099 50000
