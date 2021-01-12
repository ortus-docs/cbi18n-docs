# Configuration With No Resource Bundles

You can also use the config declaration for just tracking user locales and no resource bundles support

```javascript
//i18n & Localization
i18n = {
    defaultLocale = "en_es",
    localeStorage = "cookie"
};
```

You eliminate the resource bundle element or leave it blank. The framework will not load the resource bundles.

> **IMPORTANT** All language resource bundles are stored in the configuration structure of your application and are lazy loaded in. So if a language is not used, then it does not get loaded. This is separate to where the user's locale information is stored.

