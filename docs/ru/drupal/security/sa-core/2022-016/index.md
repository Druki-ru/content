---
title: SA-CORE-2022-016
slug: wiki/security/SA-CORE-2022-016
metatags:
  title: 'Drupal: SA-CORE-2022-016'
  description: 'Умеренно критично. Исправлено в версиях: 9.4.3, 9.3.19.'
authors:
  - Niklan
---

**Проект:** Drupal Core\
**Дата публикации:** 28 сентября 2022\
**Риск безопасности:** Критично 18∕25. AC:Basic/A:Admin/CI:All/II:All/E:Proof/TD:All\
**Уязвимость:** Множественные уязвимости

## Описание

Drupal использует стороннюю библиотеки Twig для шаблонизации и санитизации. Для
Twig вышло обновление безопасности которое влияет и на Drupal. Разработчики Twig
оценивают данную уязвимость как серьезную.

Код Drupal, расширяющий Twig, был обновлён, чтобы нивелировать обнаруженные
уязвимости.

Доступны несколько вариантов для атаки на сайт если недоверенные пользователи
могут использовать Twig. Уязвимость может быть использована для доступа к
содержимому приватных файлов или прочих файлов на сервере, например настроек с
подключением к БД.

Опасность уязвимости в Drupal невелируется тем что использование данных
уязвимостей доступно только с определёнными административными правами доступа.
Но также остаётся риск использования данных уязвимостей через сторонние модули.

[Drupal Steward](https://www.drupal.org/steward) не покрывает данную проблему.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 9.4, обновитесь до Drupal [9.4.7](../../../releases/9/9.4.x/9.4.7/index.md).
- Если используете Drupal 9.3, обновитесь до Drupal [9.3.22](../../../releases/9/9.3.x/9.3.22/index.md).

## Ссылки

- [SA-CORE-2022-016](https://www.drupal.org/SA-CORE-2022-016) (англ.), drupal.org, 28 сентября 2022
- [Twig security release: Possibility to load a template outside a configured directory when using the filesystem loader](https://symfony.com/blog/twig-security-release-possibility-to-load-a-template-outside-a-configured-directory-when-using-the-filesystem-loader) (англ.), Symfony, 28 сентября 2022