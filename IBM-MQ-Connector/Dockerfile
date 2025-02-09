FROM confluentinc/cp-kafka-connect:7.6.0

# Install dependencies
USER root
RUN microdnf update -y && \
    microdnf install -y \
      unzip \
      curl \
      jq \
      nc \
      && \
    microdnf clean all

# Install IBM MQ Client
ENV MQ_CLIENT_VERSION 9.3.2.0
RUN mkdir -p /opt/mqm/java/lib && \
    curl -o /opt/mqm/java/lib/com.ibm.mq.allclient-${MQ_CLIENT_VERSION}.jar \
      https://repo1.maven.org/maven2/com/ibm/mq/com.ibm.mq.allclient/${MQ_CLIENT_VERSION}/com.ibm.mq.allclient-${MQ_CLIENT_VERSION}.jar && \
    chmod 644 /opt/mqm/java/lib/com.ibm.mq.allclient-${MQ_CLIENT_VERSION}.jar

# Install IBM MQ client libraries
ENV JMS_SPRING_BOOT_VERSION 2.7.6
RUN mkdir -p /usr/share/java/ibm-mq \
  && curl -o /tmp/mq-jms-spring-boot.jar https://repo1.maven.org/maven2/com/ibm/mq/mq-jms-spring-boot-starter/${JMS_SPRING_BOOT_VERSION}/mq-jms-spring-boot-starter-${JMS_SPRING_BOOT_VERSION}.jar \
  && cp /tmp/mq-jms-spring-boot.jar /usr/share/java/ibm-mq/
  
# Download IBM MQ Kafka Connector from IBM's official source
ENV MQ_CONNECTOR_VERSION 2.3.0
RUN mkdir -p /usr/share/confluent-hub-components/ibm-mq-connector && \
    curl -L -o /usr/share/confluent-hub-components/ibm-mq-connector/ibm-mq-connector-${MQ_CONNECTOR_VERSION}.jar \
      https://github.com/ibm-messaging/kafka-connect-mq-source/releases/download/v${MQ_CONNECTOR_VERSION}/kafka-connect-mq-source-${MQ_CONNECTOR_VERSION}-jar-with-dependencies.jar && \
    chmod 644 /usr/share/confluent-hub-components/ibm-mq-connector/ibm-mq-connector-${MQ_CONNECTOR_VERSION}.jar

# Configure permissions
RUN chmod -R 775 /opt/mqm/java/lib && \
    chown -R appuser:appuser /opt/mqm

USER appuser
