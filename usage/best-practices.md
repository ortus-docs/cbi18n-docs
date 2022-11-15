# Best Practices

* Eclipse (not CFEclipse or CFBuilder) doesn't add a BOM to UTF-8 encoded files, make sure you `utf-8` as the default encoding for files.
* Always use `cfprocessingdirective`

```javascript
<cfprocessingdirective pageencoding="utf-8">
```

* This module uses the core java resource bundle flavor so you have to use a proper resource bundle tool to manage these.
