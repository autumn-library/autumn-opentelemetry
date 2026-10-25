---
title: Семантические соглашения
---

# Семантические соглашения

Какие атрибуты [семантических соглашений OpenTelemetry](https://opentelemetry.io/docs/specs/semconv/)
заполняет `autumn-opentelemetry`. В таблицах — stable-атрибуты semconv (версия 1.44.0); устаревшие
атрибуты отмечены отдельно. Группы атрибутов названы так же, как в semconv.

Атрибуты, которые заполняет само ядро, описаны в [семантических соглашениях opentelemetry](/opentelemetry/semconv.md),
атрибуты работы с базой данных — в [семантических соглашениях opentelemetry-instrumentation-entity](/opentelemetry-instrumentation-entity/semconv.md).

## Ресурс

### Telemetry Distro

[Telemetry Distro](https://opentelemetry.io/docs/specs/semconv/registry/attributes/telemetry/)

| Атрибут | Значение |
|---|---|
| `telemetry.distro.name` | `autumn-opentelemetry` |
| `telemetry.distro.version` | версия пакета |

Атрибуты заполняет детектор [ОтелДетекторРесурсаДистрибутива](/api/autumn-opentelemetry/Классы/ОтелДетекторРесурсаДистрибутива.md).
Адрес схемы его ресурса — `https://opentelemetry.io/schemas/1.44.0`.

Остальные атрибуты ресурса — `service.*`, `telemetry.sdk.*`, атрибуты встроенных детекторов — заполняет
ядро, см. [Ресурс в семантических соглашениях opentelemetry](/opentelemetry/semconv.md#ресурс).

## Code

[Code](https://opentelemetry.io/docs/specs/semconv/registry/attributes/code/)

| Атрибут | Кто заполняет |
|---|---|
| `code.function.name` | спаны `&Наблюдаемый`, гистограммы `&Замеряемый`, счётчики `&Подсчитываемый`: имя метода желудя |

### Устаревшие атрибуты

| Атрибут | Кто заполняет |
|---|---|
| `code.namespace` | спаны `&Наблюдаемый`, гистограммы `&Замеряемый`, счётчики `&Подсчитываемый`: имя желудя. В semconv атрибут устарел: пространство имён входит в полное имя `code.function.name` |

## Исключения

[Exceptions](https://opentelemetry.io/docs/specs/semconv/exceptions/)

Спан `&Наблюдаемый`, из метода которого вышло исключение, получает статус `Error` и событие
исключения через [ОтелСпан.ЗаписатьИсключение](/api/opentelemetry/Трассировка/ОтелСпан.md).
Атрибуты `exception.*` события заполняет ядро, см. [Исключения в семантических соглашениях opentelemetry](/opentelemetry/semconv.md#исключения).

## Инструментирование entity

Спаны и метрики работы с базой данных через `autumn-data` пишет `opentelemetry-instrumentation-entity`.
Их атрибуты описаны в [семантических соглашениях opentelemetry-instrumentation-entity](/opentelemetry-instrumentation-entity/semconv.md).
