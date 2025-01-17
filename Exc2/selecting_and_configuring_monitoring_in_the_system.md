# Выбор и настройка мониторинга в системе

## Мотивация

- Что позволит добавление мониторинга.
  - Упростить выявление узких мест.
  - Позволит правильно провести масштабирование база данных и приложений.
  - Позволит предупредить если начнется перегрузка серверов.
  - Позволит собирать статистику и оценивать внедряемые фич.
  - Позволит собирать бизнес метрики.

## Выбор подхода

### Internet Shop, MES, Shop DB, Mes DB и 3d file storage, Message Queue

RED. Этот подход хорошо подходит для мониторинга веб-сервисов, запросов к базам данных, очередей и других подобных систем.

### MES API и Shop API

Четыре золотых сигнала. Данный подход позволит проконтролировать аномалии которые могут возникнуть при работе api и быстро на них среагировать.

### CRM API

USE. Данный подход позволит увидеть нагрузку на сервера при расчете модели и на основе этого позволит рассчитать кол-во оборудования нужного для поддержания.

## Метрики

- Number of dead-letter-exchange letters
  - Для кого: Message Queue.
  - Зачем: показывает количество недоставленных сообщений. Указывает на проблему в очереди.  (Частота запросов)
  - Ярлыки: queue_id, startpoint, endpoint, error_id
- Number of message in flight
  - Для кого: Message Queue.
  - Зачем: показывает общее кол-во сообщений, демонстрирует нагрузку на системы. (Насыщенность)
  - Ярлыки: queue_id, startpoint, endpoint
- Response time (latency)
  - Для кого: Shop API, MES API.
  - Зачем: Позволяет измерять время отклика API. (Задержка)
  - Ярлыки: endpoint.
- Number of HTTP 200
  - Для кого: Shop API, MES API.
  - Зачем: Показывает трафик приложения. (Трафик)
  - Ярлыки: endpoint.
- Number of HTTP 500
  - Для кого: Shop API, MES API.
  - Зачем: Показывает частоту ошибок. (Ошибки)
  - Ярлыки: endpoint, error_id.
- CPU %
  - Для кого: CRM API.
  - Зачем: Показывает нагрузку которое создает приложение для расчета модели. (Утилизация)
  - Ярлыки: container_id
- Memory Utilization
  - Для кого: CRM API.
  - Зачем: Показывает нагрузку которое создает приложение для расчета модели. (Утилизация)
  - Ярлыки: container_id
- Size queue (нету в базовом списке)
  - Для кого: CRM API.
  - Зачем: Показывает то сколько еще моделей надо обработать. (Насыщенность)
  - Ярлыки: container_id
- Number of connections
  - Для кого: Shop db, Mes db.
  - Зачем: Показывает то сколько открыто соединений, позволяет увидеть нагрузку на базы данных.
  - Ярлыки: db_id

## План действий

1. Согласовать метрики с аналитиками и бизнесом.
2. Выбрать инструменты для мониторинга.
3. Доработать существующий код приложений, чтобы можно было запросить метрики.
4. Развернуть инструмент агрегации метрик.
   1. Развернуть Time-Series БД для хранения метрик.
   2. Развернуть агенты-экспортёров, которые будут собирать метрики.
   3. Развернуть коллектор агентов, который будет агрегировать данные с агентов и записывать их в БД.
5. Развернуть инструмент для визуализации данных.
   1. Настроить графики и дашборды.
   2. Настроить сигнализацию об ошибках и предупреждениях.
6. Протестировать полученную систему.
7. Написать документацию и гайды, а также ознакомить команду с системой мониторинга.
