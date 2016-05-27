
## How does it work?

The logic is contained in `addon.jsx`.

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

export const extensions = [MyHomepagePhrase]
```

### What the hell is that?

That is a Lacona grammar, built using a lot of fancy ES2015 and JSX syntax.
This syntax is valuable for a number of reasons, but here's the equivalent
pure ES5 Javascript, for your sake. You can use either format,
but the ES2015/JSX method is recommended.

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

exports.extensions = [MyHomepagePhrase]
```
