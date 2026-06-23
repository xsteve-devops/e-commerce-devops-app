# Ad Service

The Ad service provides advertisements based on context keys. If no context keys are provided, it returns random ads.

## Building Locally

The Ad service requires JDK 21 to build and uses the Gradle wrapper to compile, install, and distribute the application.

From the `src/ad` directory, run:

```sh
./gradlew --no-daemon installDist -PprotoSourceDir=./pb
```

It will create an executable script:

```text
build/install/opentelemetry-demo-ad/bin/Ad
```

To run the Ad Service:

```sh
export AD_PORT=8080
export FEATURE_FLAG_GRPC_SERVICE_ADDR=featureflagservice:50053
./build/install/opentelemetry-demo-ad/bin/Ad
```

### Upgrading Gradle

If you need to upgrade the version of Gradle, run:

```sh
./gradlew wrapper --gradle-version <new-version>
```

## Building Docker

This Dockerfile is designed to use `src/ad` as the Docker build context.

From the repository root, run:

```sh
docker build -t ad-service:test -f src/ad/Dockerfile src/ad
```

Or from the `src/ad` directory, run:

```sh
docker build -t ad-service:test .
```