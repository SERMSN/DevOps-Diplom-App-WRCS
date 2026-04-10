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
- `APP_HOST` (optional, default: `app.wrcs.su`)

## Local Build

```bash
docker build -t diploma-app:local .
```

## Release Verification

Ниже короткий сценарий для демонстрации CI/CD.

### 1. Проверка push в master

```bash
git add index.html
git commit -m "Update app page for release verification"
git push origin master
```

Ожидаемо:

- в GitHub Actions запускается `App CI/CD`
- выполняются `build -> push -> deploy`
- в кластере появляется обновлённая страница

### 2. Проверка деплоя по тегу

```bash
git tag v1.0.2
git push origin v1.0.2
```

Ожидаемо:

- workflow запускается по тегу `v1.0.2`
- image публикуется с тегом `v1.0.2`
- deployment обновляется на image с тегом `v1.0.2`

### 3. Команда финальной проверки в Kubernetes

```bash
kubectl -n app get deploy diploma-app -o=jsonpath='{.spec.template.spec.containers[0].image}'; echo
```

Ожидаемый формат:

```text
cr.yandex/<REGISTRY_ID>/diploma-app:v1.0.2
```
