FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD ./target/reading-cloud-book-1.0.0.jar app.jar
ENTRYPOINT ["java", "-jar", "-Dspring.cloud.nacos.config.namespace=89d0c247-4384-400a-84a1-d6cfd3b0fbb8", "/app.jar"]