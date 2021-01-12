# Configuration

The module can be configured by adding a cbi18n key in the moduleSettings structure within the config/Coldbox.cfc

```javascript
config/coldbox.cfc

// Module settings
moduleSettings = {
    cbi18n = {
 				// the resource file type, default = java
 				resourceType="java",
 				// The base path of the default resource bundle to load, default=""
				defaultResourceBundle = "includes/i18n/main",
				// additional resource bundles. default ={
				resourceBundles = {
					"support" = "includes/i18n/support"
				},
				// default locale when no user specific locale is set with setFWLocale
				defaultLocale = "en_US",
				// storage service to store locale set with setFWLocale
				localeStorage = "cookieStorage@cbstorages",
				// returned string when no translation is found, default none
				unknownTranslation = "**NOT FOUND**",
				logUnknownTranslation = true,
		}
		
};
```

`cbi18n` capabilities can be added to other modules by adding a `cbi18n` configuration key to the settings of other modules in ModuleConfig.cfc.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Required</th>
      <th style="text-align:left">Default</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">resourceType</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">java</td>
      <td style="text-align:left">java or json</td>
    </tr>
    <tr>
      <td style="text-align:left">defaultResourceBundle</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>only required for resourcebundles.</p>
        <p>This is the path for a resource bundle but <b>without </b>the lang_COUNTRY.(properties|json)
          part e.g .<code>includes/i18n/main_en_US.properties</code> should be specified
          as <code>includes/i18n/main</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">resourceBundles</td>
      <td style="text-align:left">struct</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">{}</td>
      <td style="text-align:left">key-value struct of resource alias name and bundle path <b>without </b>the
        lang_COUNTRY.(properties|json) part.</td>
    </tr>
    <tr>
      <td style="text-align:left">defaultLocale</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">en_US</td>
      <td style="text-align:left">default locale</td>
    </tr>
    <tr>
      <td style="text-align:left">localeStorage</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">cookieStorage@cbstorages</td>
      <td style="text-align:left">cbstorages service where current locale is stored</td>
    </tr>
    <tr>
      <td style="text-align:left">unknownTranslation</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">if no unknowTranslation is set getResource will fail on unknown resourceKeys</td>
    </tr>
    <tr>
      <td style="text-align:left">logUnknownTranslation</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">no</td>
      <td style="text-align:left">false</td>
      <td style="text-align:left">will log unknown translations to logbox</td>
    </tr>
  </tbody>
</table>

Each module in your ColdBox Application can have its own resource bundles that can be loaded by this module and thus providing modular i18n resources. 

{% hint style="danger" %}
When specifying resources in a MODULE, you should **never** specify a defaultResourceBundle, because it will conflict with your main settings.   
If you specify additional resourceBundles it is wise to choose an aliasname which will not conflict with other modules or your main settings
{% endhint %}

A resource bundle can be a `.properties` file containing your translations \(java resources\) OR a `.json` file.  For instance, using the default settings above, you would need to create a `includes/i18n/main_en_US.properties` file with your translations. If you want to use JSON files, you should  set `resourceType=json` and use an  `includes/i18n/main_en_US,json` file.

Here is an example:

```text
main.readthedocs=Read The Docs!
greeting=Hello {name}
```

