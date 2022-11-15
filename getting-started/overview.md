# Overview

The cbi18n module was built to provide localization and internationalization features to any ColdBox application.  It will not only allow you to represent resources in multiple languages, but will also track the user's locale for you.  It has a plethora of utilities for localizing strings, dates, currencies and much more.

There are two main models that are registered for you with the following WireBox Id's:

* `i18n@cbi18n` : Service that tracks user's locale, changing of locales, and a plethora of localized functions.  It also bootstraps the resource bundles used in the application.
* `ResourceService@cbi18n` In charge of retrieving language keys from locale specific resource bundles, whether they are Java property files or JSON bundles.

They can either be injected, called via our mixin helpers or added via our ColdBox Delegates:

```javascript
// Mixins
i18n()
resourceService()

// Injection
property name="i18n" inject="i18n@cbi18n";
property name="resourceService" inject="resourceService@cbi18n"

// Delegates
component delegates="Resourceful@cbi18n"{
 
 ...
    return "Hello + #getResource( 'myResource' )#";
 ...
}
```

Once you have access to those objects you can leverage their methods to your ❤️'s content.

```java
Your locale is #getFwLocale()#!

// localized button
#html.submitButton( $r( 'btn.submit' ) )#

// Localized string with replacements via arrays
#html.h2( $r(resource="txt.hello", values=[ "Luis", "Majano" ] ) )#

// txt.hello resource
txt.hello="Hello Mr {1} {2}! I hope you have an awesome day Mr. {2}!"

// Localized string with replacements via structs
#html.h2( $r( resource="txt.hello", values={ name="Luis", age="35" } ) )#

// txt.hello resource
txt.hello="Hello Mr {name}! You are {age} years old today!"

// Localized string from a bundle, notice the @cbcore alias
#$r( 'common.ok@cbcore' )#

// function change a user's locale
function changeLocale(event,rc,prc){
	setFwlocale( rc.locale );
	relocate( 'home' );
}
```

Please see the API docs for the latest methods available to you:

{% embed url="https://apidocs.ortussolutions.com/coldbox-modules/cbi18n/3.0.0/index.html" %}
