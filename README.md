# Backend Basics

Учебный проект для знакомства со структурой Python-проекта.

## Требования

Python 3.12.x

Git и VS Code для работы над проектом. Единственная dev-зависимость — Ruff,
её версия зафиксирована в `requirements.txt`. Код использует стандартную библиотеку.

## Подготовка окружения

Из корня проекта, macOS/Linux:

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Windows PowerShell:

```powershell
py -3.12 -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

В новом терминале снова активируйте `.venv`. Проверить версию: `python --version`.

## Запуск

```bash
python main.py
```

Пример результата (лог идёт в stderr, задача — в stdout):

```text
INFO:tasks:Created task
Task(id=1, title='Подготовить README', description='Добавить инструкцию запуска проекта', due_date=datetime.date(2027, 6, 30))
```

## Структура и логика

| Файл | Назначение |
| --- | --- |
| `main.py` | Создаёт словарь, запускает сценарий и печатает результат |
| `src/__init__.py` | Пустой файл, обозначающий Python-пакет |
| `src/models.py` | `Task`: dataclass с типизированными полями |
| `src/exceptions.py` | `TaskNotFoundError` для отсутствующей задачи |
| `src/logger.py` | Настройка стандартного logging |
| `src/services.py` | Создание задачи, сохранение в словарь и поиск по id |
| `.gitignore` | Исключает виртуальное окружение и кеши из Git |
| `requirements.txt` | Зафиксированная версия Ruff |

`dict[int, Task]` связывает id с объектом задачи. Поиск по ключу в среднем
занимает O(1). При создании `max(tasks_by_id, default=0) + 1` выбирает следующий
id, поэтому создание в этом учебном варианте занимает O(n). Данные хранятся
в памяти и исчезают при завершении процесса. Если id отсутствует, `get_task`
выбрасывает `TaskNotFoundError`. Сервис пишет события через logging, а вывод
пользователю остаётся в `main.py`.

## Проверка

```bash
ruff check .
ruff format .
python main.py
git status
git log --oneline
```

## История работы

Первый коммит `Initialize project environment` содержит окружение проекта:
README, зависимости и правила Git. Второй `Add task model and service logic`
добавляет код и инструкцию запуска. Рабочая ветка `homework-1` предназначена
для Pull Request в `main` без слияния до проверки.

## Аутентификация

Программа работает локально: HTTP-запросов, учётных записей, паролей и токенов
в ней нет. Запуск вызывает `main()`, затем `create_task()` и `get_task()`.
