# Use latest aerogear/wildfly image as the base
FROM aerogear/wildfly 
MAINTAINER Matthias Wessendorf <matzew@apache.org>

# Download Aerogear distribution
ENV UPSVERSION=1.3.0-no-auth-SNAPSHOT
ENV UPSDIST=/opt/aerogear-unifiedpush-server-$UPSVERSION

RUN curl -L -o /opt/aerogear-unifiedpush-server-$UPSVERSION-dist.tar.gz https://github.com/aerogear/aerogear-unifiedpush-server/releases/download/1.3.0-no-auth/aerogear-unifiedpush-server-1.3.0-no-auth-SNAPSHOT-dist.tar.gz
WORKDIR /opt
RUN tar zxf aerogear-unifiedpush-server-$UPSVERSION-dist.tar.gz

# unzip migrator and copy liquibase.properties
WORKDIR $UPSDIST/migrator
RUN unzip ups-migrator-dist.zip
COPY liquibase.properties  $UPSDIST/migrator/ups-migrator/

# install openssl needed for certificate
RUN yum install -y openssl && yum -q clean all

# Run everything below as aerogear user
USER jboss

# Switch to the working dir $JBOSS_HOME/standalone/deployments
WORKDIR /opt/jboss/wildfly/standalone/deployments

# copy war files
RUN cp $UPSDIST/servers/unifiedpush-server-wildfly.war $JBOSS_HOME/standalone/deployments


# copy and run startup script
# migration is done inside the startup script before launching the server
COPY entrypoint.sh /opt/
ENTRYPOINT ["/opt/entrypoint.sh"]
