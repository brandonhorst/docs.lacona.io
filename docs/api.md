# API

Lacona commands and extensions don't do any good unless they interact with
the outside world. However, lacona addons are executed in a very limited
Javascript environment - they do not have access to standard `node.js` modules,
or to browser features like `XMLHttpRequest`. Rather, interaction with
the outside world is done through the `lacona-api` module.

`lacona-api` provides the ability to **execute system commands**,
**run spotlight queries**, **display notifications**, **open files**,
**execute Applescript**, and much more.

The most useful functions are shown here. For the full list, see
[lacona-api](https://github.com/laconalabs/lacona-api). To request
additional features,
[submit a Github issue](https://github.com/laconalabs/lacona-api/issues).

Please note that support for network requests and file reading
is not currently available, but is coming soon.
If you want to implement such a command now, you will want
to use `callSystem` to execute a Python/Ruby/perl/bash script. Node.js is not
installed on OSX by default, and should not be relied upon.

## Useful Commands

### runApplescript

```js
runApplescript({script: String}, done: Callback<Any>)
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

### callSystem

```js
callSystem({
  command: String,
  args: Array<String>
}, done: Callback<String>)
```

Runs a arbitrary system command. Calls `done` with the contents of `STDOUT`.

### showNotification

```js
showNotification({
  title: String,
  subtitle: String,
  content: String
}, done: Callback<void>)
```

Displays an OS notification (using Notification Center, on OSX).

### openURL

```js
openURL({url: String}) -> void
```

Opens a given URL (with a protocol) in the default handler.

### openFile

```js
openFile({path: String}) -> void
```

Opens a given file in the default handler. Leading `~` will be expanded.
