![A workflow status badge](https://github.com/KatyaSoloveva/kittygram_final/actions/workflows/main.yml/badge.svg)
# Проект Kittygram


* **Описание**: Kittygram - социальная сеть для обмена фотографиями котиков. Вы можете добавить фотографию вашего кота, а также его имя, год рождения, цвет и описание достижений, у вас есть возможность редактирования своих записей. Проект также предоставляет открытый API.
* **Стек**  
  Django, Django REST Framework, Gunicorn, Nginx, PostgreSQL, Docker
* **Установка**  
Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/KatyaSoloveva/kittygram_final.git
```  

```
cd kittygram_final
```
Создать и заполнить .env, который должен содержать:
```
POSTGRES_USER=django_user
POSTGRES_PASSWORD=mysecretpassword
POSTGRES_DB=django
DB_HOST=db
DB_PORT=5432
SECRET_KEY='указать SECRET_KEY'
ALLOWED_HOSTS='указать доменное имя или IP хоста'
DEBUG=False
SQLITE=False
```
Запустить Docker Compose в режиме демоны
```
docker compose -f docker-compose.production.yml up -d
```
Выполнить миграции, собрать статические файлы бэкенда и скопировать их в /backend_static/static/
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```
Чтобы использовать CI/CD, нужно перейти в настройки репозитория — Settings, выбрать на панели слева Secrets and Variables → Actions, нажмать New repository secret и добавить следующие переменные:
```
DOCKER_PASSWORD - пароль для Docker Hub
DOCKER_USERNAME - логин  для Docker Hub
HOST - IP-адрес вашего сервера
SSH_KEY - ваш закрытый SSH-ключ
SSH_PASSPHRASE -  содержимое текстового файла с закрытым SSH-ключом
USER - ваше имя пользователя
TELEGRAM_TO - ID своего телеграм-аккаунта
TELEGRAM_TOKEN - токен вашего бота
```
Проект будет проверен на соответствие PEP8, а также пройдет автоматические тесты. Будут собраны образы проекта и отправлены на Docker Hub, обновлены образы на сервере и перезапущено приложение при помощи Docker Compose, выполнены команды для сборки статики в приложении бэкенда, а статика перенесена в volume, выполнены миграции. Затем вам будет отправлено смс Telegram об успешном завершении деплоя.

* **Created by Ekaterina Soloveva**  
https://github.com/KatyaSoloveva
