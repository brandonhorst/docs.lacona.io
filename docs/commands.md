# Commands

Commands are phrases that

1. extend `lacona-phrases#Command`
2. Have an `execute` method

This is the simplest possible addon that exports a command.

```jsx
/** @jsx createElement */
import {Command} from 'lacona-phrases'
import {createElement} from 'elliptical'

const MyCommand = {
  extends: [Command],

  execute (result) {
    console.log(result)
  },
  
  describe () {
    return <literal text='Hello, world!' value='hello' />
  }
}

export default [MyCommand]
```

When the user calls up Lacona, "Hello, world!" will be presented as an option.
If the user selects it and presses Return, `execute` will be called, with the
grammar's result, printing `hello` to the console log.
