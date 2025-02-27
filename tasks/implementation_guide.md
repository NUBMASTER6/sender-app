# Руководство по интеграции

## Общая структура проекта

```
telegram_sender/
├── sender/
│   ├── models/
│   │   ├── __init__.py
│   │   ├── account.py        # Модели Эрика
│   │   ├── group.py         # Модели Эрика
│   │   ├── template.py      # Модели Эрика
│   │   ├── message.py       # Модели Давида
│   │   ├── history.py       # Модели Давида
│   │   └── flood.py         # Модели Давида
│   ├── services/
│   │   ├── __init__.py
│   │   ├── telegram_client.py   # Эрик
│   │   └── scheduler_service.py # Давид
│   ├── api/
│   │   ├── __init__.py
│   │   ├── views/
│   │   │   ├── account.py      # Эрик
│   │   │   ├── group.py        # Эрик
│   │   │   ├── template.py     # Эрик
│   │   │   └── message.py      # Давид
│   │   └── urls.py
│   └── admin.py
```

## Порядок интеграции

1. **Базовая настройка (Эрик)**
   - Инициализация проекта
   - Настройка окружения
   - Создание базовых моделей

2. **Подключение Telegram (Эрик)**
   - Реализация TelegramClient
   - Настройка авторизации
   - Базовые операции с API

3. **Система сообщений (Давид)**
   - Реализация моделей сообщений
   - Создание планировщика
   - Интеграция с TelegramClient

4. **API интеграция**
   - Эрик: API для управления аккаунтами и группами
   - Давид: API для работы с сообщениями
   - Общая настройка маршрутизации

## Точки взаимодействия

### 1. Сервисный слой
```python
# services/telegram_client.py (Эрик)
class TelegramClient:
    async def send_message(self, chat_id, message):
        pass

# services/scheduler_service.py (Давид)
class MessageScheduler:
    def __init__(self, telegram_client: TelegramClient):
        self.telegram_client = telegram_client
```

### 2. Модели данных
```python
# models/message.py (Давид)
class ScheduledMessage(models.Model):
    account = models.ForeignKey('TelegramAccount')  # Модель Эрика
    groups = models.ManyToManyField('TelegramGroup')  # Модель Эрика
```

### 3. API Endpoints
```python
# api/urls.py
urlpatterns = [
    # Эрик
    path('accounts/', include('sender.api.views.account')),
    path('groups/', include('sender.api.views.group')),
    # Давид
    path('messages/', include('sender.api.views.message')),
]
```

## Запуск и тестирование

1. **Подготовка**
   ```bash
   python manage.py migrate
   python manage.py createsuperuser
   ```

2. **Порядок тестирования**
   - Добавление Telegram аккаунта
   - Авторизация в Telegram
   - Добавление групп
   - Создание тестового сообщения
   - Проверка отправки

## Важные моменты

1. **Обработка ошибок**
   - Все ошибки Telegram API логируются
   - Базовый механизм повторных попыток
   - Простой контроль флуда

2. **Асинхронность**
   - TelegramClient работает асинхронно
   - Планировщик использует асинхронные вызовы
   - Django REST Framework синхронный

3. **Безопасность**
   - Сессии Telegram хранятся в зашифрованном виде
   - Базовая аутентификация Django
   - Простое логирование действий

## Ограничения прототипа

1. Нет сложной системы кэширования
2. Простая обработка ошибок
3. Базовый механизм повторных попыток
4. Отсутствие сложной оптимизации
5. Простой механизм контроля флуда 