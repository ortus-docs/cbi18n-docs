# Custom Resource Services

If you elect not to use property files for your resource bundles, or need to expand your i18n implementation to include content pulled from other sources, the module allows for the specification of a `customResourceService` in the configuration.

To begin, create a custom resource service which extends the core i18n resource service:

```javascript
component 
  name="MyCustomResourceService" 
  extends="cbi18n.models.ResourceService" 
  singleton=true{
  
  property name="Controller" inject="coldbox";
  property name="Wirebox" inject="wirebox";

}
```

To override the resource bundle used, you would declare a new `loadResourceBundle` method. To override the implementation of the `getResource( i18nResourceLabel )` you would declare a new `getResource` method. An example, using your database to retrieve an i18n resource bundle:

```javascript
public void function loadBundle( 
    required string rbLocale=VARIABLES.i18n.getfwLocale() 
){
    var bundles = "";

    // Lazy Load?
    if( NOT VARIABLES.controller.settingExists("RBundles") ){
        VARIABLES.controller.setSetting("RBundles",structnew());
    }

    //set bundles ref
    bundles = VARIABLES.controller.getSetting("RBundles");
    var rb = structNew();

    //cache our resource bundle query
    var qRb = new query( datasource=application, cachedWithin=CreateTimeSpan(0, 6, 0, 0) );
    qRb.addParam( name="locale", value=arguments.rbLocale, cfsqltype="cf_sql_varchar" );
    qRb.setSQL( "SELECT * from customI18nTable WHERE locale=:locale" );
    var rbResult = = qRb.execute().getResult();
    for( var resource in rbResult ){
        rb[ resource.label ] = resource.value;
    }

    bundles[ arguments.rbLocale ] = rb;
}
```

Lets say we want to account for a more complex metadata structure in our resource bundles. For example, allow a `draft` value to allow for administrators to see which items in our bundle are pending review but display the default value to the public. We might also want to add any new resource requests containing a default value as drafts in the system (which we won't cover here). For special circumstances, such as these, we would need to change the implementation of the `getResource` method to account for this draft functionality:

```javascript
public string function getResource(
    // The resource (key) to retrieve from the main loaded bundle.
    required any resource,
    //A default value to send back if the resource (key) not found
    any default,
    //Pass in which locale to take the resource from. By default it uses the user's current set locale
    any locale="#VARIABLES.i18n.getfwLocale()#",
    //An array, struct or simple string of value replacements to use on the resource string
    any values,
    //default
    any bundle="default"
){
    //prevent errors via empty label strings from being passed
    if( !len( trim(resource) ) ) return resource;

    //protect from a bad locale value being passed
    if( !len( trim( arguments.locale ) ) ) arguments.locale = VARIABLES.i18n.getfwLocale();

    var thisLocale         = arguments.locale;
    var resourceString = "";

    var rBundles = Controller.getSetting("RBundles");
    var currentUser = Wirebox.getInstance( "SessionService" ).getCurrentUser();

    if( !structKeyExists( rBundles, arguments.locale ) ){
        loadBundle( rbLocale=arguments.locale, rbAlias=arguments.bundle );
    }

    var resourceBundle = rBundles[ arguments.locale ];

    if( structKeyExists( resourceBundle, arguments.resource  ){
        var resource = resourceBundle[ arguments.resource ];
        if( resource.published ){
            return formatResourceObject( resourceObj=resource, args=arguments );
        } else if( !resource.published && currentUser.isAdministrator() ) {
            return formatDraftResource( resourceObj=resource, args=arguments );
        }
    } else if( !isNull( arguments.default ) ) {
        if( !isNull( arguments.values ) ){
            return formatRBString( arguments.default, arguments.values );
        } else {
            return arguments.default;
        }
    } else {
        return renderUnknownTranslation( arguments.resource );
    }
}
```

The ability to extend and implement custom resource services provides you with a great deal of flexibility in your internationalization efforts. Use cases might include database-driven i18n resources, distributed caching implementations, and variable bundle formats used in third-party tools and plugins.
