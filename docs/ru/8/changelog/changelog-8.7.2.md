---
id: changelog-8.7.2
title: 'Drupal 8.7.2'
path: /docs/8/changelog/8.7.2
core: 8
metatags:
  title: 'Drupal 8.7.2: Список изменений'
  description: 'Исправление критических багов и прочие изменения.'
---

**Дата релиза**: 23 мая 2019 г.

Это патч-релиз Drupal 8, готовый к использованию на продакшен сайтах.

Следующие критические ошибки были исправлены в данном релизе:

- [#3052271](https://www.drupal.org/project/drupal/issues/3052271) Исправлена ошибка обновления `media_library_update_8701()` при обновлении с Drupal 8.6.15.
- [#3052147](https://www.drupal.org/project/drupal/issues/3052147) Исправлена ошибка обновления `comment_update_8701()`, если комментарии не имеют поля `field_name`.
- [#3052467](https://www.drupal.org/project/drupal/issues/3052467) Исправлена ошибка обновления `system_update_8702()`, при которой происходила ошибка "Call to a member function getKey() on null".
- [#3053552](https://www.drupal.org/project/drupal/issues/3053552) Библиотека `typo3/phar-stream-wrapper` была обновлена до версии 2.1.2, где была удалена зависимость "fileinfo", приводящая к ошибкам.
- [#3052431](https://www.drupal.org/project/drupal/issues/3052431) Исправлена ошибка в `layout_builder_post_update_make_layout_untranslatable()`, при которой, функция по прежнему пыталась получить все ревизии сущности без поддержки ревизий.

## Прочие изменения

- Откачены изменения [#2822778](https://www.drupal.org/node/2822778), которые исправляли поведение модальных окон Views при активном трее тулбара. Так как оно приводит к новым проблемам с модальными окнами.
- [#3030363](https://www.drupal.org/node/3030363) Добавлен тест для проверки возможности нажатия AJAX кнопоки пробелом.
- [#3053129](https://www.drupal.org/node/3053129) Исправлены ссылки на документацию Twig, которые были изменены и теперь отдают 404.
- TODO

## Ссылки

- [Официльные примечания к выпуску](https://www.drupal.org/project/drupal/releases/8.7.2) (англ.), Drupal.org, 23 мая 2019.
