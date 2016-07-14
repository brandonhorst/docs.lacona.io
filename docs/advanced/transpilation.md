# Transpilation

Lacona addons make use of a lot of syntactic sugar provided by
[ES2015](https://babeljs.io/docs/learn-es2015/) and
[JSX](https://facebook.github.io/react/docs/jsx-in-depth.html).

Addons are compiled using [Babel](https://babeljs.io/).

Additionally, the Lacona command execution environment cannot make use of
the `require` function to load external modules. As a result, any modular code
(or any code using external modules like `lodash` or `rxjs`) must be
compiled using [Browserify](https://github.com/substack/node-browserify).

This setup is all handled for you with `lacona init`.

You do not *need* to use transpilation, but it is *heavily recommended*.

## Example

```jsx
/** @jsx createElement */
import {URL} from 'lacona-phrases'
import {createElement} from 'elliptical'

const MyHomepagePhrase = {
  extends: [URL],
  
  describe () {
    return (
      <argument text='homepage'>
        <literal text='my homepage' value='http://lacona.io' />
      </argument>
    )
  }
}

export default [MyHomepagePhrase]
```

-- becomes --

```js
var URL = require('lacona-phrases').URL
var createElement = require('elliptical').createElement

var MyHomepagePhrase = {
  extends: [laconaPhrases.URL],

  describe: function describe () {
    return createElement('argument', {text: 'homepage'},
      createElement('literal', {text: 'my homepage', value: 'http://lacona.io'})
    )
  }
}

exports.default = [MyHomepagePhrase]
```
