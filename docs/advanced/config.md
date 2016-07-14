# User Preferences

Lacona Addons can add entries to the Lacona Preferences page. The selected
settings are then made available Extensions through an object called `config`.

Create a preferences schema in `config.json` using
[confine](https://github.com/brandonhorst/confine#confine-schemas).

## Example

### `config.json`

```
{
  "myCustomPrefs": {
    "type: "object",
    "title": "Custom Preferences",
    "properties": {
      "homepage": {
        "type": "String",
        "default": "http://lacona.io"
      }
    }
  }
}
```

After installing this addon, the Lacona Preferences should have a new entry
in the sidebar - `Custom Preferences`, which contains a single string entry
labeled `Homepage`. Its value will default to `http://lacona.io`,
but the user can change it.

### `extensions.json`

The full config object will then be made available to the `describe` function.

```
/** @jsx createElement */
import { createElement } from 'elliptical'
import { Command } from 'lacona-phrases'
import { openURL } from 'lacona-api'

export const MyNewCommand = {
  extends: [Command],

  execute (result) {
    openURL({url: result})
  },

  describe ({config}) {
    return (
      <literal
        text='visit my homepage'
        value={config.myCustomPrefs.homepage} />
    )
  }
}

export default [MyNewCommand]
```
