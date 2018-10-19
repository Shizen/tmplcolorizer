
#{ Callouts

A variety of rules fragments are in the `scratch.txt` file at the root of the project, where I was converting between various formats by hand (tut, tut).

PList, YAML, JSON

@See also [Language Colorizers.md](index://vscode.extensions.colorizers).

}

[Sublime HTML colorizer](https://github.com/bradrobertson/sublime-packages/blob/master/HTML/HTML.tmLanguage)  
With this mod: [stackoverflow](https://stackoverflow.com/questions/9655039/sublime-text-2-recognize-underscore-templates-as-html/11886848#11886848)

I also took parts of vscode's js colorizer.

Could start from [vscode's html colorizer](https://github.com/Microsoft/vscode/blob/master/extensions/html/syntaxes/html.tmLanguage.json)

Finally [this tmLanguage file](https://github.com/johnrork/ST3-HTML-Underscore-Syntax)

Of course I converted all my original sources into xml format...

`vsce` won't honor scoped npm package names, because it's all about the *fail*.  [#186](https://github.com/Microsoft/vscode-vsce/issues/186).  No excuse for this sort of obtuse lack of interop.  Coerce standard `package.json` pattern but then denegrade it?  Inexcusable.

//"@shizen/tmplcolorizer",

- I wonder if I should `.gitignore` `scratch*`...  I mean it's not clean for other people, but it's useful for *me* ;).

## Issues

- I need to test the HTML-escaped recognizer `[=-]?` (I haven't tested it yet).
- So, I can't just include the `source.js` compiler because it barfs on js fragments (at least that's my theory).

      <key>patterns</key>
      <array>
        <dict>
          <key>include</key>
          <string>source.js</string>
        </dict>
      </array>

  Even if I *could* get this to work, I would want to mute the coloring somehow.  Ideally on mouse over, which is not possible, afaik.

- The `.tmLanguage` file has this promising decl called `<key>foldingStartMarker</key>`, but I'm reasonably convinced that it is not actually being honored by vscode.

`.tmLanguage` files utilize a slightly aberrant japanese version of regex with some extended rules.  The link in the TextMate documents to the full documentation on that regex engine are dead.  By and large this isn't much of an issue, and I have not verified that VSCode actually uses the documented regex engine, but existing tmLanguage files *do* make use of some of the specialized rules.  To what effect I'm not sure--I don't have the documentation :).

## Feature Thoughts

- Pursuant to my thoughts and issues with parsing embedded js, ideally the system would actually only colorize the embed level I was currently moused over/editting.  This is two-fold UI.
  1. Identify if I'm currently typing/cursor focused (so either typing or cursor commanding, which, in the end is still typing).  This effectively boils down to the last UI user event being either from the keyboard or the mouse.
  2. Based then on 1. take either the mouse or cursor position, and do the styling for that level of code *only*.  So if in a `<?js>` tag, all the js embedded code would be colorized, but none of the html level (or other embedded source).  If in the html context, the html tags would be colorized and none of the js stuff.

