FROM openjdk:16-jdk-alpine3.13

LABEL author="fengxsong"
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && apk add --no-cache tini

ENV  VERSION 2.5.0
WORKDIR /opt/

ENV AGENT_HOME /opt/agent/
COPY docker-depends/config.yaml    $AGENT_HOME
COPY docker-depends/jmx_prometheus_javaagent-0.15.0.jar $AGENT_HOME

ENV JAVA_AGENT="-javaagent:$AGENT_HOME/jmx_prometheus_javaagent-0.15.0.jar=9999:$AGENT_HOME/config.yaml"
ENV JAVA_HEAP_OPTS="-Xms1024M -Xmx1024M -Xmn100M "
ENV JAVA_OPTS="-verbose:gc  \
    -XX:MaxMetaspaceSize=256M  -XX:+DisableExplicitGC -XX:+UseStringDeduplication \
    -XX:+UseG1GC  -XX:+HeapDumpOnOutOfMemoryError   -XX:-UseContainerSupport"

RUN wget https://github.com/louyongjiu/logi-kafka-manager/releases/download/v${VERSION}/kafka-manager-${VERSION}.tar.gz && \
    tar xvf kafka-manager-${VERSION}.tar.gz && \
    chmod +x /opt/kafka-manager/bin/startup.sh && \
    rm -rf kafka-manager-${VERSION}*

EXPOSE 8080  9999

ENTRYPOINT ["tini", "--"]

CMD [ "sh", "-c", "/opt/kafka-manager/bin/startup.sh"]
