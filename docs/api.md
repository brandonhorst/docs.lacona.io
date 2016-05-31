# API

Lacona commands and extensions don't do any good unless they interact with
the outside world. However, lacona addons are executed in a very limited
Javascript environment - they do not have access to standard `node.js` modules,
or to browser features like `XMLHttpRequest`. Rather, interaction with
the outside world is done through the `lacona-api` module.

`lacona-api` provides the ability to **execute system commands**,
**run spotlight queries**, **display notifications**, **open files**,
**execute Applescript**, **call out to Node.js**, and much more.

The most useful functions are shown here. For the full list, see
[lacona-api](https://github.com/laconalabs/lacona-api). To request
additional features,
[submit a Github issue](https://github.com/laconalabs/lacona-api/issues).

Please note that if you want to support network requests, file reading, etc,
you need to use `callNode` or `callSystem`. There are no built-in API calls to
handle hese functions.

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

### callNode (UNRELEASED)

```js
callNode({
  func: Function,
  args: Array<Any>
}, done: Callback<Error?, Any, String>
```

Executes a function `func` in a [node.js](https://nodejs.org/en/) environment.
This function **can** make use of the node standard library through
`require`.

This node executable is bundled with Lacona - it does not use the node installed
on the user system, if one exists.

`func` will be passed any arguments specified in `args`, followed by a
`callback` function. That callback should be called `callback(err, result)`
`result` is any JSON-serializable object.

The `done` callback will be called with 3 arguments: an Error or null,
the `result` object, and a newline-joined string of all `console.log` and
`console.error` statements called.

#### Limitations

- As it is executing in an entirely different environment, the context of the passed osaFunction is completely ignored. It cannot behave like a closure or modify any external variables.
- As JSON is used as the transport mechanism, only Objects, Arrays, Numbers, Strings, true, false, and null can be passed back and forth between the two environments. That is to say, you cannot return an complex object (Streams, Observables, Promises, EventEmitters, etc).
- As each call spawns off a new process and spins up a new environment, there may be some delay.

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
