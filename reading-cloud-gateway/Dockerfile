FROM openjdk:8-jdk-alpine

ADD ./target/reading-cloud-gateway-1.0.0.jar /usr/local

ENTRYPOINT ["java", "-jar", "-Dspring.cloud.nacos.config.namespace=89d0c247-4384-400a-84a1-d6cfd3b0fbb8", "reading-cloud-gateway-1.0.0.jar.jar"]