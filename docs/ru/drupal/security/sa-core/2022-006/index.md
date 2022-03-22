---
title: SA-CORE-2022-006
slug: wiki/security/sa-core-2022-006
metatags:
  title: 'Drupal: SA-CORE-2022-006'
  description: 'Умеренно критично. Исправлено в версиях: 9.3.9, 9.2.16.'
authors:
  - Niklan
---

**Проект:** Drupal Core\
**Дата публикации:** 21 марта 2022\
**Риск безопасности:** Умеренно критично 11∕25. AC:Complex/A:None/CI:None/II:Some/E:Theoretical/TD:Default\
**Уязвимость:** Сторонние библиотеки

## Описание

Drupal использует стороннюю библиотеку Guzzle для работы с HTTP запросами и ответами к сторонним ресурсам. Для [Guzzle вышло обновление безопасности](https://github.com/guzzle/psr7/security/advisories/GHSA-q7rv-6hp3-vh96), которое может иметь воздействие на Drupal сайты.

Данное обновление для Drupal выходит не как обычно в третью среду месяца, так как информация об уязвимости уже опубликована Guzzle и она может присутствовать в кодовой базе ядра Drupal, сторонних или собственных модулях использующих данную библиотеку. Guzzle оценили проблему как «минимальный риск».

[Drupal Steward](https://www.drupal.org/steward) не покрывает данную проблему.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 9.3, обновитесь до Drupal [9.3.9](../../../releases/9/9.3.x/9.3.9/index.md).
- Если используете Drupal 9.2, обновитесь до Drupal [9.2.16](../../../releases/9/9.2.x/9.2.16/index.md).

## Ссылки

- [SA-CORE-2022-006](https://www.drupal.org/SA-CORE-2022-006) (англ.), drupal.org, 21 марта 2022
- [Improper Input Validation in guzzlehttp/psr7 ](https://github.com/guzzle/psr7/security/advisories/GHSA-q7rv-6hp3-vh96) (англ.), github.com, 21 марта 2022