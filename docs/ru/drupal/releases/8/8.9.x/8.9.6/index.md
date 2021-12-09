---
title: 'Drupal 8.9.6'
slug: wiki/drupal/releases/8.9.6
core: 8
metatags:
  title: 'Drupal 8.9.6: Список изменений'
  description: 'Обновления безопасности: SA-CORE-2020-007, SA-CORE-2020-008, SA-CORE-2020-009, SA-CORE-2020-010, SA-CORE-2020-011.'
---

**Дата релиза**: 16 сентября 2020

Этот выпуск исправляет уязвимости безопасности. Настоятельно рекомендуется обновить сайты сразу после прочтения примечаний ниже и объявления о безопасности.

Исправлены следующие уязвимости:

- Drupal Core — Умеренно критическое — Межсайтовый скриптинг — [SA-CORE-2020-007](../../../../security/sa-core/2020-007/index.md)
- Drupal Core — Умеренно критическое — Обход доступа — [SA-CORE-2020-008](../../../../security/sa-core/2020-008/index.md)
- Drupal Core — Критическое — Межсайтовый скриптинг — [SA-CORE-2020-009](../../../../security/sa-core/2020-009/index.md)
- Drupal Core — Умеренно критическое — Межсайтовый скриптинг — [SA-CORE-2020-010](../../../../security/sa-core/2020-010/index.md)
- Drupal Core — Умеренно критическое — Раскрытие информации — [SA-CORE-2020-011](../../../../security/sa-core/2020-011/index.md)

## Важная информация об обновлении

- [SA-CORE-2020-007](../../../../security/sa-core/2020-007/index.md) Если вы отправляете JSONP запросы при помощи Drupal AJAX API, вам необходимо добавить новую опцию. Если вы используете jQuery AJAX для отправки запросов и вам не нужно отправлять JSONP, отключите данную опцию.
- [SA-CORE-2020-008](../../../../security/sa-core/2020-008/index.md) Если вы используете модуль Workspaces, вам необходимо сбросить сессии для пользователям с доступом к рабочим областям.
- [SA-CORE-2020-009](../../../../security/sa-core/2020-009/index.md) Если вы переопределяете `\Drupal\Core\Form\FormBuilder::renderPlaceholderFormAction` и/или `\Drupal\Core\Form\FormBuilder::buildFormAction`, вам необходимо убедиться что вы сантизируете URL.

## Ссылки

- [Drupal 8.9.6](https://www.drupal.org/project/drupal/releases/8.9.6) (англ.), drupal.org, 16 сентября 2020
