FROM thorsager/java:8-ubuntu16.04
MAINTAINER Michael Thorsager, github.com/thorsager

RUN apt-get -y install xmlstarlet wget \
	&& apt-get -y clean \
	&& apt-get -y autoclean

ENV GF_USER=glassfish
ENV GF_GROUP=glassfish

ENV GLASSFISH_ARCHIVE=glassfish4 \
	GLASSFISH_URL=http://download.oracle.com/glassfish/4.1.2/release/glassfish-4.1.2.zip \
	DOMAIN_NAME=domain1 \
	INSTALL_DIR=/opt \
	PASSWORD=glassfish
ENV GLASSFISH_HOME=${INSTALL_DIR}/${GLASSFISH_ARCHIVE}/glassfish\
	PATH=$PATH:${INSTALL_DIR}/${GLASSFISH_ARCHIVE}/bin
ENV	DEPLOYMENT_DIR=${GLASSFISH_HOME}/domains/${DOMAIN_NAME}/autodeploy

RUN curl -o ${INSTALL_DIR}/${GLASSFISH_ARCHIVE}.zip -L {$GLASSFISH_URL} && \ 
    unzip ${INSTALL_DIR}/${GLASSFISH_ARCHIVE}.zip -d ${INSTALL_DIR} && \ 
    rm ${INSTALL_DIR}/${GLASSFISH_ARCHIVE}.zip && \
	echo "--- Setup the password file ---" && \
    echo "AS_ADMIN_PASSWORD=" > /tmp/glassfishpwd && \
    echo "AS_ADMIN_NEWPASSWORD=${PASSWORD}" >> /tmp/glassfishpwd  && \
    echo "--- Enable DAS, change admin password, and secure admin access ---" && \
    asadmin --user=admin --passwordfile=/tmp/glassfishpwd change-admin-password --domain_name ${DOMAIN_NAME} && \
    asadmin start-domain ${DOMAIN_NAME} && \
    echo "AS_ADMIN_PASSWORD=${PASSWORD}" > /tmp/glassfishpwd && \
    asadmin --user=admin --passwordfile=/tmp/glassfishpwd enable-secure-admin && \
    asadmin --user=admin stop-domain ${DOMAIN_NAME} && \
    rm /tmp/glassfishpwd

RUN groupadd ${GF_GROUP} && useradd -g ${GF_GROUP} ${GF_USER} -m \
 && chown -R ${GF_USER}:${GF_GROUP} ${GLASSFISH_HOME}

COPY gf_env.sh ${GLASSFISH_HOME}/bin
#ENTRYPOINT ./gf_env.sh && ./asadmin start-domain --verbose ${DOMAIN_NAME} 
#ENTRYPOINT ./gf_entrypoint.sh start-domain --verbose ${DOMAIN_NAME} 
EXPOSE 4848 9009 8080 8181

WORKDIR ${GLASSFISH_HOME}/bin
USER ${GF_USER}
ENTRYPOINT ./gf_env.sh && ./asadmin start-domain --verbose ${DOMAIN_NAME} 
#CMD ["asadmin", "start-domain","${DOMAIN_NAME}", "-v"]
