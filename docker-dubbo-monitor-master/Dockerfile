
# base image
FROM maven:3.6.3-jdk-8

RUN git clone --branch dubbo-2.6.0 https://github.com/apache/dubbo \
    && cd dubbo \
    && rm -fr dubbo-config/dubbo-config-spring/src/test \
    && mvn package -DskipTests \
    && mkdir -p /target \
    && tar xvf dubbo-simple/dubbo-monitor-simple/target/dubbo-monitor-simple-2.6.0-assembly.tar.gz -C /target/ \
    # the following 4 steps are meant to delete unuseful lib jars 
    && mv /target/dubbo-monitor-simple-2.6.0/lib/dubbo-monitor-simple-2.6.0.jar . \
    && mv /target/dubbo-monitor-simple-2.6.0/lib/dubbo-2.6.0.jar . \
    && rm -f /target/dubbo-monitor-simple-2.6.0/lib/dubbo* \
    && mv ./dubbo*.jar /target/dubbo-monitor-simple-2.6.0/lib \
    && sed -i "s/dubbo.registry.address/#dubbo.registry.address /g" /target/dubbo-monitor-simple-2.6.0/conf/dubbo.properties \
    && echo "\ndubbo.registry.address=zookeeper://\${ZOOKEEPER_ADDRESS}" >> /target/dubbo-monitor-simple-2.6.0/conf/dubbo.properties \
    && echo "\ntail -f \$STDOUT_FILE" >> /target/dubbo-monitor-simple-2.6.0/bin/start.sh 

# base image
FROM openjdk:8u275-jre

COPY --from=0 /target /usr/local/

RUN apt-get update \
    && apt-get install -y procps net-tools \
    && apt-get clean

CMD ["/usr/local/dubbo-monitor-simple-2.6.0/bin/start.sh"]
