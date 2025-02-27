# Задачи Эрика (Backend)

## Основные задачи

### 1. Настройка проекта Django

- Инициализация проекта Django
- Настройка виртуального окружения
- Установка необходимых зависимостей
- Настройка базовой структуры проекта

### 2. Разработка моделей данных

- Реализация модели `TelegramAccount`
- Реализация модели `TelegramGroup`
- Реализация модели `MessageTemplate`
- Настройка миграций базы данных

### 3. Интеграция с Telegram (Telethon)

- Настройка подключения к Telegram API
- Реализация сервиса `telegram_client.py`:
  ```python
  class TelegramClient:
      def __init__(self, api_id, api_hash, phone):
          self.api_id = api_id
          self.api_hash = api_hash
          self.phone = phone

      async def connect(self):
          # Подключение к Telegram
          pass

      async def send_message(self, chat_id, message):
          # Отправка сообщения
          pass

      async def get_dialogs(self):
          # Получение списка диалогов
          pass
  ```

### 4. API Endpoints

- Реализация базовых API endpoints для:
  - Управления аккаунтами Telegram
  - Управления группами
  - Работы с шаблонами сообщений

Пример структуры API:

```python
# views.py
from rest_framework import viewsets

class TelegramAccountViewSet(viewsets.ModelViewSet):
    queryset = TelegramAccount.objects.all()
    serializer_class = TelegramAccountSerializer

class TelegramGroupViewSet(viewsets.ModelViewSet):
    queryset = TelegramGroup.objects.all()
    serializer_class = TelegramGroupSerializer

class MessageTemplateViewSet(viewsets.ModelViewSet):
    queryset = MessageTemplate.objects.all()
    serializer_class = MessageTemplateSerializer
```

### 5. Базовая админ-панель

- Настройка админ-интерфейса для управления:
  - Telegram аккаунтами
  - Группами
  - Шаблонами сообщений

## Технические требования

- Использовать Django 4.2+
- Использовать Telethon для работы с Telegram API
- Обеспечить базовую обработку ошибок
- Реализовать простую систему логирования

## Интеграционные точки с задачами Давида

- API endpoints для работы с сообщениями
- Сервис отправки сообщений
- Общие модели данных
