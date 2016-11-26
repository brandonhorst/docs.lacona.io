# API

Most Lacona commands and extensions don't do any good unless they interact with
the outside world. Lacona Commands run in a full [node.js](http://nodejs.org/)
environment, so things like Network Requests and File Management can be done
using the built-in node.js modules, or modules downloaded from npm.

However, oftentimes Lacona commands need to integrate more deeply with the OS
than node.js allows. This is done through a special module called `lacona-api`.

`lacona-api` provides the ability to **run spotlight queries**,
**display notifications**, **execute Applescript**, **open files**,
and much more.

The most useful functions are shown here. For the full list, see
[lacona-api](https://github.com/laconalabs/lacona-api). To request
additional features,
[submit a Github issue](https://github.com/laconalabs/lacona-api/issues).

## Useful Commands

### runApplescript

```js
runApplescript({script: String}) -> Promise<Any>
```

Run `script` as an applescript script. Any value returned will be exported
as a JSON object and provided to `done`.

This function does nothing if run on non-OSX environments.

### querySpotlight

```js
querySpotlight({
  query: String,
  attributes: Array<String>,
  directories: Array<String>,
  limit: Integer,
  liveUpdate: Boolean
}) -> Observable<Array<Object>>
```

Queries Spotlight, and returns an Observable. Matches the signature of
`mdfind`.

### showNotification

```js
showNotification({
  title: String,
  subtitle: String,
  content: String
}) -> Promise<void>
```

Displays an OS notification (using Notification Center, on OSX).

### openURL

```js
openURL({url: String}) -> Promise<void>
```

Opens a given URL (with a protocol) in the default handler.

### openFile

```js
openFile({path: String}) -> Promise<void>
```

### fetchClipboard

```js
fetchClipboard() -> Promise<{
  files: Array<String>
  urls: Array<String>
  strings: Array<String>,
  text: String [[DEPRECATED]]
}>
```

Returns an object representing the current clipboard, broken up by subject.
Most of the time, only one of files|urls|strings will have any content,
but it is possible to have multiple.

- `urls` is an array of URLS, including the scheme (e.g. `http://google.com`)
- `files` is an array of Paths, not including a scheme (e.g. `/Applications/Lacona.app/`). Directories will always end in a `/`.
- `strings` is an array of plain text strings.
- `text` contains the "default" plain text contained in the clipboard - essentially whatever you would get if you pressed CMD+v in a text editor. It should not be used.

### setClipboard

```js
fetchClipboard({
  files: Array<String>
  urls: Array<String>
  strings: Array<String>,
  text: String [[DEPRECATED]]
}) -> Promise<void>
```

Sets the current contents of the clipboard. Accepts an argument in the
same format as `fetchClipboard`.

Note that `setClipboard(await fetchClipboard())` does **not** leave the
clipboard unchanged. Any advanced clipboard data (image data,
audio data, annotated strings, url attributions, and much more) will
be lost. Do not attempt to use these two functions to mutate the clipboard -
only use it when you desire to completely overwrite the clipboard contents.

Setting `text` is the same as setting `strings` to a single-item array, but this
property is deprecated and should not be used.
