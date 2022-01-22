---
title: 'SA-CONTRIB-2019-060'
slug: wiki/security/sa-contrib-2019-060
metatags:
  title: 'Existing Values Autocomplete Widget: SA-CONTRIB-2019-060'
  description: 'Критическая уязвимость модуля Existing Values Autocomplete Widget. Устранена в версии 8.x-1.2'
authors:
  - Niklan
---

**Проект:** [Existing Values Autocomplete Widget](https://www.drupal.org/project/existing_values_autocomplete_widget)\
**Дата публикации:** 24 июля 2019\
**Риск безопасности:** Критический. 17/25 AC:None A:None CI:All II:None E:Theoretical TD:All\
**Уязвимость**: Обход доступа

## Описание

Модуль не производил достаточно проверок на право доступа, прежде чем отдавать результаты автозаполнения.

Уязвимость смягчается тем, что атакующему необходимо знать маршрут до контроллера, чтобы произвести атаку.

## Решение

Обновиться до [Existing Values Autocomplete Widget 8.x-1.2](https://www.drupal.org/node/3069768).

## Принимали участие

**Сообщение об ошибке:**

- [David Stinemetze](https://www.drupal.org/user/2508346)

**Исправление ошибки:**

- [Art Williams](https://www.drupal.org/user/77599)

**Скоординировано:**

- [Michael Hess](https://www.drupal.org/user/102818) из Drupal Security Team
- [Greg Knaddison](https://www.drupal.org/user/36762) из Drupal Security Team

## Ссылки

- [SA-CONTRIB-2019-060](https://www.drupal.org/sa-contrib-2019-060) (англ.), drupal.org, 24 июля 2019