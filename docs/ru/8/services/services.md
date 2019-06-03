---
id: services
title: Сервисы
core: 8
search-keywords:
  - работа с объектами
  - где как создавать объекты
  - dependency injection
  - управление зависимостями
  - что такое сервисы
  - зачем нужны сервисы
metatags:
  title: 'Drupal 8: Сервисы (Services)'
  description: 'Сервисы — PHP объекты, которые поддерживают Dependency Injection.'
---

**Сервис** - PHP объекты в Drupal, контролируемые при помощи контейнеров и объявленные соответствующим образом.

Сервисы позволяют управлять объектыми в новом формате, это делает их переиспользованиее более простым, добавляет явное определение зависимостей от других сервисов, а также возможность изменять их поведение из других модулей, если это необходимо.

> [!TIP]
> Вы можете найти определение всех сервисов, используемых ядром в файле **core.services.yml**.

## Обращение к сервисам

Drupal поддерживает два способа обращения к сервисам - при помощи глобального объекта `\Drupal` и dependency injection для {плагинов}(plugins:8), {форм}(form-api:8) и т.д.

> [!NOTE]
> Обращение к сервисам при помощи глобального объекта `\Drupal` не рекомендуемый способ. Он предназначен для legacy кода, который не поддерживает Dependency Injection вариант, например, для {хуков}(hooks:8).

### Dependency Injection

Dependency Injection предпочтительный способ внедрения сервисов в кодовую базу. Он подраузмевает что объекты сервисов, будут передавать в качестве аргументов конструктора объекта. Практически все ООП-реализации предоставляют возможность внедрения зависимостей.

Пример Dependency Injection подключения зависимостей для {формы}(form-api:8):

```php
class MyForm extends ConfigFormBase {

  /**
   * The media storage.
   *
   * @var \Drupal\media\MediaStorage
   */
  protected $mediaStorage;

  /**
   * The media view builder.
   *
   * @var \Drupal\Core\Entity\EntityViewBuilderInterface
   */
  protected $mediaViewBuilder;

  /**
   * Constructs a FrontpageSettingsForm object.
   *
   * @param \Drupal\Core\Config\ConfigFactoryInterface $config_factory
   *   The factory for configuration objects.
   * @param \Drupal\Core\Entity\EntityTypeManagerInterface $entity_type_manager
   *   The entity type manager.
   *
   * @throws \Drupal\Component\Plugin\Exception\InvalidPluginDefinitionException
   * @throws \Drupal\Component\Plugin\Exception\PluginNotFoundException
   */
  public function __construct(
    ConfigFactoryInterface $config_factory,
    EntityTypeManagerInterface $entity_type_manager
  ) {

    parent::__construct($config_factory);

    $this->mediaStorage = $entity_type_manager->getStorage('media');
    $this->mediaViewBuilder = $entity_type_manager->getViewBuilder('media');
  }

  /**
   * {@inheritDoc}
   */
  public static function create(ContainerInterface $container): object {
    return new static(
      $container->get('config.factory'),
      $container->get('entity_type.manager')
    );
  }
```

> [!IMPORTANT]
> Dependency Injection не работает в объекте объявления собственного типа сущности. На данный момент рекомендуется использовать глобальный объект `\Drupal`, пока не будет решен [вопрос](https://www.drupal.org/node/2913224).

### Используя глобальный объект

Для легаси-кода, где нет возможности использовать Dependency Injection существует специальный глобальный объект `\Drupal`.

Некоторые сервисы в нем объявлены в виде методов, например `\Drupal::database()` (аналог вызова `\Drupal::service('database')`), все остальные можно вызывать при помощи `\Drupal::service()`.

## Ссылки

- [Service Container (Symfony)](https://symfony.com/doc/3.4/service_container.html)
- [Drupal 8: Services](https://niklan.net/blog/150), Niklan, 2017.
