# Домашнее задание к уроку №1
Необходимо собрать минимальный Python-проект, который другой разработчик может открыть, настроить, запустить и продолжить развивать.

Это задание поможет закрепить знания о работе с виртуальным окружением, Git, README, пакетами Python, типизацией, коллекциями, исключениями и логированием.
## Что нужно сделать
Нужно подготовить Git-репозиторий backend-basics с двумя этапами:
1. Окружение, зависимости, .gitignore и первый README.
2. Пакет с кодом, точка запуска, обновлённый README и проверка Ruff.

## Формат сдачи
Ссылка на GitHub-репозиторий

---
## Задание
### Часть 1. Подготовить репозиторий.
1. Проверь, что установлены Python 3.12.x, Git и VSCode.
2. Создай папку проекта `backend-basics`.
3. Инициализируй локальный репозиторий:
```bash
git init -b main
```
4. Создай и активируй виртуальное окружение .venv.
5. Установи Ruff в виртуальное окружение.
6. Зафиксируй зависимости в `requirements.txt`.
7. Создай `.gitignore` минимум с такими правилами:
```gitignore
.venv/
__pycache__/
*.pyc
.ruff_cache/
```
8. Создай `README.md` и перенеси в него готовый текст:
```markdown
# Backend Basics
Учебный проект для знакомства со структурой Python-проекта.

## Требования
Python 3.12
```
9. Проверь изменения через git status.
10. Сделай первый коммит. Сообщение должно описывать сделанный шаг, например:
```bash
git add .gitignore README.md requirements.txt
git commit -m "Initialize project environment"
```

### Часть 2. Добавить код.
Создай структуру:
```text
backend-basics/
├── src/
│   ├── __init__.py
│   ├── exceptions.py
│   ├── logger.py
│   ├── models.py
│   └── services.py
├── .gitignore
├── main.py
├── README.md
└── requirements.txt
```

Обрати внимание на имена:
- `backend-basics `— папка проекта и репозитория
- `src` — импортируемый Python-пакет.

**Модель задачи**

Перенеси в `src/models.py`:
```python
from dataclasses import dataclass
from datetime import date


@dataclass
class Task:
    id: int
    title: str
    description: str
    due_date: date
```

**Исключение**

Перенеси в src/exceptions.py:
```python
class TaskNotFoundError(Exception):
    pass
```

**Логирование**

Перенеси в src/logger.py:
```python
import logging


logging.basicConfig(level=logging.INFO)
logger = logging.getLogger("tasks")

```

**Сервисные функции**

Перенеси в src/services.py:
```python
from datetime import date

from src.exceptions import TaskNotFoundError
from src.logger import logger
from src.models import Task


def create_task(
    tasks_by_id: dict[int, Task],
    title: str,
    description: str,
    due_date: date,
) -> Task:
    task_id = max(tasks_by_id, default=0) + 1
    task = Task(
        id=task_id,
        title=title,
        description=description,
        due_date=due_date,
    )
    tasks_by_id[task_id] = task
    logger.info("Created task", extra={"task_id": task_id})
    return task


def get_task(
    tasks_by_id: dict[int, Task],
    task_id: int,
) -> Task:
    if task_id not in tasks_by_id:
        raise TaskNotFoundError(task_id)
    return tasks_by_id[task_id]
```

Этот код:
- создаёт задачу и сохраняет её в словаре
- записывает создание задачи в лог
- возвращает задачу по идентификатору
- выбрасывает TaskNotFoundError, если задачи нет.

**Точка запуска**

Перенеси в main.py:
```python
from datetime import date

from src.models import Task
from src.services import create_task, get_task


def main() -> None:
    tasks_by_id: dict[int, Task] = {}
    task = create_task(
        tasks_by_id,
        title="Подготовить README",
        description="Добавить инструкцию запуска проекта",
        due_date=date(2027, 6, 30),
    )
    print(get_task(tasks_by_id, task.id))


if __name__ == "__main__":
    main()
```

Файл src/__init__.py оставь пустым.


### Часть 3. Завершить работу.
1. Добавь в README раздел запуска:
```markdown
## Запуск

`python main.py`
```

2. Запусти проект:
```bash
python main.py
```

Пример ожидаемого результата:
```text
INFO:tasks:Created task
Task(id=1, title='Подготовить README', description='Добавить инструкцию запуска проекта', due_date=datetime.date(2027, 6, 30))
```

3. Проверь и отформатируй код:
```bash
ruff check .
ruff format .
```
4. Еще раз запусти проект после форматирования.
5. Проверь состав изменений через git status.
6. Сделай второй осмысленный коммит, например:
```bash
git add .
git commit -m "Add task model and service logic"
```
7. Проверь историю:
```bash
git log --oneline
```
8. Подготовь ссылку на GitHub-репозиторий