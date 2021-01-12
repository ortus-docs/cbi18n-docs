# Release History

In this section you will find the release notes for each version we release under this major version.  If you are looking for the release notes of previous major versions use the version switcher at the top left of this documentation book.  Here is a breakdown of our major version releases.

## Version 2.0

The module has been rewritten in CFScript. 

cbi18n v1 already had java resource bundles. v2 added support for JSON resource bundles. Both flat and nested JSON bundles are supported.

In this version we introduced the concept of property inheritance. It means that key-values pairs included in less specific files are inherited by those which are higher in the inheritance tree. For example: Let's assume you are using a en\_US locale. if you have key-values in a generic `myResource.properties` file,  you can override them in a `myResource_en.properties` and an even more specific `myResource_en_US.properties` file.  This opens up the possibility to define a generic language resource and define country specific translations in a more specific resource file. 

For locale storage we now use a service from the [cbstorages](https://www.forgebox.io/view/cbstorages) module. 

## Version 1.0

A \( very old \) initial release.

