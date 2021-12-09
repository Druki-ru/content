---
title: 'SA-CORE-2019-008'
slug: wiki/security/sa-core-2019-008
metatags:
  title: 'Drupal: SA-CORE-2019-008'
  description: 'Критический. Исправлено в версии: 8.7.5.'
---

**Проект:** Drupal core\
**Дата публикации:** 17 июля 2019\
**Риск безопасности:** Критический. 17/25 AC:None A:None CI:Some II:Some E:Theoretical TD:Default\
**Уязвимость**: Обход доступа

## Описание

При использовании экспериментального модуля Workspaces на Drupal 8.7.4, появляется условие для потенциального обхода доступа.

Проблема может быть устранена путем отключения модуля Workspaces.

Drupal 8.7.3 и более ранние версии не подвержены проблеме.

## Решение

Обновиться до [Drupal 8.7.5](../../../releases/8/8.7.x/8.7.5/index.md).

## Принимали участие

**Сообщение об ошибке:**

- [Dave Botsch](https://www.drupal.org/user/3534164)

**Исправление ошибки:**

- [Jess](https://www.drupal.org/user/65776) из Drupal Security Team
- [Michael Hess](https://www.drupal.org/user/102818) из Drupal Security Team
- [Greg Knaddison](https://www.drupal.org/user/36762) из Drupal Security Team
- [Chris McCafferty](https://www.drupal.org/user/1850070) из Drupal Security Team
- [Neil Drumm](https://www.drupal.org/user/3064) из Drupal Security Team
- [Alex Pott](https://www.drupal.org/user/157725) из Drupal Security Team
- [Andrei Mateescu](https://www.drupal.org/user/729614)

## Ссылки

- [SA-CORE-2019-008](https://www.drupal.org/SA-CORE-2019-008) (англ.), drupal.org, 17 июля 2019