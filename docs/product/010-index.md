---
title: Описание
---

# autumn-opentelemetry

[![Quality Gate Status](https://sonar.openbsl.ru/api/project_badges/measure?project=autumn-opentelemetry&metric=alert_status)](https://sonar.openbsl.ru/dashboard?id=autumn-opentelemetry)
[![Coverage](https://sonar.openbsl.ru/api/project_badges/measure?project=autumn-opentelemetry&metric=coverage)](https://sonar.openbsl.ru/dashboard?id=autumn-opentelemetry)
[![Telegram](https://img.shields.io/badge/Telegram-чат-blue?logo=telegram)](https://t.me/autumn_winow)

`autumn-opentelemetry` — интеграция [OpenTelemetry SDK](https://github.com/nixel2007/opentelemetry) с [Autumn Framework](https://github.com/autumn-library/autumn).

Библиотека предоставляет аннотации для автоматической инструментации методов трассировкой, метриками и счётчиками, а также мост между логгером [logos](https://github.com/oscript-library/logos) и OTel Logs API.

## Установка

```sh
opm install autumn-opentelemetry
```

## Совместимость

- OneScript 2.0.0+
- [opentelemetry](https://github.com/nixel2007/opentelemetry) >= 1.0.0
- [autumn](https://github.com/autumn-library/autumn) >= 4.3.12

## Быстрый старт

### 1. Подключение к приложению

`autumn-opentelemetry` подключается к приложению Autumn автоматически — достаточно добавить `#Использовать autumn-opentelemetry`:

:::code-group

```bsl [main.os]
#Использовать autumn
#Использовать autumn-opentelemetry

Поделка = Новый Поделка();
Поделка.ЗапуститьПриложение();
```

:::

### 2. Минимальная конфигурация

`otel.enabled` и `otel.service.name` — обязательные параметры:

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

:::

### 3. Инструментация методов

:::code-group

```bsl [Классы/СервисЗаказов.os]
&Желудь
&Наблюдаемый
Процедура ПриСозданииОбъекта()
КонецПроцедуры

&Подсчитываемый("orders.processed")
&Замеряемый("orders.duration")
Функция ОбработатьЗаказ(&АтрибутСпана("order.id") ИдЗаказа) Экспорт
    // span с атрибутом order.id = ИдЗаказа
    // длительность в гистограмме orders.duration
    // счётчик orders.processed инкрементируется
    Возврат СформироватьОтвет(ИдЗаказа);
КонецФункции
```

:::

## Основные возможности

`ОтелДуб` автоматически:
1. Инициализирует OpenTelemetry SDK через параметры приложения
2. Регистрирует бины `ОтелSdk`, `ОтелТрассировщик`, `ОтелМетр`
3. Создаёт и регистрирует аппендер `ОтелАппендерLogos` для экспорта логов

Аннотации доступны для любого `&Желудь`:

| Аннотация | Описание |
|-----------|----------|
| `&Наблюдаемый` | Создаёт span OpenTelemetry вокруг метода |
| `&Замеряемый` | Записывает длительность вызова в гистограмму |
| `&Подсчитываемый` | Инкрементирует счётчик при каждом вызове |
| `&АтрибутСпана` | Добавляет параметр метода как атрибут span'а |

## Дальнейшее изучение

### Руководство пользователя
- [Конфигурация](/autumn-opentelemetry/configuration.md) — параметры OpenTelemetry и логирования
- [Аннотации](/autumn-opentelemetry/annotations.md) — подробное описание аннотаций с примерами

### Справочник API
- [Публичный интерфейс](/api/autumn-opentelemetry/index.md)
