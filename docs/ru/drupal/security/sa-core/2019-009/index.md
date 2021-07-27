---
title: 'SA-CORE-2019-009'
slug: security/sa-core-2019-009
metatags:
  title: 'Drupal: SA-CORE-2019-009'
  description: 'Умеренно критический. Исправлено в версиях: 8.7.11 и 8.8.1.'
---

**Проект:** Drupal Core\
**Дата публикации:** 19 декабря 2019\
**Риск безопасности:** Умеренно критический. 12/25 AC:None A:None CI:None II:None E:Theoretical TD:All\
**Уязвимость**: Отказ в обслуживании

## Описание

Посещение `install.php` может повредить кеш, что может привести к недоступности сайта пока кеш не будет пересоздан.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 8.7.x, обновитесь до [Drupal 8.7.11](../../../releases/8/8.7.x/8.7.11/index.md).
- Если используете Drupal 8.8.x, обновитесь до [Drupal 8.8.1](../../../releases/8/8.8.x/8.8.1/index.md).

Для исправления данной ошибки на прочих версиях, заблокируйте доступ к `install.php` файлу, если он вам не нужен.

## Принимали участие

**Сообщение об ошибке:**

- [Drew Webber](https://www.drupal.org/user/255969) из Drupal Security Team

**Исправление ошибки:**

- [Drew Webber](https://www.drupal.org/user/255969) из Drupal Security Team
- [Lee Rowlands](https://www.drupal.org/user/395439) из Drupal Security Team
- [Heine](https://www.drupal.org/user/17943) из Drupal Security Team
- [Alex Pott](https://www.drupal.org/user/157725) из Drupal Security Team
- [Jess](https://www.drupal.org/user/65776) из Drupal Security Team
- [Damien McKenna](https://www.drupal.org/user/108450) из Drupal Security Team
- [David Snopek](https://www.drupal.org/user/266527) из Drupal Security Team
- [Nathaniel Catchpole](https://www.drupal.org/user/35733) из Drupal Security Team
- [Greg Knaddison](https://www.drupal.org/user/36762) из Drupal Security Team

## Смотрите также

- [SA-CORE-2019-010](../2019-010/index.md)
- [SA-CORE-2019-011](../2019-011/index.md)
- [SA-CORE-2019-012](../2019-012/index.md)

## Ссылки

- [SA-CORE-2019-009](https://www.drupal.org/SA-CORE-2019-009) (англ.), drupal.org, 19 декабря 2019