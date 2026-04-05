# DevOps Diploma App

Отдельный репозиторий тестового приложения для дипломного проекта.

## Contents

- `Dockerfile`
- `nginx.conf`
- `index.html`
- `k8s/`
- `.github/workflows/app-ci-cd.yml`

## Purpose

Этот репозиторий отвечает только за приложение:

- сборка Docker image
- push в `Yandex Container Registry`
- деплой в Kubernetes

## Deployment

Приложение публикуется через:

- `Deployment`
- `Service`
- `Ingress`

Манифесты лежат в каталоге `k8s/`.

## GitHub Actions Secrets

Для workflow нужны secrets:

- `YC_TOKEN`
- `YC_CLOUD_ID`
- `YC_FOLDER_ID`
- `YC_K8S_CLUSTER_ID`
- `YC_REGISTRY_ID`

## Local Build

```bash
docker build -t diploma-app:local .
```
