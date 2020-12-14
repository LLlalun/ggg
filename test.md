# test

```sh
git clone https://github.com/terraform.git
cd terraform
git checkout -b chaos-mesh
```

## Создать папку chaos-mesh

```sh
cd helm
mkdir chaos-mesh
```

## Добавить репозиторий чарта и скачать его в папку chaos-mesh

```sh
#example
helm repo add chaos-mesh https://charts.chaos-mesh.org
cd chaos-mesh
helm pull chaos-mesh/chaos-mesh  --untar
```

## Скачать необходимые prerequisites CRD или другие ямл файлы

```sh
#example
curl -sSL https://mirrors.chaos-mesh.org/v1.0.1/crd.yaml -o crd.yaml
```

```sh
helm show values chaos-mesh/chaos-mesh > test.yaml
```

При необходимости сконфигурировать чарт в этом файлике

### Подготовить переменные для CI и создать для каждой среды джобу по установке:

```yaml
test_chaos-mesh: #_# Название джобы
  stage: helm_test #_# Стейдж для джобы может быть: helm_test helm_prod
  extends: .helm_install_community_chart
  variables:
    KUBER_ENV: test #_# Среда куда ставим может быть: test prod glc
    CHART_DIR: chaos-mesh #_# Директория куда мы скачивали файлики
    RELEASE_NAME: chaos-mesh #_# Название релиза приложения в k8s может быть любым
    CHART_NAME: chaos-mesh #_# Название чарта в репозитории может быть любым
    CHART_REPO_NAME: chaos-mesh #_# Название репозитория может быть любым
    CHART_REPO_URL: https://charts.chaos-mesh.org #_# URL до репы где лежит чарт может быть любым
    CHART_NS: chaos-mesh #_# Namespace куда будет установлен чарт может быть любым
    CHART_EXTRA: --version v0.3.1 #_# Дополнительные конфиги которые будут переданы helmу в командную строку пример:'--atomic --debug --set privet=true'
    CHART_ADDONS_BEFORE: crd.yaml #_# Файлы с аддонами которые будут отправлены в k8s ДО установки чарта перечисление файлов через пробел Пример: privet.yaml poka.yaml privet-privet.yaml
    CHART_ADDONS_AFTER: '' #_# Файлы с аддонами которые будут отправлены в k8s ПОСЛЕ установки чарта перечисление файлов через пробел Пример: privet.yaml poka.yaml privet-privet.yaml
```

```git
git add .
git commit -m 'add new app chaos-mesh'
git push origin chaos-mesh
```

### Проверить вывод CI и замерджить в мастер для установки
