RulerZBundle
============

This bundle integrates [RulerZ](https://github.com/K-Phoen/rulerz) into Symfony.

Installation
------------

Require the bundle:

```
composer require 'kphoen/rulerz-bundle'
```

And declare it into your `app/AppKernel.php` file:

```php
public function registerBundles()
{
    return array(
        // ...
        new KPhoen\RulerZBundle\KPhoenRulerZBundle(),
    );
}
```

Usage
-----

This bundle registers a `rulerz` service which is an instance of `RulerZ\RulerZ`.

```php
$rulerz = $this->container->get('rulerz');

$rulerz->filter(/* ... */);
```

### Cache

RulerZ supports [caching rules](https://github.com/K-Phoen/rulerz/blob/master/doc/caching_rules.md).
The backend used for this cache can be specified directly in the configuration:

```yaml
# app/config/config.yml

kphoen_rulerz:
    cache:
        provider: rulerz_cache
        lifetime: 86400         # seconds, optionnal
```

Where `rulerz_cache` is a service implementing \Doctrine\Common\Cache\Cache.
By default, no cache is used.

**Pro-tip**: the [DoctrineCacheBundle](https://github.com/doctrine/DoctrineCacheBundle)
can be used to easily manage cache backends.

### Custom operators

[Custom operators can be added](https://github.com/K-Phoen/rulerz/blob/master/doc/custom_operators.md)
to RulerZ executors.
The bundle provide a way to register new operators directly from the container,
you just need to tag a service:

```yaml
services:
    operator.array.like:
        class: RulerZ\Operator\ArrayExecutor\Like
        tags:
            - { name: rulerz.operator, executor: RulerZ\Executor\ArrayExecutor, operator: like }
```

In addition to the `rulerz.operator` parameter, two other values are needed:
* `executor`: the class we want to register the operator to ;
* `operator`: the name that will be given to the operator.

**Important**: Operators registered as classes must implement the `__invoke`
magic method as RulerZ expects custom operators to be defined as callable.


Licence
-------

This bundle is under the [MIT](https://github.com/K-Phoen/RulerZBundle/blob/master/LICENSE) licence.
