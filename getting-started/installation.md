# Installation

Leverage CommandBox to install into your ColdBox appL

```bash
# Latest version
install cbi18n

# Bleeding Edge
install cbi18n@be
```

This module registers the following models in WireBox:

* `i18n@cbi18n` : Helper with all kinds of methods for localization
* `resourceService@cbi18n` : Service to interact with language resource bundles

## Mixins- Helper methods

This module will register the following methods in your handlers/interceptors/layouts/views

* `getFWLocale()` gets the users currently set locale \(or default locale\)
* `setFWLocale()` set the locale for a specific user
* `getResource()`  retrieve a resource from a resource bundle with replacements
* `$r()` shortcut/alias to `getResource()`

## System requirements

* Lucee 5+
* Coldfusion 2016+

## Module settings

The module can be configured by adding a cbi18n key in the moduleSettings structure within the config/Coldbox.cfc

```javascript
config/coldbox.cfc

// Module settings
moduleSettings = {
    cbi18n = {
				defaultResourceBundle = "includes/i18n/main",
				resourceBundles = {
					"support" = "includes/i18n/support"
				},
				defaultLocale = "en_US",
				localeStorage = "cookieStorage@cbstorages",
				unknownTranslation = "**NOT FOUND**",
				logUnknownTranslation = true,
				resourceType="java"
		}
		
};
```

`cbi18n` capabilities can be added to other modules by adding a `cbi18n` configuration key to the settings of other modules in ModuleConfig.cfc.

