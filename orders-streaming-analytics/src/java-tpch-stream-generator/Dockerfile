# syntax=docker/dockerfile:1

FROM maven:3.9-eclipse-temurin-25 AS builder

WORKDIR /workspace

COPY pom.xml .
RUN mvn --batch-mode dependency:go-offline

COPY src ./src

RUN mvn --batch-mode clean package -DskipTests


FROM eclipse-temurin:25-jre

WORKDIR /app

RUN groupadd \
        --system \
        --gid 10001 \
        appgroup \
    && useradd \
        --system \
        --uid 10001 \
        --gid appgroup \
        --no-create-home \
        appuser

COPY --from=builder \
    --chown=appuser:appgroup \
    /workspace/target/tpch-generator.jar \
    /app/tpch-generator.jar

USER appuser

ENTRYPOINT ["java", "-jar", "/app/tpch-generator.jar"]
