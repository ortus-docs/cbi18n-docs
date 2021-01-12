# Whats New With 2.0.0

## Compatibility Updates

You will need to move the `i18n` configuration structure in your `config/ColdBox.cfc` to the `moduleSettings` struct and rename it to `cbi18n` to standardize it to module settings.

```javascript
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

* localeStorage will use a cbstorages compatible service now. This means you have to specify the wirebox ID now, e.g `CookieStorage@cbstorages` , `SessionStorage@cbstorages` etc
* cbi18n now supports property inheritance. So if your bundle `myBundle_en_US_somevariant.(properties|json)` does not exist or does not have the required property \(language key\) it will try to load and retrieve a property in the following order
  * `myBundle_en_US_somevariant.(properties|json)`
  * `myBundle_en_US.(properties|json)`
  * `myBundle_en.(properties|json)`
  * `myBundle.(properties|json)` 

## General updates

`cbi18n` v1 already had java resource bundles. v2 added support for JSON resource bundles. Both flat and nested JSON bundles are supported.

In this version we introduced the concept of property inheritance. It means that key-values pairs included in less specific files are inherited by those which are higher in the inheritance tree. For example: Let's assume you are using a en\_US locale. if you have key-values in a generic `myResource.properties` file,  you can override them in a `myResource_en.properties` and an even more specific `myResource_en_US.properties` file.  This opens up the possibility to define a generic language resource and define country specific translations in a more specific resource file. 



