FROM eclipse-temurin:21-jdk AS builder

WORKDIR /usr/src/app/

COPY gradlew* build.gradle settings.gradle .
COPY ./gradle ./gradle

RUN chmod +x ./gradlew
RUN ./gradlew
RUN ./gradlew downloadRepos

COPY . .
COPY ./pb ./proto

RUN chmod +x ./gradlew
RUN ./gradlew installDist -PprotoSourceDir=./proto


FROM eclipse-temurin:21-jre

WORKDIR /usr/src/app/

COPY --from=builder /usr/src/app/build/install/opentelemetry-demo-ad ./app

ENV AD_PORT=8080

RUN useradd -r -u 10001 appuser
USER appuser	

ENTRYPOINT ["./app/bin/Ad"]

