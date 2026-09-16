---
title: Конфигурация
---

# Конфигурация

## Обязательные параметры

Два параметра обязательны для запуска SDK:

| Параметр | Описание |
|----------|----------|
| `otel.enabled` | Включает инструментирование аннотациями. При `false` SDK не инициализируется и экспортёры не запускаются. |
| `otel.service.name` | Имя сервиса в телеметрии. |

:::code-group

```json [autumn-properties.json]
{
  "otel": {
    "enabled": true,
    "service": {
      "name": "my-service"
    }
  }
}
```

```sh [Переменные окружения]
OTEL_ENABLED=true
OTEL_SERVICE_NAME=my-service
```

:::

## Экспорт телеметрии

По умолчанию SDK экспортирует трассы, метрики и логи по адресу `http://localhost:4318` (протокол `http/protobuf`).

:::code-group

```json [autumn-properties.json]
{
  "otel": {
    "enabled": true,
    "service": {
      "name": "my-service"
    },
    "exporter": {
      "otlp": {
        "endpoint": "http://localhost:4318",
        "protocol": "http/protobuf"
      }
    },
    "traces":  { "exporter": "otlp" },
    "metrics": { "exporter": "otlp" },
    "logs":    { "exporter": "otlp" }
  }
}
```

:::

Чтобы отключить SDK полностью:

:::code-group

```json [autumn-properties.json]
{
  "otel": {
    "sdk": {
      "disabled": true
    }
  }
}
```

:::

Полный список параметров OpenTelemetry — в документации [opentelemetry SDK](https://github.com/nixel2007/opentelemetry).

## Инструментирование entity

Параметры `otel.entity.enabled`, `otel.entity.query-text` и `otel.entity.repository.enabled` управляют трассировкой и метриками работы с базой данных через `autumn-data`. Признаки включения по умолчанию наследуют `otel.enabled`, текст запроса по умолчанию не пишется. Подробнее - в разделе [Инструментирование entity](/autumn-opentelemetry/entity.md).

## Конфигурация логирования

`ОтелДуб` автоматически создаёт желудь `ОтелАппендерLogos` и регистрирует его в logos. Для настройки уровня экспортируемых логов используйте `autumn-properties.json`:

:::code-group

```json [autumn-properties.json]
{
  "logos": {
    "logger": {
      "rootLogger": {
        "level": "INFO",
        "appenders": ["otel", "console"]
      }
    },
    "appender": {
      "otel": {
        "type": "ОтелАппендерLogos",
        "level": "WARN"
      },
      "console": {
        "type": "ВыводЛогаВКонсоль",
        "level": "INFO"
      }
    }
  }
}
```

:::
