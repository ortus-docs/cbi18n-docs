# Configuration

 file to load the translations.  Each locale would also need a corresponding translation file.  For example, if you were supporting the You can add a `i18n` structure of settings to your `ColdBox.cfc` or to any other module configuration file: `ModuleConfig.cfc` to configure the module:

```javascript
i18n = {
    // The base path of the default resource bundle to load
    defaultResourceBundle = "includes/i18n/main",
    // The default locale of the application
    defaultLocale = "en_US",
    // The storage to use for user's locale: session, client, cookie, request
    localeStorage = "cookie",
    // The value to show when a translation is not found
    unknownTranslation = "**NOT FOUND**",
    logUnknownTranslation = true | false,
    // Extra resource bundles to load
    resourceBundles = {
        alias = "path"
    },
    //Specify a Custom Resource Service, which should implement the methods or extend the base i18n ResourceService ( e.g. - using a database to store i18n )
    customResourceService = ""
};
```

The `defaultResourceBundle` specifies a file path to find your bundle.  For example, a `defaultResourceBundle` of `includes/i18n/main` and a `defaultLocale` of `en_US` would look for a `includes/i18n/main_en_US.propertieses_SV` locale, you would need a `includes/i18n/main_es_SV.properties` file as well.

Each module in your ColdBox Application can have its own resource bundles that can be loaded by this module and thus providing modular i18n resources.

A resource bundle is a `.properties` file containing your translations.  For instance, using the default settings above, you would need to create a `includes/i18n/main_en_US.properties` file with your translations.  Here is an example:

```text
main.readthedocs=Read The Docs!
greeting=Hello {name}
```

