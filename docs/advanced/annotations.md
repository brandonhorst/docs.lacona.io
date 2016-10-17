# Annotations

Annotations allow you add supplementary information to your grammars.
For example, `lacona-finder` allows the user to open Applications. It uses
annotations to show the application icon next to the application name.

Annotations can be added to any Element using the `annotations` or `annotation` prop.
They can also be set as part of a `list`'s `items`.

Annotations must be an object of this format:

```js
{
  type: 'icon' | 'text' | 'image',
  value: String
}
```

Where `value` depends on `type`.

If `type` is `'icon'`, `value` should be a string representing the absolute
file system path where a file resides. Lacona will display the icon that the
operating system uses to display the file. This is used for files, bookmarks,
applications, and custom icons.

If `type` is `'image'`, `value` should be a string representing the absolute
file system path where an image file resides. Lacona will display the contents
of that file. This is generally used for custom images.

If `type` is `'text'`, `value` should be an arbitrary string. Lacona will
display it before the Element in the results. Emoji are acceptable and
encouraged.

## Example

```jsx
<literal text='Australia' annotation={{type: 'text', value: '🇦🇺'}} />
```