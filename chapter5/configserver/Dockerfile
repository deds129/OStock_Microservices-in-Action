# Multi-layer build 

# Stage #1
FROM openjdk:21-slim as build
LABEL maintainer="deds129 <deds129@gmail.com>"

WORKDIR configserver_application
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} configserver.jar
RUN java -Djarmode=layertools -jar configserver.jar extract

# Stage #2
FROM openjdk:21-slim
WORKDIR configserver_application

#Add volume pointing to /tmp
VOLUME /tmp

COPY --from=build configserver_application/dependencies/ ./
COPY --from=build configserver_application/spring-boot-loader/ ./
COPY --from=build configserver_application/snapshot-dependencies/ ./
COPY --from=build configserver_application/application/ ./

ENTRYPOINT ["java", "org.springframework.boot.loader.launch.JarLauncher"]