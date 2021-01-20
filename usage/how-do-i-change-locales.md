# How do I change Locales?

Well, it would not make any sense to use i18n for just one locale, would it? That is why you need to be able to change the application's locale programmatically. You can do this in two ways:

1. Using the i18n mixin method `setfwLocale()`.
2. Using the i18n service `setfwLocale()`.

Below you can see an example:

```javascript
function changeLocale( event, rc, prc ){
    setFWLocale( rc.locale );
    relocate('main.home');
}
```

Or use via injection

```javascript
component{

    property name="i18n" inject="i18n@cbi18n";

    function changeLocale(){
        i18n.setFWLocale( 'en_US' );
    }

}
```

