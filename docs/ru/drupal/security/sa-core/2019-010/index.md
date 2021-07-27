---
title: 'SA-CORE-2019-010'
slug: security/sa-core-2019-010
metatags:
  title: 'Drupal: SA-CORE-2019-010'
  description: 'Умеренно критический. Исправлено в версиях: 8.7.11 и 8.8.1.'
---

**Проект:** Drupal Core\
**Дата публикации:** 19 декабря 2019\
**Риск безопасности:** Умеренно критический. 14/25 AC:Basic A:Admin CI:Some II:All E:Theoretical TD:Default\
**Уязвимость**: Множественные уязвимости

## Описание

Функция `file_save_upload()` из ядра Drupal 8 не удаляла начальную точку (`.`) из названий имён, как это делал Drupal 7.

Пользователи, у которых есть права на загрузку файлов с любым расширением файла, в связке со сторонними модулями, могут иметь возможность загружать в файловую систему файлы на подобии `.htaccess`, что может позволит перекрыть доступы к файлам в соответствующих местах.

Теперь `file_save_upload()` удаляет начальную точку.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 8.7.x, обновитесь до [Drupal 8.7.11](../../../releases/8/8.7.x/8.7.11/index.md).
- Если используете Drupal 8.8.x, обновитесь до [Drupal 8.8.1](../../../releases/8/8.8.x/8.8.1/index.md).

## Принимали участие

**Сообщение об ошибке:**

- [Rohit Kapur](https://www.drupal.org/user/3623849)
- [Filipe Reis](https://www.drupal.org/user/3521501)
- [Dan Reif](https://www.drupal.org/user/454444)
- [mramydnei](https://www.drupal.org/user/3529990)

**Исправление ошибки:**

- [Lee Rowlands](https://www.drupal.org/user/395439) из Drupal Security Team
- [Greg Knaddison](https://www.drupal.org/user/36762) из Drupal Security Team
- [Michael Hess](https://www.drupal.org/user/102818) из Drupal Security Team
- [Kim Pepper](https://www.drupal.org/user/370574)
- [Alex Pott](https://www.drupal.org/user/157725) из Drupal Security Team
- [Derek Wright](https://www.drupal.org/user/46549)
- [Jess](https://www.drupal.org/user/65776) из Drupal Security Team
- [David Rothstein](https://www.drupal.org/user/124982) из Drupal Security Team

## Смотрите также

- [SA-CORE-2019-009](../2019-009/index.md)
- [SA-CORE-2019-011](../2019-011/index.md)
- [SA-CORE-2019-012](../2019-012/index.md)

## Ссылки

- [SA-CORE-2019-010](https://www.drupal.org/SA-CORE-2019-010) (англ.), drupal.org, 19 декабря 2019