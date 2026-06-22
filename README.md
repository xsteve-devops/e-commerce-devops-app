# E-Commerce DevOps Application

## Overview

This repository contains the application-side implementation for my E-Commerce DevOps project.

The project is based on the OpenTelemetry Demo. I selected several microservices from the original demo and rebuilt the delivery workflow with a DevOps-focused approach, including Docker image creation, GitHub Actions CI, image scanning, image publishing, and GitOps manifest updates.

The project uses a two-repository design:

- `e-commerce-devops-app`: application source code, Dockerfiles, GitHub Actions workflows, image build, image scan, and image publishing
- `e-commerce-devops-infra`: Terraform infrastructure code, Kubernetes manifests, Argo CD Applications, and GitOps desired state

This repository focuses on how application code is built, validated, packaged, and released as container images. Infrastructure and Kubernetes deployment state are managed in the infra repository.

## Architecture

![Two Repository GitOps Workflow](docs/images/two-repo-gitops-workflow.png)

## Current Scope

The current implementation focuses on the Ad Service as the first completed service.

Completed for Ad Service:

- Multi-stage Dockerfile
- Java Gradle build validation
- GitHub Actions CI workflow
- Docker image build
- Trivy image scan
- SonarCloud code quality scan on the main branch
- Docker image publishing with SHA-based tags
- Automatic update of the Kubernetes image tag in the infra repository

Product Catalog Service and Recommendation Service are planned as the next services to be added following the same workflow.

## Repository Structure

```text
.github/
  workflows/
    ad-service-ci.yaml

src/
  ad/
    Dockerfile
    build.gradle
    settings.gradle
    src/
    pb/

  product-catalog/
    Dockerfile
    ...

  recommendation/
    Dockerfile
    ...
```

## CI/CD Workflow

![App CI/CD Pipeline](docs/images/app-ci-cd-pipeline.png)

The Ad Service workflow is implemented with GitHub Actions.

### Pull Request Flow

When a pull request targets the `main` branch, the workflow runs validation steps without publishing images.

```text
Pull Request
  -> Gradle build validation
  -> Docker image build
  -> Trivy image scan
```

This ensures that code changes can be built and packaged safely before being merged.

### Main Branch Flow

When changes are merged into the `main` branch, the workflow runs the release flow.

```text
Push to main
  -> Gradle build validation
  -> SonarCloud scan
  -> Docker image build
  -> Trivy image scan
  -> Push Docker image
  -> Update Kubernetes image tag in infra repository
```

The workflow does not deploy directly to Kubernetes. Instead, it updates the Kubernetes manifest in the infrastructure repository. Argo CD then detects the Git change and synchronizes the desired state to AWS EKS.

## Image Tagging Strategy

Docker images are tagged using the GitHub commit SHA.

```text
<registry>/<image-name>:<github-sha>
```

Using the commit SHA makes each image version traceable back to the exact source code revision that produced it.

## Ad Service Docker Build

Ad Service is a Java Gradle application.

The Dockerfile uses a multi-stage build:

```text
builder stage
  -> install dependencies
  -> build application with Gradle
  -> generate runnable distribution

runtime stage
  -> copy built application
  -> run the service
```

The Gradle build command used for the service is:

```bash
./gradlew --no-daemon installDist -PprotoSourceDir=./pb
```

The final runtime image only contains the files required to run the service, which reduces image size and keeps build-time dependencies out of the runtime layer.

## Quality And Security Checks

The workflow currently includes:

- Gradle build validation
- SonarCloud scan
- Docker image build validation
- Trivy image vulnerability scan

SonarCloud is used for source code quality analysis. Trivy is used to scan container images for operating system and application dependency vulnerabilities.

## GitOps Integration

After a successful main-branch release, the workflow updates the image tag in the infra repository.

```text
Application repository
  -> build and push image
  -> update image tag in infrastructure repository
  -> Argo CD syncs the updated manifest to EKS
```

This keeps the application CI pipeline separated from direct cluster access. The Kubernetes cluster state is managed declaratively through GitOps.

## Related Repository

Infrastructure, Kubernetes manifests, and Argo CD configuration are managed in:

```text
e-commerce-devops-infra
```

## Tech Stack

- Java
- Gradle
- Docker
- GitHub Actions
- Docker Hub
- Trivy
- SonarCloud
- Kubernetes
- Argo CD
- GitOps
