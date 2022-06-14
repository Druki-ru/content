---
title: SA-CORE-2022-011
slug: wiki/security/sa-core-2022-011
metatags:
  title: 'Drupal: SA-CORE-2022-011'
  description: 'Умеренно критично. Исправлено в версиях: 9.3.16, 9.2.21.'
authors:
  - Niklan
---

**Проект:** Drupal Core\
**Дата публикации:** 10 июня 2022\
**Риск безопасности:** Умеренно критично 13∕25. AC:Basic/A:User/CI:Some/II:Some/E:Theoretical/TD:Default\
**Уязвимость:** Сторонние библиотеки

## Описание

Drupal использует стороннюю библиотеку Guzzle для управления запросами и ответами к сторонним сервисам. Guzzle выпустили два обновления безопасности:

- [Failure to strip the Cookie header on change in host or HTTP downgrade](https://github.com/guzzle/guzzle/security/advisories/GHSA-f2wf-25xc-69c9)
- [Fix failure to strip Authorization header on HTTP downgrade](https://github.com/guzzle/guzzle/security/advisories/GHSA-w248-ffj2-4v5q)

Они не влияют на Drupal ядро, но могут оказать воздействие на некоторые сторонние модули или пользовательский код.

Данный релиз безопасности выпущен за пределами обычного расписания, поскольку Guzzle уже опубликовал информацию об уязвимости, и они могут существовать на проектах которые используют библиотеку для внешних запросов. Guzzle оценил данную уязвимость как «Высокий риск».

[Drupal Steward](https://www.drupal.org/steward) не покрывает данную проблему.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 9.3, обновитесь до Drupal [9.3.16](../../../releases/9/9.3.x/9.3.16/index.md).
- Если используете Drupal 9.2, обновитесь до Drupal [9.2.21](../../../releases/9/9.2.x/9.2.21/index.md).

## Ссылки

- [SA-CORE-2022-011](https://www.drupal.org/SA-CORE-2022-011) (англ.), drupal.org, 25 мая 2022
- [Cross-domain cookie leakage](https://github.com/guzzle/guzzle/security/advisories/GHSA-cwmx-hcrq-mhc3) (англ.), github.com, 25 мая 2022