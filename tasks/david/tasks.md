# Задачи Давида (Backend)

## Основные задачи

### 1. Система планирования сообщений
- Реализация модели `ScheduledMessage`
- Реализация модели `MessageHistory`
- Реализация модели `FloodControl`
- Настройка миграций для новых моделей

### 2. Сервис планировщика
- Реализация сервиса `scheduler_service.py`:
  ```python
  class MessageScheduler:
      def __init__(self):
          self.telegram_client = None
          
      async def schedule_message(self, message_data):
          # Создание запланированного сообщения
          pass
          
      async def process_scheduled_messages(self):
          # Обработка запланированных сообщений
          pass
          
      async def handle_flood_control(self, account, group):
          # Обработка ограничений отправки
          pass
  ```

### 3. Система отправки сообщений
- Реализация очереди сообщений
- Обработка ошибок при отправке
- Сохранение истории отправки
- Базовый механизм повторных попыток

### 4. API Endpoints для сообщений
- Реализация API для работы с сообщениями:
  ```python
  # views.py
  from rest_framework import viewsets
  
  class ScheduledMessageViewSet(viewsets.ModelViewSet):
      queryset = ScheduledMessage.objects.all()
      serializer_class = ScheduledMessageSerializer
      
      @action(detail=True, methods=['post'])
      def schedule(self, request, pk=None):
          # Логика планирования сообщения
          pass
          
  class MessageHistoryViewSet(viewsets.ReadOnlyModelViewSet):
      queryset = MessageHistory.objects.all()
      serializer_class = MessageHistorySerializer
  ```

### 5. Базовый механизм контроля флуда
- Реализация простой системы задержек
- Отслеживание ограничений Telegram
- Базовая очередь сообщений

## Технические требования
- Обеспечить асинхронную работу с Telegram API
- Реализовать базовую обработку ошибок
- Обеспечить сохранение истории отправки
- Реализовать простой механизм повторных попыток

## Интеграционные точки с задачами Эрика
- Использование TelegramClient для отправки сообщений
- Работа с общими моделями данных
- Интеграция с API endpoints для групп и аккаунтов 