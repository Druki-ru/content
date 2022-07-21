---
title: SA-CORE-2022-015
slug: wiki/security/sa-core-2022-015
metatags:
  title: 'Drupal: SA-CORE-2022-015'
  description: 'Умеренно критично. Исправлено в версиях: 9.4.3, 9.3.19.'
authors:
  - Niklan
---

**Проект:** Drupal Core\
**Дата публикации:** 20 июля 2022\
**Риск безопасности:** Умеренно критично 11∕25. AC:Complex/A:User/CI:Some/II:Some/E:Theoretical/TD:Uncommon\
**Уязвимость:** Множественные уязвимости

## Описание

Маршрут для oMebed iframe, предоставляемый модулем Media, некорректно проверял
настройки домена для iframe, что могло позволить выводить iframe в контексте
основного домена. В особых обстоятельствах это может привести к XSS, краже 
Cookie или прочим подобным уязвимостям.

[Drupal Steward](https://www.drupal.org/steward) не покрывает данную проблему.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 9.4, обновитесь до Drupal [9.4.3](../../../releases/9/9.4.x/9.4.3/index.md).
- Если используете Drupal 9.3, обновитесь до Drupal [9.3.19](../../../releases/9/9.3.x/9.3.19/index.md).

## Ссылки

- [SA-CORE-2022-015](https://www.drupal.org/SA-CORE-2022-015) (англ.), drupal.org, 20 июля 2022