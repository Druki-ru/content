---
id: sa-contrib-2019-058
title: 'SA-CONTRIB-2019-058'
path: /security/sa-contrib-2019-058
metatags:
  title: 'Metatag: SA-CONTRIB-2019-058'
  description: 'Умеренно критическая уязвимость модуля Metatag. Устранена в версии 8.x-1.9'
---

**Проект:** [Metatag](https://www.drupal.org/project/metatag)\
**Дата публикации:** 24 июля 2019\
**Риск безопасности:** Умеренно критический. 13/25 AC:None A:None CI:Some II:None E:Theoretical TD:Uncommon\
**Уязвимость**: Раскрытие информации

## Описание

Модуль Metatag не производил в достаточной мере проверки на предмет того, что сайт находится в режиме обслуживания. Таким образом, метатеги могли отображаться на страницах, запрещенных к просмотру при активном режиме обслуживания.

Данная уязвимость смягчается тем фактом, что сайт должен быть настроен на запрет доступа к определенному контенту и должен быть переведен в режим обслуживания.

## Решение

Обновиться до [Metatag 8.x-1.9](https://www.drupal.org/project/metatag/releases/8.x-1.9).

## Принимали участие

**Сообщение об ошибке:**

- [Shloma](https://www.drupal.org/user/2858019)

**Исправление ошибки:**

- [Damien McKenna](https://www.drupal.org/user/108450) из Drupal Security Team

**Скоординировано:**

- [Damien McKenna](https://www.drupal.org/user/108450) из Drupal Security Team

## Ссылки

- [SA-CONTRIB-2019-058](https://www.drupal.org/sa-contrib-2019-058) (англ.), drupal.org, 24 июля 2019