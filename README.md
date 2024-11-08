
# Server Software Development Technologies 2024

## Опис
Цей проєкт є веб-застосунком на Flask для демонстрації базових серверних технологій. Містить основний ендпоінт `healthcheck` та структуру для подальшої розробки бізнес-логіки.

## Встановлення та Налаштування

### 1. Клонування проєкту
Клонуйте репозиторій:
```bash
git clone https://github.com/miraclegott/ServerSoftwareDevelopmentTechnologies2024.git
cd ServerSoftwareDevelopmentTechnologies2024
```

### 2. Налаштування Python середовища
> **Примітка:** Цей крок є опціональним, ви можете одразу працювати з Docker.

#### Встановлення Python з Pyenv
Рекомендуємо використовувати `pyenv` для керування версіями Python: [Pyenv на GitHub](https://github.com/pyenv/pyenv).

#### Створення та активація віртуального середовища
1. Створіть віртуальне середовище:
   ```bash
   python3 -m venv env
   ```
2. Активуйте віртуальне середовище:
   ```bash
   source ./env/bin/activate (Linux)
   ./env/Scripts/activate (Windows)
   ```

#### Встановлення залежностей
1. Встановіть Flask:
   ```bash
   pip install flask
   ```
2. Запишіть залежності в `requirements.txt`:
   ```bash
   pip freeze > requirements.txt
   ```

### 3. Створення структури проєкту
1. Створіть папку для модуля додатку.
2. У цій папці створіть файли `__init__.py` та `views.py`.

### 4. Налаштування Flask-додатку
- У файлі `__init__.py` ініціалізуйте Flask-додаток:
  ```python
  from flask import Flask
  
  app = Flask(__name__)
  
  import <your_module>.views
  ```
  Замініть `<your_module>` на назву вашого модуля.
  
- У файлі `views.py` реалізуйте базовий ендпоінт `healthcheck`:
  ```python
  from flask import jsonify
  from . import app
  
  @app.route('/healthcheck')
  def healthcheck():
      return jsonify(status="OK"), 200
  ```

### 5. Запуск локального Flask-додатку
Запустіть застосунок командою:
```bash
flask run --host 0.0.0.0 -p <your_port>
```

## Налаштування Docker

### 1. Створення Dockerfile
У кореневій папці створіть файл `Dockerfile`:
```dockerfile
FROM python:3.11.3-slim-bullseye

WORKDIR /app

COPY requirements.txt .

RUN python -m pip install -r requirements.txt

COPY . /app

CMD flask --app <your app name> run -h 0.0.0.0 -p $PORT
```
> Замініть `<your app name>` на назву вашого модуля.

### 2. Білд Docker-образу
Збілдьте Docker-образ:
```bash
docker build . -t <image_name>:latest
```

### 3. Запуск Docker-контейнера
Запустіть контейнер:
```bash
docker run -it --rm --network=host -e PORT=<your_port> <image_name>:latest
```

## Використання Docker Compose

### Створення docker-compose.yaml
Для спрощення запуску контейнера, створіть файл `docker-compose.yaml`:
```yaml
version: '3'

services:
  <app_name>:
    restart: always
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      PORT: "<your_port>"
    ports:
      - "<your_port>:8080"
```
> Замініть `<app_name>` та `<your_port>` на відповідні назви та порт.

### Запуск з Docker Compose
1. Виконайте команду для збілду:
   ```bash
   docker-compose build
   ```
2. Запустіть контейнер:
   ```bash
   docker-compose up
   ```

## Деплой на Render.com

### 1. Реєстрація та налаштування
1. Зареєструйтесь на [Render.com](https://render.com/).
2. Створіть новий сервіс типу **Web Service** і підключіть репозиторій.
3. Виберіть бажану гілку, порт, та ім'я проєкту.

### 2. Деплой та тестування
1. Render автоматично почне деплой. 
2. Після завершення ваш застосунок буде доступний за URL-адресою на сторінці дашборду Render.
