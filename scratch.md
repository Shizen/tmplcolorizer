Easiest, perhaps, to start with an explanation of a few use cases.

1. [`underscore.js`](https://underscorejs.org/) is a javascript framework which contains a template feature.  These templates are basically `javascript` embedded in `html`.  One of underscore's configuration options for templates allows the "client" to choose their embed delimeter.  E.g. instead of the standard `<% %>` delimeter, it might use `<?js ?>`.  This scenario is very difficult to deal with in a standard `.tmLanguage` file.

2. In a complex project where one might be importing several packages built using different frameworks you might have `html` template files with different "default" embedded languages.  For instance, `<% %>` might be `javascript` if it was an underscore template, but it might, instead, be `Ruby` (for instance).  Which often depends on where in the project's source tree the source file lives.

It would be nice if one could define keyword parameters in one's settings, scoped by glob pattern which could then be leveraged by tmLanguage files to deal with these issues.  At the same time, using the same glob patterning, it would be nice if one could explicitly associate or override file extension to language extension mappings.  There are many possible patterns for what this might look like, but here's a first pass idea.

```json
"customizeLanguageExtensions" : {
    "blockify/**": {
      "extensions": {
        ".tmpl": "embeddedPythonExtension"
      }
    },
    "package2/**": {
      "extensions": {
        ".tmpl": "tmplcolorizer"
      },
      "parameters": {
        "underscore-delimeter" : "&lt;?js"
      }
    }
  },
```

With a usage pattern something like...

```html
<key>begin</key>
<string>(?:\s*)({underscore-delimeter}[=-]?)</string>
<key>defaults</key>
<dict>
	<key>underscore-delimeter</key>
	<string>&lt;\?js</string>
</dict>
```

I haven't looked in the source to see where this is done, per se, but this pattern is probably most easily implemented by doing an expansion when the tmLanguage file is loaded and caching it via whatever mapping (now glob indexed) vscode is already using.