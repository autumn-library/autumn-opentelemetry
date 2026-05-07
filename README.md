# autumn-opentelemetry

[![Quality Gate Status](https://sonar.openbsl.ru/api/project_badges/measure?project=autumn-opentelemetry&metric=alert_status)](https://sonar.openbsl.ru/dashboard?id=autumn-opentelemetry)
[![Coverage](https://sonar.openbsl.ru/api/project_badges/measure?project=autumn-opentelemetry&metric=coverage)](https://sonar.openbsl.ru/dashboard?id=autumn-opentelemetry)
[![Bugs](https://sonar.openbsl.ru/api/project_badges/measure?project=autumn-opentelemetry&metric=bugs)](https://sonar.openbsl.ru/dashboard?id=autumn-opentelemetry)
[![Code Smells](https://sonar.openbsl.ru/api/project_badges/measure?project=autumn-opentelemetry&metric=code_smells)](https://sonar.openbsl.ru/dashboard?id=autumn-opentelemetry)
[![Telegram](https://img.shields.io/badge/Telegram-чат-blue?logo=telegram)](https://t.me/autumn_winow)

Интеграция [OpenTelemetry SDK](https://github.com/nixel2007/opentelemetry) с [Autumn Framework](https://github.com/autumn-library/autumn).

Предоставляет аннотации для автоматической инструментации методов трассировкой и метриками.

## Возможности

- `&Наблюдаемый` — автоматическое создание span'ов вокруг методов
- `&Замеряемый` — запись длительности методов в гистограмму (аналог `@Timed` в Spring Boot)
- `&Подсчитываемый` — счётчик вызовов методов (аналог `@Counted` в Spring Boot)
- `&АтрибутСпана` — добавление параметров метода как атрибутов span'а
- Автоматическая настройка `ОтелАппендерLogos` для экспорта логов через OTel

## Совместимость

- OneScript 2.0.0+
- [opentelemetry](https://github.com/nixel2007/opentelemetry) >= 1.0.0
- [autumn](https://github.com/autumn-library/autumn) >= 4.3.12

## Установка

```sh
opm install autumn-opentelemetry
```

## Быстрый старт

`autumn-opentelemetry` автоматически подключается к приложению Autumn через `ОтелДуб` — достаточно добавить `#Использовать autumn-opentelemetry` в конфигурацию приложения:

```bsl
#Использовать autumn
#Использовать autumn-opentelemetry

Поделка = Новый Поделка();
Поделка.ЗапуститьПриложение();
```

`ОтелДуб` автоматически:
1. Инициализирует OpenTelemetry SDK через [переменные окружения](https://github.com/nixel2007/opentelemetry?tab=readme-ov-file#автоконфигурация)
2. Регистрирует бины `ОтелSdk`, `ОтелТрассировщик`, `ОтелМетр`
3. Создаёт и регистрирует аппендер `ОтелАппендерLogos` для экспорта логов

## Аннотации

### &Наблюдаемый — трассировка методов

Автоматически создаёт span OpenTelemetry вокруг вызовов метода.

```bsl
&Желудь
Процедура ПриСозданииОбъекта()
КонецПроцедуры

// Инструментировать один метод
&Наблюдаемый("orders.process")
Функция ОбработатьЗаказ(&АтрибутСпана("order.id") ИдЗаказа) Экспорт
    // span с атрибутом order.id = значение ИдЗаказа
    Возврат СформироватьОтвет(ИдЗаказа);
КонецФункции
```

Размещение `&Наблюдаемый` на конструкторе инструментирует все экспортные методы:

```bsl
&Желудь
&Наблюдаемый
Процедура ПриСозданииОбъекта()
КонецПроцедуры
```

**Параметры аннотации:**

| Параметр | По умолчанию | Описание |
|----------|--------------|----------|
| `Значение` | `"ИмяБина.ИмяМетода"` | Имя span'а |
| `ВидСпана` | `internal` | Вид span'а: `internal`, `server`, `client`, `producer`, `consumer` |

### &АтрибутСпана — атрибуты параметров

Добавляет значение параметра метода как атрибут текущего span'а:

```bsl
&Наблюдаемый
Функция ОбработатьЗаказ(&АтрибутСпана("order.id") ИдЗаказа, Данные) Экспорт
    // span автоматически получит атрибут order.id = ИдЗаказа
КонецФункции
```

### &Замеряемый — измерение длительности

Записывает длительность (в мс) каждого вызова метода в гистограмму OTel:

```bsl
&Желудь
Процедура ПриСозданииОбъекта()
КонецПроцедуры

&Замеряемый("payments.process.duration")
Функция ОбработатьПлатёж(Данные) Экспорт
    // длительность записывается с атрибутами code.function.name, code.namespace
    Возврат ПровестиПлатёж(Данные);
КонецФункции
```

### &Подсчитываемый — счётчик вызовов

Инкрементирует счётчик OTel при каждом вызове метода:

```bsl
&Желудь
Процедура ПриСозданииОбъекта()
КонецПроцедуры

&Подсчитываемый("api.requests")
Функция ОбработатьЗапрос(Запрос) Экспорт
    // счётчик с атрибутами result=success|failure, exception
    Возврат СформироватьОтвет(Запрос);
КонецФункции
```

## Конфигурация логирования

`ОтелДуб` автоматически создаёт бин `ОтелАппендерLogos` и регистрирует его в logos. Для настройки уровня экспортируемых логов используйте `autumn-properties.json`:

```json
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

## Конфигурация OpenTelemetry

SDK активируется автоматически при подключении `autumn-opentelemetry`. Единственный обязательный параметр — имя сервиса.

Минимальная конфигурация через `autumn-properties.json`:

```json
{
  "otel": {
    "service": {
      "name": "my-service"
    }
  }
}
```

Или через переменную окружения:

```sh
OTEL_SERVICE_NAME=my-service
```

Все остальные параметры опциональны — по умолчанию SDK экспортирует трассы, метрики и логи по `http://localhost:4318` (протокол `http/protobuf`). Полный список параметров в `autumn-properties.json`:

```json
{
  "otel": {
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

Чтобы отключить SDK: `"otel": { "sdk": { "disabled": true } }`.

Подробнее — в документации [opentelemetry](https://github.com/nixel2007/opentelemetry).
