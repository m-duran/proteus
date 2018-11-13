# build image
FROM openjdk:8-jdk-alpine AS builder
ENV APP_HOME=/root/dev/myapp/
RUN mkdir -p $APP_HOME/src/main/java
WORKDIR $APP_HOME
COPY build.gradle gradlew gradlew.bat $APP_HOME
COPY gradle $APP_HOME/gradle

# download dependencies as a separate step to cache them
# RUN ./gradlew build -x :bootRepackage -x test --continue
# I don't know if this line is accomplishing what I wanted
# The original line above doesn't work, the task 'bootRepackage' isn't found
RUN ./gradlew build -x bootJar -x test --continue
COPY . .
RUN ./gradlew build

# Build final image
# Only need JRE here, not the whole JDK
FROM openjdk:8-jre-alpine
WORKDIR /root/
COPY --from=builder /root/dev/myapp/build/libs/movie-service-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]