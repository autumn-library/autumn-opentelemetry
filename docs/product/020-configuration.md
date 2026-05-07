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

## Конфигурация логирования

`ОтелДуб` автоматически создаёт бин `ОтелАппендерLogos` и регистрирует его в logos. Для настройки уровня экспортируемых логов используйте `autumn-properties.json`:

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
