FROM --platform=linux/amd64 openjdk:17-jdk
WORKDIR /app
COPY target/*.jar /app/spring-petclinic.jar
ENTRYPOINT java -jar /app/spring-petclinic.jar