# Kittygram

Kittygram — учебное веб-приложение для публикации карточек котов. Пользователи могут зарегистрироваться, войти в аккаунт, просматривать карточки, добавлять фотографии и достижения, редактировать и удалять свои карточки.

## Стек

- Бэкенд — Python, Django, Django REST Framework, Djoser и Gunicorn.
- Фронтенд — JavaScript и React.
- База данных — PostgreSQL 13.
- Прокси и раздача статики и медиа — Nginx.
- Контейнеры и автоматизация — Docker Compose, Docker Hub и GitHub Actions.

## Локальный запуск

Установите Docker с плагином Compose. Склонируйте [репозиторий Kittygram](https://github.com/SmashEA/kittygram_final) и откройте его корневую папку в терминале.

1. Создайте файл `.env` рядом с `.env.example`. Скопируйте в него содержимое шаблона и заполните переменные по таблице ниже.
2. Запустите приложение из опубликованных образов:

   ```bash
   docker compose -p kittygram -f docker-compose.production.yml up -d
   docker compose -p kittygram -f docker-compose.production.yml ps -a
   ```

3. Откройте [http://localhost:9000/](http://localhost:9000/). При запуске бэкенд применяет миграции и собирает статику. Контейнер `frontend` копирует сборку в общий том и штатно завершается с кодом `0`.

Для сборки контейнеров из исходников используйте `docker compose -f docker-compose.yml up --build -d`.

### Переменные окружения

| Переменная | Что указать |
| --- | --- |
| `POSTGRES_DB` | Имя базы данных, например `kittygram`. |
| `POSTGRES_USER` | Имя пользователя базы, например `kittygram_user`. |
| `POSTGRES_PASSWORD` | Собственный пароль пользователя базы. |
| `DB_HOST` | `db`, имя сервиса PostgreSQL в Docker Compose. |
| `STATIC_ROOT` | `/static/static`, путь для статики Django в общем томе. |
| `DJANGO_ALLOWED_HOSTS` | `localhost,127.0.0.1`. При проксировании через локальный Nginx добавьте `kittygram.localhost` через запятую. |
| `DJANGO_SECRET_KEY` | Уникальный случайный секретный ключ Django. |

Храните пароли и ключи в локальном `.env`. В репозитории хранится шаблон `.env.example`.

## Автоматические проверки

GitHub Actions проверяет бэкенд с Ruff и запускает тесты Django и React. После успешных проверок при пуше в `main` workflow публикует три образа Kittygram в Docker Hub и отправляет уведомление в Telegram.
