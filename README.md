# E-Commerce DevOps Implementation

## 项目概述

本项目基于 [OpenTelemetry Demo](https://github.com/open-telemetry/opentelemetry-demo) 进行 DevOps 实践改造。

我从原始微服务项目中选取了三个服务：

- Ad Service
- Product Catalog Service
- Recommendation Service

目标是围绕这三个服务构建一套完整的端到端 DevOps 流程，包括 Docker 镜像构建、GitHub Actions CI、AWS ECR 镜像推送、Terraform 基础设施创建、Kubernetes 声明式部署，以及基于 Argo CD 的 GitOps 持续交付。

本项目重点不是简单地运行一个 demo，而是模拟真实团队中的 DevOps 交付流程：应用代码和基础设施配置分离，CI 负责构建不可变镜像，CD 通过 GitOps 方式将声明式配置同步到 AWS EKS。

## 项目目标

- 为 Go、Java、Python 三个不同技术栈的微服务编写 Dockerfile
- 使用 GitHub Actions 自动完成构建、测试和镜像推送
- 使用 AWS ECR 作为容器镜像仓库
- 使用 Terraform 创建 AWS VPC、EKS Cluster、Node Group 和 IAM 相关资源
- 使用 Kubernetes YAML 定义 Deployment、Service、ConfigMap 等资源
- 使用 Argo CD 实现 GitOps 持续交付
- 实现从代码提交到 EKS 自动部署的完整流程
- 记录部署、验证、回滚和故障排查过程，用于项目复盘和面试展示

## 技术栈

| 分类 | 技术 |
|---|---|
| Source Control | GitHub |
| CI | GitHub Actions |
| CD | Argo CD |
| Container | Docker |
| Container Registry | AWS ECR |
| Infrastructure as Code | Terraform |
| Kubernetes Platform | AWS EKS |
| Deployment | Kubernetes YAML, GitOps |
| Application Services | Go, Java, Python |
| Cloud Provider | AWS |

## Repository Design

本项目采用双仓库设计，将应用代码和基础设施配置分离。

### App Repository

`e-commerce-devops-app` 负责应用相关内容：

- 微服务源码
- Dockerfile
- 单元测试
- GitHub Actions CI workflow
- Docker image build
- Push image to AWS ECR

### Infra Repository

`e-commerce-devops-infra` 负责基础设施和部署状态：

- Terraform infrastructure code
- AWS EKS cluster configuration
- Kubernetes manifests
- Argo CD Application manifests
- GitOps desired state

这样设计的原因是：CI pipeline 只负责构建和发布不可变镜像，不直接操作 Kubernetes 集群。部署状态由 Git 中的 Kubernetes manifests 声明，Argo CD 负责将 Git 中的期望状态同步到 EKS，从而实现可追溯、可回滚、声明式的持续交付流程。

## 架构设计

```text
Developer
   |
   | git push / pull request
   v
GitHub Actions
   |
   | build / test / docker build
   v
AWS ECR
   |
   | push image with version tag
   v
Update Infra Repository
   |
   | update Kubernetes image tag
   v
Argo CD
   |
   | detect Git changes and sync desired state
   v
AWS EKS
   |
   v
Ad / Product Catalog / Recommendation Services
