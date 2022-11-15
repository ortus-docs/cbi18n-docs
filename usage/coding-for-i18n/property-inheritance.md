# Property inheritance

In many languages there are country or region specific words. You  wonder if you should provide resource bundles for each country while only a small subset of words is different. For this scenario you can use property inheritance which is nicely explained [here](https://www.baeldung.com/java-resourcebundle#inheritance).

So let's say I want to provide translations for english and Dutch. Because we want to provide Dutch translations for both Netherlands and Belgium we want to use the following locales: nl\_NL(dutch) and nl\_BE (flemish). Instead of duplicating the nl\_NL resource file `myResource_nl_NL`.properties to `myResource_nl_BE.properties` we can create a general `myResource_nl.properties` file and specify a `myResource_nl_BE.properties` file which only differs for a few keys. If we retrieve a resource with getResource(resource = "some.key", locale="nl\_BE) it will try to lookup this key in the following order

* `myResource_nl_BE.(properties|json)`
* `myResource_nl.(properties|json)`
* `myResource.(properties|json)`

&#x20;As you can see this lookup order does not stop at myResource.nl.(properties|json). It even goes up to a resource file without language indication. This can be especially useful if you create your app in one language, and only have incomplete translations for other languages. So let's say you create an english language app, you could just create a `myResource.(properties|json)` file and assume your main language is english. If you want to provide French as a second language but don't have the full translation yet (because it is a community effort for example) you can add `myResource.fr.(properties|json)` as a resource file, so you can provide a growing number of translated resource keys.&#x20;

It is even possible to be more specific by adding country variants, e.g `myResource.en_GB_someVariant.properties`
