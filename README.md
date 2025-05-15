# ✉️ Letters Parser

**Letters Parser** — асинхронное приложение для парсинга писем с архивного сайта [pismapobedy.ru](https://pismapobedy.ru/letters). Позволяет собирать структурированные данные о письмах (дата, автор, текст, отправитель, получатель и др.) и сохранять их в базу данных PostgreSQL.

## 📦 Зависимости

- [Python 3.12+](https://www.python.org/downloads/)
- [Poetry](https://python-poetry.org/docs/#installation)
- [Playwright](https://playwright.dev/python/) (устанавливается через Poetry)
- [PostgreSQL](https://www.postgresql.org/) (для хранения данных)
- [Task](https://taskfile.dev/) (опционально, для автоматизации)

## 📂 Структура проекта

- **app**: Основной код приложения:
  - `main.py`: Точка входа, запуск парсера и планировщика задач (APScheduler).
  - `parse.py`: Асинхронные парсеры для сбора писем.
  - `logger.py`: Настройка логирования.
  - `config.py`: Загрузка и хранение конфигурации из `.env`.
  - **db/**: Работа с базой данных:
    - `database.py`: Подключение, инициализация и сессии PostgreSQL.
    - `crud.py`: CRUD-операции для писем (создание, подсчёт и др.).
    - `models.py`: ORM-модели для хранения писем.
    - `__init__.py`: Инициализация пакета.
- **Taskfile.yml**: Сценарии для автоматизации (установка, запуск, форматирование и др.).
- **pyproject.toml**: Конфигурация зависимостей Poetry.
- **README.md**: Документация проекта.
- **LICENSE**: Лицензия проекта.

## 🛠️ Установка и запуск

### 1. Клонируйте репозиторий

```bash
git clone https://github.com/NKTKLN/hackathon-parser-module
cd hackathon-parser-module
```

### 2. Установите зависимости

```bash
poetry install --no-root
poetry run playwright install
```

### 3. Настройте переменные окружения

Создайте файл `.env` (или используйте `.env.example` как шаблон):

```env
LETTERS_COUNT=100

POSTGRES_PASSWORD=your_password
POSTGRES_USER=your_user
POSTGRES_DB=your_db
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
```

### 4. Запустите приложение

```bash
poetry run python -m app.main
```

- Парсер автоматически создаст таблицы в базе данных и начнёт сбор писем.
- По умолчанию задача парсинга запускается каждые 12 часов (APScheduler).
- Все логи выводятся в консоль и/или файл (см. настройки логгера).

## 🐳 Запуск с помощью Docker

### 1. Соберите и запустите контейнеры

```bash
docker compose up --build
```

### 2. Остановите контейнеры

```bash
docker compose down
```

### ⚙️ Использование Taskfile (опционально)

Для удобства используйте [Task](https://taskfile.dev/):

```bash
task install
task playwright-install
task run
```

## ⚡ Возможности

- Асинхронный парсинг с помощью Playwright.
- Гибкая настройка количества писем для сбора через `.env`.
- Хранение данных в PostgreSQL.
- Логирование процесса.
- Планировщик задач (APScheduler) — автоматический запуск парсинга по расписанию.

<!-- ## 🐞 Тестирование

(Добавьте инструкции по тестированию, если есть тесты.) -->

## 📜 Лицензия

Этот проект распространяется под лицензией MIT. Подробнее см. в файле [LICENSE](./LICENSE).
