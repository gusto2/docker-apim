# ------------------------------------------------------------------------
#
# Copyright 2018 WSO2, Inc. (http://wso2.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License
#
# ------------------------------------------------------------------------

# set to latest Centos
FROM centos:7
MAINTAINER WSO2 Docker Maintainers "dev@wso2.org"

# set user configurations
ARG USER=wso2carbon
ARG USER_ID=8020
ARG USER_GROUP=wso2
ARG USER_GROUP_ID=8020
ARG USER_HOME=/home/${USER}
# set dependant files directory
ARG FILES=./files
# set jdk configurations
ARG JDK=jdk1.8.0*
ARG JAVA_HOME=${USER_HOME}/java
# set wso2 product configurations
ARG WSO2_SERVER=wso2am-analytics
ARG WSO2_SERVER_VERSION=2.6.0
ARG WSO2_SERVER_PACK=${WSO2_SERVER}-${WSO2_SERVER_VERSION}
ARG WSO2_SERVER_HOME=${USER_HOME}/${WSO2_SERVER_PACK}
ARG JDBC_DRIVER=ojdbc6_1.0.0.jar

# set WSO2 EULA
ARG MOTD='printf "\n\
 Welcome to WSO2 Docker Resources \n\
 --------------------------------- \n\
 This Docker container comprises of a WSO2 product, running with its latest updates \n\
 which are under the End User License Agreement (EULA) 2.0. \n\
 Read more about EULA 2.0 here @ https://wso2.com/licenses/wso2-update/2.0 \n"'
# install required packages
RUN yum update -y && \
    yum install -y nc && \
    rm -rf /var/cache/yum/* && \
    echo $MOTD > /etc/profile.d/motd.sh

# create a user group and a user
RUN groupadd --system -g ${USER_GROUP_ID} ${USER_GROUP} && \
    useradd --system --create-home --home-dir ${USER_HOME} --no-log-init -g ${USER_GROUP_ID} -u ${USER_ID} ${USER}

# copy the jdk and wso2 product distributions to user's home directory
COPY ${FILES}/${JDK} ${USER_HOME}/java/
COPY ${FILES}/${WSO2_SERVER_PACK}/ ${WSO2_SERVER_HOME}/
# copy mysql connector jar to the server as a third party library
#COPY ${FILES}/mysql-connector-java-*-bin.jar ${WSO2_SERVER_HOME}/lib/
COPY  ${FILES}/${JDBC_DRIVER} ${WSO2_SERVER_HOME}/lib/

# copy keystore and truststore
COPY ${FILES}/wso2carbon.jks  ${WSO2_SERVER_HOME}/resources/security/
COPY ${FILES}/client-truststore.jks ${WSO2_SERVER_HOME}/wso2/dashboard/conf/data-bridge/
COPY ${FILES}/client-truststore.jks ${WSO2_SERVER_HOME}/wso2/worker/conf/data-bridge/
COPY ${FILES}/client-truststore.jks ${WSO2_SERVER_HOME}/manager/conf/data-bridge/
COPY ${FILES}/client-truststore.jks ./wso2am-analytics-2.6.0/resources/security/
RUN chown -R wso2carbon:wso2 ${WSO2_SERVER_HOME}

# set the user and work directory
USER ${USER_ID}
WORKDIR ${USER_HOME}

# set environment variables
ENV JAVA_HOME=${JAVA_HOME} \
    PATH=$JAVA_HOME/bin:$PATH \
    WSO2_SERVER_HOME=${WSO2_SERVER_HOME} \
    WORKING_DIRECTORY=${USER_HOME}

# expose ports
EXPOSE 9091 9444 7712 7612 9613 9713 7444 7071

