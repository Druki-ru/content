---
id: sa-core-2019-012
title: 'SA-CORE-2019-012'
path: /security/sa-core-2019-012
metatags:
  title: 'Drupal: SA-CORE-2019-012'
  description: 'Умеренно критический. Исправлено в версиях: 8.7.11 и 8.8.1.'
---

**Проект:** Drupal Core\
**Дата публикации:** 19 декабря 2019\
**Риск безопасности:** Критический. 17/25 AC:Basic A:User CI:All II:All E:Proof TD:Uncommon\
**Уязвимость**: Множественные уязвимости

## Описание

Drupal использует стороннюю библиотеку [Archive_Tar](https://pear.php.net/package/Archive_Tar/), для которой вышло обновление безопасности, которое может затронуть некоторые конфигурации Drupal.

Возможны множественные уязвимости если Drupal разрешено загружать и обрабатывать файлы `.tar`, `.tar.gz`, `.bz2` или `.tlz`.

Drupal увеличивает версию зависимости до 1.4.9 где эта проблема решена.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 8.7.x, обновитесь до [Drupal 8.7.11](../../../8/releases/8.7.x/8.7.11/index.md).
- Если используете Drupal 8.8.x, обновитесь до [Drupal 8.8.1](../../../8/releases/8.8.x/8.8.1/index.md).

Альтернативно, вы можете защитить сайт сняв галочку "Включить расширенный UI" на `/admin/config/media/media-library` (_данное решение не работает на 8.7.x_). 

## Принимали участие

**Сообщение об ошибке:**

- [Jasper Mattsson](https://www.drupal.org/user/521118)

**Исправление ошибки:**

- [Lee Rowlands](https://www.drupal.org/user/395439) из Drupal Security Team
- [Peter Wolanin](https://www.drupal.org/user/49851) из Drupal Security Team
- [Sam Becker](https://www.drupal.org/user/1485048)
- [Jasper Mattsson](https://www.drupal.org/user/521118)
- [David Rothstein](https://www.drupal.org/user/124982) из Drupal Security Team
- [michieltcs](https://www.drupal.org/user/3587972)
- [Ayesh Karunaratne](https://www.drupal.org/user/796148)
- [Alex Pott](https://www.drupal.org/user/157725) из Drupal Security Team
- [Jess](https://www.drupal.org/user/65776) из Drupal Security Team
- [Samuel Mortenson](https://www.drupal.org/user/2582268)
- [Vijaya Chandran Mani](https://www.drupal.org/user/93488)
- [Drew Webber](https://www.drupal.org/user/255969)

## Смотрите также

- [SA-CORE-2019-009](../2019-009/index.md)
- [SA-CORE-2019-010](../2019-010/index.md)
- [SA-CORE-2019-011](../2019-011/index.md)

## Ссылки

- [SA-CORE-2019-012](https://www.drupal.org/SA-CORE-2019-012) (англ.), drupal.org, 19 декабря 2019