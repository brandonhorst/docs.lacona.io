# Previews

Previews are the popover results that appear to the side of selected options.
These are useful for displaying results to the user before actually executing them.
Search results, computation results, basic information, and previews are all
good candidates to show up in the Preview Popover.

Every command can display a preview if it chooses to. This is done by having a
`preview` function. This is passed the `results` from the parse as well as
the same object that is passed to `describe`, containing

```js
{
  props: Object<Any>,
  observe: (Element<Source>) => Any,
  config: Object<Any>
}
```

It can use the `observe` function in the same way as that `describe` - by
passing an Element describing a Source, which will return the current value
of the source.

## Return Values

If the preview function returns an object, it will be displayed in the preview
popover for a given option. The returned object should be in the form

```js
{
  type: 'text' | 'html' | 'tex',
  value: String
}
```

Where `value` depends on `type`.

If `type` is `'text'`, then `value` should be a string that will be displayed
in the preview window. If `type` is `'html'`, `value` should be a string that
will be interpreted as an HTML string. If `type` is `'tex'`, `value` will be
interpreted as a TeX string, and rendered using
[KaTeX](https://khan.github.io/KaTeX/).
