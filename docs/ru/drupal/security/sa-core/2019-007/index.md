---
title: 'SA-CORE-2019-007'
slug: wiki/security/sa-core-2019-007
metatags:
  title: 'Drupal: SA-CORE-2019-007'
  description: 'Умеренно критический. Исправлено в версиях: 8.7.1, 8.6.16 и 7.67.'
authors:
  - Niklan
---

**Проект:** Drupal core\
**Дата публикации:** 8 мая 2019\
**Риск безопасности:** Умеренно критический. 14/25 AC:Complex, A:Admin, CI:All, II:All, E:Theoretical, TD:Uncommon\
**Уязвимость**: Сторонние библиотеки

## Описание

Данный релиз безопасности исправляет сторонние зависимости, которые поставляются с Drupal или являются зависимостью ядра Drupal.

### Описание проблемы от TYPO3

Для того чтобы перехватить выполнение файлов такими функциями, как `file_exists()` или `stat()`, на скомпрометированные Phar архивы, базовое имя должно быть определено и проверено, прежде чем разрешать передачу файла на выполнение в PHP Phar stream.

В примере ниже базовое имя `/path/bad.phar`, а `/phar-content.txt` - файл, поставляемый с данным Phar архивом.

```php
$userSubmittedPath = 'phar:///path/bad.phar/phar-content.txt';
file_exists($userSubmittedPath);
```

Текущая реализация уязвима к атаке path traversal (когда можно получить доступ к файлам уровнем ниже чем корневой `../`), что приводит к сценариям, когда проверяемый Phar архив, не является тем, что запросили на самом деле.

```php
// Helper::determineBaseFile(
//    'phar:///path/bad.phar/../good.phar'
// )
// некорректно определит базовое имя как '/path/good.phar'
// ... что можно использовать для разрешения вызова.
if ($interceptor->assert('/path/good.phar')) {
    $wrapper->invokeInternalStreamWrapper(
      'stat',
      'phar:///path/bad.phar/../good.phar'
    );
}
```

## Решение

Обновиться до актуальных версий:

- Если вы используете Drupal 8.7, обновитесь до [Drupal 8.7.1](../../../releases/8/8.7.x/8.7.1/index.md)
- Если вы используете Drupal 8.6 или ниже, обновитесь до Drupal 8.6.16.
- Если вы используете Drupal 7, обновитесь до Drupal 7.67.

## Принимали участие

**Сообщение об ошибке:**

- [Daniel Le Gall](https://www.drupal.org/user/3606561)

**Исправление ошибки:**

- [Jess](https://www.drupal.org/user/65776) из Drupal Security Team
- [Michael Hess](https://www.drupal.org/user/102818) из Drupal Security Team
- [Oliver Hader](https://www.drupal.org/user/3602633)
- [David Snopek](https://www.drupal.org/user/266527) из Drupal Security Team
- [Alex Pott](https://www.drupal.org/user/157725) из Drupal Security Team
- [Daniel Le Gall](https://www.drupal.org/user/3606561)
- [Tim Plunkett](https://www.drupal.org/user/241634)

## Ссылки

- [SA-CORE-2019-007](https://www.drupal.org/SA-CORE-2019-007) (англ.), drupal.org, 8 мая 2019
- [TYPO3-PSA-2019-007](https://typo3.org/security/advisory/typo3-psa-2019-007/) (англ.), TYPO3, 8 мая 2019