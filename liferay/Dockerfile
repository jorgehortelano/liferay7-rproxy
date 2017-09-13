FROM openjdk:8-jdk-alpine

# Install required packages
RUN apk --no-cache add curl supervisor bash

# Configure supervisord
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Install liferay
ARG liferay_home_folder
ARG liferay_tomcat_folder
ARG liferay_mysql_user
ARG liferay_mysql_password
ARG machine_domain

RUN curl -O -s -k -L -C - https://downloads.sourceforge.net/project/lportal/Liferay%20Portal/7.0.3%20GA4/liferay-ce-portal-tomcat-7.0-ga4-20170613175008905.zip \	
	&& mkdir /opt \
	&& unzip liferay-ce-portal-tomcat-7.0-ga4-20170613175008905.zip -d /opt \
	&& rm liferay-ce-portal-tomcat-7.0-ga4-20170613175008905.zip

# Add config to tomcat
RUN /bin/echo -e '\nCATALINA_OPTS="$CATALINA_OPTS -Dfile.encoding=UTF8 -Djava.net.preferIPv4Stack=true"' >> ${liferay_tomcat_folder}/bin/setenv.sh

# Add configuration liferay file and disable installation wizard
COPY config/portal-bundle.properties ${liferay_home_folder}/portal-bundle.properties
COPY config/portal-bd-MYSQL.properties ${liferay_home_folder}/portal-bd-MYSQL.properties

#Set variable values
RUN sed -i "s|LIFERAY_HOME|${liferay_home_folder}|g" ${liferay_home_folder}/portal-bundle.properties \
        && sed -i "s|MACHINE_DOMAIN|${machine_domain}|g" ${liferay_home_folder}/portal-bundle.properties \
	&& sed -i "s|MYSQL_LIFERAY_USER|${liferay_mysql_user}|g" ${liferay_home_folder}/portal-bd-MYSQL.properties \
	&& sed -i "s|MYSQL_LIFERAY_USER_PASSWORD|${liferay_mysql_password}|g" ${liferay_home_folder}/portal-bd-MYSQL.properties \
	&& sed -i "s|LIFERAY_TOMCAT_FOLDER|${liferay_tomcat_folder}|g" /etc/supervisor/conf.d/supervisord.conf 

WORKDIR ${liferay_home_folder}

# Volumes
VOLUME /var/liferay ${liferay_home_folder}

# Ports
EXPOSE 8080

# EXEC
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]