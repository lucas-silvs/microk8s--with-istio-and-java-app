FROM gradle:7.5.1-jdk11 AS build
COPY --chown=gradle:gradle . /home/gradle/src
WORKDIR /home/gradle/src
RUN gradle build --no-daemon


##Dockerfile para projetos java SpringBoot com java 11 Eclipse Temurin
FROM eclipse-temurin:11.0.17_8-jdk

LABEL "author" = "Lucas Santos"
LABEL "email"="lucas.parkl@outlook.com"

COPY --from=build /home/gradle/src/build/libs/*.jar /app/app.jar
WORKDIR /app

ENV JAVA_OPTS "$JAVA_OPTS \
    -XX:+UseParallelGC \
    -XX:ActiveProcessorCount=2 \
    -XX:MaxRAMPercentage=75 \
    --add-opens java.base/java.lang=ALL-UNNAMED \
    -Duser.timezone=America/Fortaleza"

ENTRYPOINT [ "sh", "-c", "java ${JAVA_OPTS} -jar app.jar"]
EXPOSE 5000 9090;

#para buildar execute docker build -t Nome_da_imagem .
#execute sudo docker run -e SPRING_PROFILES_ACTIVE=docker -p 5000:5000 -i feature--toggle