# Grammars

Lacona grammars are written using
[Elliptical](http://elliptical.laconalabs.com/).

In addition to the built-in [phrases](docs/phrases.md), you can use all
valid [Elliptical phrases](http://elliptical.laconalabs.com/doc/apireference/phrases.html):

- `literal`
- `list`
- `choice`
- `sequence`
- `repeat`
- `label`
- `freetext` (note - you should probably use `lacona-phrases#String` in most cases)
- `filter`
- `map`
- `tap`
- `raw`
- [`dynamic`](https://github.com/brandonhorst/elliptical-observe)