FROM maven:3.8.7-openjdk-18-slim as stage1
WORKDIR /app
COPY . .
RUN mvn clean package

FROM openjdk:22-jdk-slim-bullseye as stage2 
WORKDIR /app1
COPY --from=stage1 /app/target/**.jar app.jar
CMD ["java","-jar","app.jar"]