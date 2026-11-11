# `devops-diplom-app` - `Рахманов Александр`  

Тестовое приложение содержит статичную `HTML-страницу` и `Dockerfile` с nginx, который будет отдавать статические данные html-страницы.  

**GitHub Actions.**  

В файле [ci-cd.yaml](https://github.com/SLOV1977/devops-diplom-app/tree/main/.github/workflows/ci-cd.yaml) настроено:  
- При любом коммите в репозиторий с тестовым приложением происходит сборка и отправка в регистр Docker образа.  
- При создании тега (например, v1.0.0) происходит сборка и отправка с соответствующим label в регистри, а также деплой соответствующего Docker образа в кластер Kubernetes.

### Как это работает:  

**1.** Коммит в ветку main (без тега):  

```
git commit -am "added README.md"
git push origin main
```

![build-and-push по коммиту](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/001.png)

![build-and-push по коммиту](img/001.png)

Запускается только `build-and-push`  

![build-and-push в DockerHub](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/002.png)

![build-and-push в DockerHub](img/002.png)

Образ помечается как `slov1977/devops-diplom-app:20261111-abc123`  

![Образ в DockerHub](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/003.png)

![Образ в DockerHub](img/003.png)

`deploy` - НЕ запускается  

**2.** Создание тега:  

```
git tag v1.0.1
git push origin v1.0.1
```

![v1.0.1](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/004.png)

![v1.0.1](img/004.png)

Запускаются обе jobs: `build-and-push` и `deploy`  

![build-and-push и deploy в DockerHub](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/005.png)

![build-and-push и deploy в DockerHub](img/005.png)

Образ помечается как `slov1977/devops-diplom-app:v1.0.1`  

![Образ с тегом v1.0.1 в DockerHub](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/006.png)

![Образ с тегом v1.0.1 в DockerHub](img/006.png)

Приложение деплоится в кластер `Kubernetes`  

![Страница приложения v1.0.1 в браузере](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/007.png)

![Страница приложения v1.0.1 в браузере](img/007.png)

**3.** Коммит с тегом (альтернативный способ):  

```
git commit -am "release v1.0.3"
git tag v1.0.3
git push origin main --tags
```
![v1.0.3 + коммит](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/008.png)

![v1.0.3 + коммит](img/008.png)

Пуш `git push origin main --tags` создаёт два отдельных запуска `workflow`:  

1. При пуше в `main` запустится `build-and-push` с SHA-тегом  

2. При пуше тега v1.0.3 запустится `build-and-push` и `deploy` с версионным тегом  

![Два отдельных запуска `workflow`](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/009.png)

![Два отдельных запуска `workflow`](img/009.png)


![`build-and-push` приложения v1.0.3](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/010.png)

![`build-and-push` приложения v1.0.3](img/010.png)


![`build-and-push` и `deploy` приложения v1.0.3](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/011.png)

![`build-and-push` и `deploy` приложения v1.0.3](img/011.png)


![Страница приложения v1.0.3 в браузере](https://github.com/SLOV1977/devops-diplom-app/tree/main/img/012.png)

![Страница приложения v1.0.3 в браузере](img/012.png)
