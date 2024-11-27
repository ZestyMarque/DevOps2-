
# DevOps Lab2*. Работа с Docker Compose
### Требования:
- Написать “плохой” Docker Compose файл, в котором есть не менее трех “bad practices” по их написанию.
- Написать “хороший” Docker Compose файл, в котором эти плохие практики исправлены.
- В Readme описать каждую из плохих практик в плохом файле, почему она плохая и как в хорошем она была исправлена, как исправление повлияло на результат.
- После предыдущих пунктов в хорошем файле настроить сервисы так, чтобы контейнеры в рамках этого compose-проекта так же поднимались вместе, но не "видели" друг друга по сети. В отчете описать, как этого добились и кратко объяснить принцип такой изоляции.
## 1. Создание "плохого" Docker Compose файла

Для начала установим Docker и Docker-compose:
```
sudo apt-get update
sudo apt-get install -y docker.io docker-compose
```
В рабочий директории создадим плохой Docker compose файл с тремя плохими практиками:
- При использование `:latest` при каждом запуске загружается самая новая версия образа, что приводит к непредсказуемому поведению.
- `privileged: true` создают угрозу безопасности.
- Секреты хранятся в переменных средах, поэтому могут быть легко считаны из контейнера.
```
version: '3.8'

services:
  app1:
    image: ubuntu:latest
    command: bash -c "apt-get update && apt-get install -y curl && curl app2"
    privileged: true
    environment:
      - SECRET_KEY=secret1

  app2:
    image: ubuntu:latest
    command: bash -c "apt-get update && apt-get install -y curl && curl app1"
    privileged: true
    ports:
      - "80:8080"
```

## 2. Создание "хорошего" Docker Compose файла

