# E-Commerce DevOps Implementation

## 项目概述

本项目基于 [OpenTelemetry Demo](https://github.com/open-telemetry/opentelemetry-demo) 进行 DevOps 实践改造。

我从原始微服务项目中选取了三个服务：

- Ad Service
- Product Catalog Service
- Recommendation Service

目标是为这三个服务构建一套完整的端到端 DevOps 流程，包括本地容器化、CI 镜像构建、镜像推送、基础设施自动化、Kubernetes 声明式部署，以及基于 Argo CD 的 GitOps 持续交付。

本项目采用双仓库设计：

- `e-commerce-devops-app`：应用源码、Dockerfile、测试、GitHub Actions CI、镜像构建与推送
- `e-commerce-devops-infra`：Terraform、EKS、Kubernetes manifests、Argo CD Application、GitOps deployment

## 项目目标

- 为三个不同语言的微服务编写和优化 Dockerfile
- 使用 GitHub Actions 自动完成测试、构建、镜像扫描和推送
- 使用 AWS ECR 作为容器镜像仓库
- 使用 Terraform 创建 AWS VPC、EKS Cluster、Node Group 和相关 IAM 资源
- 使用 Kubernetes YAML 以声明式方式定义服务部署
- 使用 Argo CD 实现 GitOps 持续交付
- 实现从代码提交到 EKS 自动部署的完整流程
- 记录部署过程、故障排查和回滚方式，用于面试展示和复盘

## 技术栈

| 分类 | 技术 |
|---|---|
| Source Control | GitHub |
| CI/CD | GitHub Actions, Argo CD |
| Container | Docker |
| Container Registry | AWS ECR |
| Infrastructure as Code | Terraform |
| Kubernetes Platform | AWS EKS |
| Deployment | Kubernetes YAML, GitOps |
| Services | Go, Java, Python |
| Security | GitHub Secrets, IAM, Image Scan |