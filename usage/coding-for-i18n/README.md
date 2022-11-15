# Coding for i18n

Now that you have completed the initialization of the i18n settings in the configuration file, you are ready to code for them. There are several methods that are available to all handlers, interceptors, layouts and views that deal with i18n via runtime mixins. You can use these methods directly or go to the services directly if you inject them.

## Mapped Models

This module registers the following models in WireBox:

* `i18n@cbi18n` : Helper with all kinds of methods for localization
* `resourceService@cbi18n` : Service to interact with language resource bundles

## Mixin Helpers

The module registers the following methods for **handlers/layouts/views/interceptors**:

```javascript
/**
* Get the user's currently set locale or default locale according to settings
*/
function getFWLocale()

/**
* Set the locale for a specific user
* @locale The locale to set. Must be Java Style Standard: en_US, if empty it will default to the default locale
* @dontLoadRBFlag Flag to load the resource bundle for the specified locale (If not already loaded)
* 
* @return i18n Service
*/
function setFWLocale( string locale="", boolean dontloadRBFlag=false )

/**
* Retrieve a resource from a resource bundle with replacements or auto-loading
* @resource The resource (key) to retrieve from the main loaded bundle.
* @defaultValue A default value to send back if the resource (key) not found
* @locale Pass in which locale to take the resource from. By default it uses the user's current set locale
* @values An array, struct or simple string of value replacements to use on the resource string
* @bundle The bundle alias to use to get the resource from when using multiple resource bundles. By default the bundle name used is 'default'
*/
function getResource(
    required resource,
    defaultValue,
    locale,
    values,
    bundle,
)

// Alias to getResource
function $r()

/**
 * Get Access to the i18n Model
 */
function i18n()

/**
 * Get the resource service model
 */
function resourceService()
```

## Bundle Aliases

You can leverage the `getResource() or $r()` method to retrieve resources from specific bundles by using the `bundle` argument or the `@bundle` convention.

```javascript
// Direct
#getResource( resource="common.ok", bundle="cbcore" )#

// Convention
#getResource( "common.ok@cbcore")#
```

We do like our `@bundle` convention as it looks prettier and you type a lot less.

{% hint style="info" %}
all bundles, including the default one, have names (aliases). If you want you can specify a resource in your default bundle as&#x20;

```javascript
#getResource( "some.resource@default")#
// OR
#getResource( "some.resource")#
```
{% endhint %}

## Examples

Here are some examples of usage

```javascript
Your locale is #getFwLocale()#!

// localized button
#html.submitButton( value=getResource( 'btn.submit' ) )#

// Localized string with replacements via arrays
#html.h2( $r(resource="txt.hello", values=[ "Luis", "Majano" ] ) )#

// txt.hello resource
txt.hello="Hello Mr {1} {2}! I hope you have an awesome day Mr. {2}!"

// Localized string with replacements via structs
#html.h2( getResource(resource="txt.hello", values={ name="Luis", age="35" } ) )#

// txt.hello resource
txt.hello="Hello Mr {name}! You are {age} years old today!"

// Localized string from a bundle, notice the @cbcore alias
#getResource( 'common.ok@cbcore' )#

// function change a user's locale
function changeLocale(event,rc,prc){
    setFwlocale( rc.locale );
    setNextEvent( 'home' );
}
```

> **INFO** Please note that the i18N services are singleton objects and lives throughout your entire application span.

You can place the `i18n@cbi18n` service in the `prc` scope and then use this utility for i18n specific methods anywhere in your layouts and views, below is a simple example:

```javascript
<cfoutput >
<strong>Locale:</strong> <br />#prc.i18n.getFWLocale()#<br />
<strong>Language:</strong> <br />#prc.i18n.getFWLanguage()#<br />
<strong>Language Code:</strong> <br />#prc.i18n.getFWLanguageCode()#<br />
<strong>Language ISO3 Code:</strong> <br />#prc.i18n.getFWISO3LanguageCode()#<br />


<strong>Country:</strong> <br />#prc.i18n.getFWCountry()#<br />
<strong>Country Code:</strong> <br />#prc.i18n.getFWCountryCode()#<br />
<strong>Country ISO3 Code3:</strong> <br />#prc.i18n.getFWISO3CountryCode()#<br />


<strong>TimeZone:</strong> <br />#prc.i18n.getServerTZ()#<br />
<strong>i18nDateFormat:</strong> <br />#prc.i18n.i18nDateFormat(prc.i18n.toEpoch(now()),1)#<br />
<strong>i18nTimeFormat:</strong> <br />#prc.i18n.i18nTimeFormat(prc.i18n.toEpoch(now()),2)#<br />
<hr>
<strong>I18NUtilVersion:</strong> <br />#prc.i18n.getVersion().I18NUtilVersion#<br>
<strong>I18NUtilDate:</strong> <br />#prc.i18n.dateLocaleFormat(prc.i18n.getVersion().I18NUtilDate)#<br>
<strong>Java version:</strong> <br />#prc.i18n.getVersion().javaVersion#<br>
</cfoutput>
```

**INFO** There are tons of great utility methods in the i18n service that will allow you to work with localization and internationalization. Please view them via the API Docs for the latest methods and arguments.
