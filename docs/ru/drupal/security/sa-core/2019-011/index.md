---
title: 'SA-CORE-2019-011'
slug: wiki/security/sa-core-2019-011
metatags:
  title: 'Drupal: SA-CORE-2019-011'
  description: 'Умеренно критический. Исправлено в версиях: 8.7.11 и 8.8.1.'
---

**Проект:** Drupal Core\
**Дата публикации:** 19 декабря 2019\
**Риск безопасности:** Умеренно критический. 10/25 AC:Basic A:User CI:Some II:None E:Theoretical TD:Default\
**Уязвимость**: Обход доступа

## Описание

Модуль Media Library имеет уязвимость в безопасности в некоторых случаях, когда недостаточно ограничивает права доступа.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 8.7.x, обновитесь до [Drupal 8.7.11](../../../releases/8/8.7.x/8.7.11/index.md).
- Если используете Drupal 8.8.x, обновитесь до [Drupal 8.8.1](../../../releases/8/8.8.x/8.8.1/index.md).

Альтернативно, вы можете защитить сайт сняв галочку "Включить расширенный UI" на `/admin/config/media/media-library` (_данное решение не работает на 8.7.x_). 

## Принимали участие

**Сообщение об ошибке:**

- [Adam G-H ](https://www.drupal.org/user/205645)

**Исправление ошибки:**

- [Adam G-H ](https://www.drupal.org/user/205645)
- [Jess](https://www.drupal.org/user/65776) из Drupal Security Team
- [Andrei Mateescu](https://www.drupal.org/user/729614)
- [Greg Knaddison](https://www.drupal.org/user/36762) из Drupal Security Team
- [Alex Bronstein](https://www.drupal.org/user/78040) из Drupal Security Team
- [Sean Blommaert](https://www.drupal.org/user/545912)
- [Lee Rowlands](https://www.drupal.org/user/395439) из Drupal Security Team

## Смотрите также

- [SA-CORE-2019-009](../2019-009/index.md)
- [SA-CORE-2019-010](../2019-010/index.md)
- [SA-CORE-2019-012](../2019-012/index.md)

## Ссылки

- [SA-CORE-2019-011](https://www.drupal.org/SA-CORE-2019-011) (англ.), drupal.org, 19 декабря 2019