# Installation

Leverage CommandBox to install into your ColdBox application:

```bash
# Latest version
install cbi18n

# Bleeding Edge
install cbi18n@be
```

This module registers the following models in WireBox:

* `i18n@cbi18n` : Helper with all kinds of methods for localization
* `resourceService@cbi18n` : Service to interact with language resource bundles

## Mixins Helper methods

This module will register the following methods in your handlers/interceptors/layouts/views

* `getFWLocale()` gets the users currently set locale (or default locale)
* `setFWLocale()` set the locale for a specific user
* `getResource()`  retrieve a resource from a resource bundle with replacements
* `$r()` shortcut/alias to `getResource()`
* `i18n()` gets the i18n Model
* `resourceService()` gets the Resource Service

### Delegates

You can use the `Resourceful@cbi18n` delegate to add resource traits to your objects and access to the `getResource()` method.

```javascript
component name="SecurityService" delegates="Resourceful@cbi18n" {

   // Then just use the getResource() method
}
```

## System requirements

* Lucee 5+
* Adobe ColdFusion 2018+
