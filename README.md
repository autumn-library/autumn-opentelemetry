# autumn-opentelemetry

[![Quality Gate Status](https://sonar.openbsl.ru/api/project_badges/measure?project=autumn-opentelemetry&metric=alert_status)](https://sonar.openbsl.ru/dashboard?id=autumn-opentelemetry)
[![Coverage](https://sonar.openbsl.ru/api/project_badges/measure?project=autumn-opentelemetry&metric=coverage)](https://sonar.openbsl.ru/dashboard?id=autumn-opentelemetry)
[![Telegram](https://img.shields.io/badge/Telegram-чат-blue?logo=telegram)](https://t.me/autumn_winow)

Интеграция [OpenTelemetry SDK](https://github.com/nixel2007/opentelemetry) с [Autumn Framework](https://github.com/autumn-library/autumn). Аннотации для автоматической инструментации методов трассировкой и метриками.

## Установка

```sh
opm install autumn-opentelemetry
```

## Быстрый старт

```bsl
#Использовать autumn
#Использовать autumn-opentelemetry

&Желудь
&Наблюдаемый
Процедура ПриСозданииОбъекта()
КонецПроцедуры

&Подсчитываемый("orders.count")
&Замеряемый("orders.duration")
Функция ОбработатьЗаказ(&АтрибутСпана("order.id") ИдЗаказа) Экспорт
    Возврат СформироватьОтвет(ИдЗаказа);
КонецФункции
```

## Документация

- [Руководство пользователя](docs/product/010-index.md)
- [Справочник API](docs/api/index.md)

## Основные возможности

- 🔭 **`&Наблюдаемый`** — автоматическое создание span'ов вокруг методов
- ⏱️ **`&Замеряемый`** — запись длительности методов в гистограмму
- 🔢 **`&Подсчитываемый`** — счётчик вызовов методов
- 🏷️ **`&АтрибутСпана`** — параметры метода как атрибуты span'а
- 📋 **Logs bridge** — автоматический экспорт логов logos через OTel Logs API

## Лицензия

MIT License. Подробности в файле [LICENSE.md](LICENSE.md).
