---
title: Семантические соглашения
---

# Семантические соглашения

Какие атрибуты [семантических соглашений OpenTelemetry](https://opentelemetry.io/docs/specs/semconv/)
заполняет `autumn-opentelemetry`. В таблицах — stable-атрибуты semconv (версия 1.44.0). Группы
атрибутов названы так же, как в semconv.

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
| `code.function.name` | спаны `&Наблюдаемый`, гистограммы `&Замеряемый`, счётчики `&Подсчитываемый`: полное имя метода `ИмяЖелудя.ИмяМетода` |

## Исключения

[Exceptions in logs](https://opentelemetry.io/docs/specs/semconv/exceptions/exceptions-logs/)

Исключение, вышедшее из метода `&Наблюдаемый`, записывается записью лога в контексте спана метода:

| Поле | Значение |
|---|---|
| Имя события | `exception`: метод желудя не относится к операции какой-либо предметной области семантических соглашений |
| Серьёзность | `ERROR` у спанов `server` и `consumer`, `WARN` у остальных |
| Атрибуты | `exception.*` из исключения, `code.function.name` |

Атрибуты `exception.*` заполняет ядро, см. [Исключения в семантических соглашениях opentelemetry](/opentelemetry/semconv.md#исключения).

## Error

[Error](https://opentelemetry.io/docs/specs/semconv/registry/attributes/error/)

| Атрибут | Кто заполняет |
|---|---|
| `error.type` | спаны `&Наблюдаемый`, гистограммы `&Замеряемый` и счётчики `&Подсчитываемый`, если из метода вышло исключение: [ОтелУтилиты.ТипОшибки](/api/opentelemetry/Ядро/ОтелУтилиты.md) — код ошибки библиотеки errors, код нативной ошибки OneScript или `_OTHER`. У успешных вызовов атрибута нет |

Описание статуса такого спана — текст исключения, по [recording-errors](https://opentelemetry.io/docs/specs/semconv/general/recording-errors/).

## Инструментирование entity

Спаны и метрики работы с базой данных через `autumn-data` пишет `opentelemetry-instrumentation-entity`.
Их атрибуты описаны в [семантических соглашениях opentelemetry-instrumentation-entity](/opentelemetry-instrumentation-entity/semconv.md).
