---
id: changelog-8.8.0
title: 'Drupal 8.8.0'
path: /docs/8/changelog/8.8.0
core: 8
metatags:
  title: 'Drupal 8.8.0: Список изменений'
  description: 'Актуальный список изменений Drupal 8.8.0, релиз которого запланирован на 4 декабря 2019 г.'
---


> [!IMPORTANT]
> Будущий релиз Drupal. Не предназначен для использования на продакшен сайтах. Список изменений неполный и многое может измениться.

**Дата релиза**: 4 декабря 2019 г.\
**Перевод на поддержку безопасности**: 3 июня 2020 г.\
**Окончание поддержки**: +- полгода после перевода на поддержку безопасности.

## Обновление с 8.7.x

На данный момент информация отсутствует. Она появится ближе к релизу или в день релиза.

## Прочие изменения

- [#3033540](https://www.drupal.org/node/3033540) Формы модуля `action` были перенесены в `src/Form`.
- [#3041002](https://www.drupal.org/node/3041002) CSS стандарты для сортировки теперь обрабатываются с использованием styleling-order плагином.
- [#3043694](https://www.drupal.org/node/3043694) Возможность передачи массива для `link_uri` плагина источника Migrate API помечено устаревшим.
- [#3037022](https://www.drupal.org/node/3037022) Новый сервис `ExportStorage` для помощи при экспортировании конфигураций.
- [#3038171](https://www.drupal.org/node/3038171) Модуль `user` теперь предоставляет новый маршрут поддерживающий стандарт [RFC5785](https://github.com/WICG/change-password-url), позволяющий менять пароль по well-known пути `/.well-known/change-password` (произведет 301-й редирект на форму восстановления пароля Drupal).
- [#2853355](https://www.drupal.org/node/2853355) Добавлен новый трейт `ConfigurableTrait` для уменьшения кода плагинов.
- [#2835616](https://www.drupal.org/node/2835616) Функции `entity_get_display()` и `entity_get_form_display()` получили замену в виде сервиса `entity_display.repository`.
- [#3033656](https://www.drupal.org/node/3033656) Функции для просмотра сущностней помечены устаревшими.
- [#3051463](https://www.drupal.org/node/3051463) Функции `user_delete()` и `user_delete_multiple()` помечены устаревшими.
- [#3040204](https://www.drupal.org/node/3040204) Установка идентификатора раскрытого фильтра Views теперь обрабатываются корректно.
- [#3050078](https://www.drupal.org/node/3050078) Third party settings для форматтеров полей теперь передаются вместе с рендер элементом и доступы по ключу `#third_party_settings`.
- [#3051983](https://www.drupal.org/node/3051983) Функция `drupal_schema_get_field_value()` помечена устаревшей.
- [#2769027](https://www.drupal.org/node/2769027) Для SQLite включена опция [WAL](https://www.sqlite.org/wal.html) (Write-Ahead Logging) по умолчанию.
- [#3047897](https://www.drupal.org/node/3047897) Конструктор `NodeNewComments` изменен. В него добавили два новых параметра, которые ожидают экземпляры объектов `EntityTypeManagerInterface` и `EntityFieldManagerInterface`, соответственно. Для старого кода будет выводиться предупреждение об ошибке и сервисы будут автоматически получены из глобального контейнера.
- [#3030340](https://www.drupal.org/node/3030340) Объект `WebTestBase` помечен устаревшим.
- [#3056869](https://www.drupal.org/node/3056869) Поддержка PHPUnit 4.x прекращена, теперь будет запрашиваться версия ^6.5. В соответствии с данным изменением, {Drush}(drush) команды `drupal-phpunit-upgrade` и `drupal-phpunit-upgrade-check` больше не нужны и были удалены.
- [#3056639](https://www.drupal.org/node/3056639) `MailManagerInterface::mail()` теперь позволяет переопределять сообщение об ошибке.
- [#3035273](https://www.drupal.org/node/3035273) Несколько функций относящихся с к uri и scheme файлов помечены устаревшими и перенесены в `\Drupal\Core\StreamWrapper\StreamWrapperManagerInterface`.
- [#2943918](https://www.drupal.org/node/2943918) `ConfigImporter` теперь также получает сервис `extension.list.module` в качестве аргумента.
- [#3059344](https://www.drupal.org/node/3059344) Добавлен подкласс `\Symfony\Component\Validator\ConstraintViolation` в Drupal `\Drupal\Core\Validation\ConstraintViolation` для использования в `\Drupal\Core\TypedData\Validation\ExecutionContext::addViolation()`.
- [#3057326](https://www.drupal.org/node/3057326) Передача File сущности в качестве первого аргумента в `assertFileExists` и `assertFileNotExists` помечена устаревшей.

## Ссылки

- [Change records for Drupal](https://www.drupal.org/list-changes/drupal) (англ.)