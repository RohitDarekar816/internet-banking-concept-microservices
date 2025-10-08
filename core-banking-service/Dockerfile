FROM eclipse-temurin:21.0.2_13-jdk-alpine AS build
LABEL maintainer="chinthaka@javatodev.com"

WORKDIR /workspace

# Copy Gradle wrapper and build files first (better layer caching)
COPY gradlew ./
COPY gradle ./gradle
COPY build.gradle settings.gradle ./

# Copy source
COPY src ./src

# Ensure wrapper is executable and build the fat jar (skip tests for faster image builds)
RUN chmod +x ./gradlew \
    && ./gradlew --no-daemon -Dorg.gradle.java.installations.auto-download=true bootJar -x test -x generateGitProperties

FROM eclipse-temurin:21.0.2_13-jre-alpine
LABEL maintainer="chinthaka@javatodev.com"

WORKDIR /app

# Copy the built jar from the build stage
COPY --from=build /workspace/build/libs/core-banking-service-0.0.1-SNAPSHOT.jar app.jar

# Copy wait script and make it executable
COPY wait-for-it.sh ./wait-for-it.sh
RUN chmod +x ./wait-for-it.sh

# Add bash for wait-for-it.sh
RUN apk add --no-cache bash

# Expose application port
EXPOSE 8092

# Run as a non-root user for better security
RUN adduser -D appuser && chown -R appuser:appuser /app
USER appuser

# Use exec form; put JVM/system props before -jar
ENTRYPOINT ["java", "-Dspring.profiles.active=docker", "-jar", "app.jar"]
