# Use latest jboss/wildfly
FROM jboss/wildfly:8.2.0.Final

MAINTAINER "Rafael Benevides" <benevides@redhat.com>

#Create admin user
RUN /opt/jboss/wildfly/bin/add-user.sh -u admin -p docker#admin --silent

# Add customization folder
COPY customization /opt/jboss/wildfly/customization/

USER root

# Run customization scripts as root
RUN chmod +x /opt/jboss/wildfly/customization/execute.sh
RUN echo '/opt/jboss/wildfly/customization/execute.sh standalone standalone-ha.xml' > /tmp/execute.sh
RUN chmod +x /tmp/execute.sh

ADD ticket-monster.war /opt/jboss/wildfly/standalone/deployments/

# Fix for Error: Could not rename /opt/jboss/wildfly/standalone/configuration/standalone_xml_history/current
RUN rm -rf /opt/jboss/wildfly/standalone/configuration/standalone_xml_history

RUN chown -R jboss:jboss /opt/jboss/wildfly/

USER jboss

# Expose the ports we're interested in
EXPOSE 8080 9990 8009

# Set the default command to run on boot
# This will boot WildFly in the standalone mode and bind to external interface and enable HA
CMD /tmp/execute.sh && /opt/jboss/wildfly/bin/standalone.sh -b `hostname -i` -bmanagement `hostname -i` -c standalone-ha.xml

