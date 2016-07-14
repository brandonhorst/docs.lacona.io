# Extensions

Extensions are phrases that

1. extend something other than `lacona-phrases#Command`
2. the phrase's results match the interface of the extended phrase

The result of a `lacona-phrases#URL` is a String. Any phrase that extends
`URL` should also have a String result.

Here is an example phrase that extends `lacona-phrases#URL`.

```jsx
/** @jsx createElement */
import {URL} from 'lacona-phrases'
import {createElement} from 'elliptical'

const MyExtension = {
  extends: [URL],
  
  describe () {
    return <literal text='My Homepage' value='http://lacona.io' />
  }
}

export default [MyExtension]
```

When the user calls up Lacona, every command that uses a `URL` will now be
able to use `My Homepage`. For example, you could now type
`open My Homepage` or `translate My Homepage to Spanish`.

## Extensions with Methods

The result of a `URL` is just a String, but some phrases have more complicated
results.

`lacona-phrases#PreferencePane` for example, can not be represented by a simple
string. Rather, its result should be an object with methods that do things.
In the case of `PreferencePane`, there is one method called `open`.
If you are extending
a phrase like this, one good way is to use a class definition, like so:

```jsx
/** @jsx createElement */
import {PreferencePane} from 'lacona-phrases'
import {createElement} from 'elliptical'
import {openURL} from 'lacona-api'

class OnlinePrefs {
  constructor (url) {
    this.url = url
  }

  open () {
    openURL({url: this.url})
  }
}

const MyExtension = {
  extends: [PreferencePane],
  
  describe () {
    return (
      <argument text='Preference Pane'>
        <list items={[
          {text:'Google Account Settings', value: new OnlinePrefs('https://myaccount.google.com/?pli=1')},
          {text:'Dropbox Settings', value: new OnlinePrefs('https://www.dropbox.com/account')}
        ]} />
      </argument>
    )
  }
}

export default [MyExtension]
```

This extension extends `PreferencePane`, but opens a web page rather
than a page in the System Preferences app. We are specifying the behavior
in the `open` method, calling `lacona-api@openURL`.
