# Whats New With 2.0.0

## Compatibility Updates

### CFML Engines

Compatibility with Adobe ColdFusion 11 and Lucee 4.5 have been dropped.

### Module Configuration Settings

You will need to move the `i18n` configuration structure in your `config/ColdBox.cfc` to the `moduleSettings` struct and rename it to `cbi18n` to standardize it to module settings.

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

### ColdBox Module Settings

Every ColdBox module has the i18n capabilities available to them as well.  They can use it to register their own resource bundles. The previous version allowed for a setting called `i18n` this is now called `cbi18n` to comply with the same global naming convention.  Just update your key root in your `Moduleconfig.cfc`

{% code title="ModuleConfig.cfc" %}
```javascript
cbi18n = {
		defaultLocale = "es_SV",
		resourceBundles = {
			"module@test1" = "#moduleMapping#/includes/module"
		}
};
```
{% endcode %}

### Locale Storage Options

* `localeStorage` will use a [cbstorages](https://www.forgebox.io/view/cbstorages) compatible service now. This means you have to specify the WireBox ID now, e.g `CookieStorage@cbstorages` , `SessionStorage@cbstorages` etc

## Major updates

### JSON Resources

`cbi18n` v1 already had java resource bundles. v2 added support for JSON resource bundles. Both flat and nested JSON bundles are supported.

#### Flat

{% code title="includes/i18n/jsontest\_en\_US.json" %}
```javascript
{
    "sub.IntroMessage": "Normal JSON"
}
```
{% endcode %}

{% code title="includes/i18n/jsontest\_es\_SV.json" %}
```javascript
{
    "sub.IntroMessage": "JSON Normal"
}
```
{% endcode %}

#### Nested

{% code title="includes/i18n/nested\_en\_US.json" %}
```javascript
{
    "sub": {
        "IntroMessage": "Nested JSON"
    }
}
```
{% endcode %}

{% code title="includes/i18n/nested\_es\_SV.json" %}
```javascript
{
    "sub": {
        "IntroMessage": "JSON Incrustado"
    }
}
```
{% endcode %}

### Locale Storage

The locale storage has now moved to leveraging a [cbStorages](https://www.forgebox.io/view/cbstorages?#description) compliant driver. This way you can leverage any of the [cbStorages](https://www.forgebox.io/view/cbstorages?#description) drivers or anything custom you would like.

* applicationStorage@cbstorages
* cacheStorage@cbstorages
* clientStorage@cbstorages
* **cookieStorage@cbstorages \(default\)**
* requestStorage@cbstorages
* sessionStorage@cbstorages

### Property Inheritance

In this version we introduced the concept of property inheritance. It means that key-values pairs included in less specific files are inherited by those which are higher in the inheritance tree. For example: Let's assume you are using a en\_US locale. if you have key-values in a generic `myResource.properties` file,  you can override them in a `myResource_en.properties` and an even more specific `myResource_en_US.properties` file.  This opens up the possibility to define a generic language resource and define country specific translations in a more specific resource file.

The order of lookup is:

* `myBundle_en_US_somevariant.(properties|json)`
* `myBundle_en_US.(properties|json)`
* `myBundle_en.(properties|json)`
* `myBundle.(properties|json)` 

### New Mixin Helpers

You get some brand new helpers in this release:

```javascript
/**
 * Get i18n Model
 */
function i18n()

/**
 * Get the resource service model
 */
function resourceService()
```

### Unknown Translation Interception

The event `onUnknownTranslation` is emitted via an interception when a translation is not found.  You will receive the following interception data packet:

```javascript
{ 
    resource 	= ..., 
    locale 		= ... , 
    bundle  	= ... 
}
```

 

