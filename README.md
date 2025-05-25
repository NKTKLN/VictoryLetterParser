# ✉️ Letters Parser

**Letters Parser** — асинхронное приложение для парсинга писем с архивного сайта [pismapobedy.ru](https://pismapobedy.ru/letters). Позволяет собирать структурированные данные о письмах (дата, автор, текст, отправитель, получатель и др.) и сохранять их в базу данных PostgreSQL.

## 📦 Зависимости

- [Python 3.12+](https://www.python.org/downloads/)
- [Poetry](https://python-poetry.org/docs/#installation)
- [Playwright](https://playwright.dev/python/) (устанавливается через Poetry)
- [PostgreSQL](https://www.postgresql.org/) (для хранения данных)
- [Task](https://taskfile.dev/) (опционально, для автоматизации)

## 📂 Структура проекта

- **app/** — основной код приложения:
  - `main.py` — точка входа, запуск парсера и планировщика задач (APScheduler)
  - `parse.py` — асинхронные парсеры для сбора писем
  - `logger.py` — настройка логирования
  - `config.py` — загрузка и хранение конфигурации из `.env`
  - **db/** — работа с базой данных:
    - `database.py` — подключение, инициализация и сессии PostgreSQL
    - `crud.py` — CRUD-операции для писем (создание, подсчёт и др.)
    - `models.py` — ORM-модели для хранения писем
- **tests/** — тесты для парсеров и бизнес-логики:
  - `test_parsers.py` — асинхронные тесты для парсеров писем
  - `conftest.py` — фикстуры и моки для тестирования парсеров
- **Taskfile.yml** — сценарии для автоматизации (установка, запуск, тесты, форматирование и др.)
- **pyproject.toml** — конфигурация зависимостей Poetry
- **Dockerfile** — сборка контейнера приложения
- **README.md** — документация проекта
- **LICENSE** — лицензия проекта

## 🛠️ Установка и запуск

### 1. Клонируйте репозиторий

```bash
git clone https://github.com/NKTKLN/VictoryLetterParser
cd VictoryLetterParser 
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

## 🐞 Тестирование

В проекте используются асинхронные тесты на базе **pytest** и **pytest-asyncio** для проверки парсеров и логики сбора писем.

### Запуск тестов

```bash
poetry run pytest tests/ -v
```

Или с помощью [Taskfile](https://taskfile.dev/):

```bash
task test
```

### Описание тестов

- **tests/test_parsers.py** — покрывает:
  - Извлечение идентификаторов писем из HTML (`LetterIdsParser`)
  - Парсинг данных одного письма (`LetterParser`)
  - Интеграционный тест полного процесса (`Parser`)
- Для имитации сетевых запросов и HTML используется `unittest.mock.AsyncMock` и `patch`.
- Тесты не требуют реального подключения к сайту или базе данных.

### Проверка покрытия

Для проверки покрытия кода тестами:

```bash
task coverage
```

## 📜 Лицензия

Этот проект распространяется под лицензией MIT. Подробнее см. в файле [LICENSE](./LICENSE).
