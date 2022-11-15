# Configuration

## Global Configuration

The module can be configured by adding a `cbi18n` key in the `moduleSettings` structure within the `config/Coldbox.cfc`

{% code title="config/Coldbox.cfc" %}
```javascript
moduleSettings = {

		cbi18n = {
			// The default resource to load and aliased as `default`
			"defaultResourceBundle" : "includes/i18n/main",
			// The locale to use when none defined
			"defaultLocale"         : "en_US",
			// The default storage for the locale
			"localeStorage"         : "cookieStorage@cbstorages",
			// What to emit to via the resource methods if a translation is not found
			"unknownTranslation"    : "**NOT FOUND**",
			// If true, we will log to LogBox the missing translations
			"logUnknownTranslation" : true,
			// A-la-carte resources to load by name
			"resourceBundles"       : {},
			// Your own CFC instantiation path
			"customResourceService" : ""
		}
		
};
```
{% endcode %}



| Key                     | Type    | Required | Default                    | Description                                                                                                                                                                                                                                                                           |
| ----------------------- | ------- | -------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `defaultResourceBundle` | string  | no       |                            | This is the path for a resource bundle that will be treated as the default resource bundle with an alias of `default` . The path must NOT include language or variants, just the name of path prefix`includes/i18n/main_en_US.properties` should be specified as `includes/i18n/main` |
| `resourceBundles`       | struct  | no       | `{}`                       | key-value struct of resource alias name and bundle path **without** the lang\_COUNTRY.(properties\|json) part.                                                                                                                                                                        |
| `defaultLocale`         | string  | no       | `en_US`                    | default locale                                                                                                                                                                                                                                                                        |
| `localeStorage`         | string  | no       | `cookieStorage@cbstorages` | cbstorages service where current locale is  stored                                                                                                                                                                                                                                    |
| `unknownTranslation`    | string  | no       |                            | if no unknowTranslation is set getResource will fail on unknown resourceKeys                                                                                                                                                                                                          |
| `logUnknownTranslation` | boolean | no       | false                      | will log unknown translations to logbox                                                                                                                                                                                                                                               |

{% hint style="warning" %}
**WARNING**

When specifying resources in a MODULE, you should **never** specify a `defaultResourceBundle`, because it will conflict with your main settings. \
If you specify additional `resourceBundles` it is wise to choose an `aliasname` which will not conflict with other modules or your main settings
{% endhint %}

A resource bundle can be a `.properties` file containing your translations (java resources) OR a `.json` file.  For instance, using the default settings above, you would need to create a `includes/i18n/main_en_US.properties` file with your translations. If you want to use JSON files, you should use an  `includes/i18n/main_en_US.json` file.

{% hint style="info" %}
ColdBox will auto-detect the extension of the resource bundle and deal with it appropriately.  The supported extension types are:&#x20;

* `.properties` - Java resource bundle
* `.json` - JSON resource bundle (flat or nested)
{% endhint %}

**Java Properties**

{% code title="includes/i18n/main_en_US.properties" %}
```java
main.readthedocs=Read The Docs!
greeting=Hello {name}
```
{% endcode %}

#### JSON Flat

{% code title="includes/i18n/jsontest_en_US.json" %}
```javascript
{
    "sub.IntroMessage": "Normal JSON"
}
```
{% endcode %}

{% code title="includes/i18n/jsontest_es_SV.json" %}
```javascript
{
    "sub.IntroMessage": "JSON Normal"
}
```
{% endcode %}

#### JSON Nested

{% code title="includes/i18n/nested_en_US.json" %}
```javascript
{
    "sub": {
        "IntroMessage": "Nested JSON"
    }
}
```
{% endcode %}

{% code title="includes/i18n/nested_es_SV.json" %}
```javascript
{
    "sub": {
        "IntroMessage": "JSON Incrustado"
    }
}
```
{% endcode %}

## Module Configuration

Every ColdBox module has the i18n capabilities available to them as well.  They can use it to register their own resource bundles. The previous version allowed for a setting called `i18n` this is now called `cbi18n` to comply with the same global naming convention.  Just update your key root in your `Moduleconfig.cfc.`

{% code title="ModuleConfig.cfc" %}
```javascript
cbi18n = {
  resourceBundles = {
     "bundleIdentifier" = "#moduleMapping#/includes/module"
  }
};
```
{% endcode %}

The keys in the `resourceBundles` struct represents the `bundle` that can be passed to `getResource` or appended on the `resource` string with an `@` sign.

```javascript
i18n.getResource( "myTranslationKey@bundleIdentifier" );
// same as
i18n.getResource( resource = "myTranslationKey", bundle = "bundleIdentifier" );
```

## Configuration With No Resource Bundles

You can also use the config declaration for just tracking user locales and no resource bundles support

```javascript
//i18n & Localization
i18n = {
    defaultLocale = "en_es",
    localeStorage = "cookieStorage@cbstorages"
};
```

You eliminate the resource bundle element or leave it blank. The framework will not load the resource bundles.

{% hint style="danger" %}
**IMPORTANT** \
\
All language resource bundles are stored in the configuration structure of your application and are lazy loaded in. So if a language is not used, then it does not get loaded. This is separate from where the user's locale information is stored.
{% endhint %}
