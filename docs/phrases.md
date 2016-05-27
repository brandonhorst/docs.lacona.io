# Phrases

Lacona commands and extensions are built using Phrases. A Phrase is just an
object with a `describe` function that returns an `Element`.

```jsx
/** @jsx createElement */
import {createElement} from 'elliptical'

const MyPhrase = {
  describe () {
    return <literal text='my homepage' />
  }
}
```

Elements are specified using JSX or `createElement`, and they ultimately
just reference phrases. They can reference one another.

```jsx
/** @jsx createElement */
import {createElement} from 'elliptical'

const MyPhrase = {
  describe () {
    return <literal text='my homepage' />
  }
}

const MyCommand = {
  describe () {
    return (
      <sequence>
        <literal text='open' />
        <MyPhrase />
      </sequence>
    )
  }
}
```

Phrases can pass values to Elements by using `props`.

```jsx
/** @jsx createElement */
import {createElement} from 'elliptical'

const MyPhrase = {
  describe ({props}) {
    return <literal text={`my ${props.name}`} />
  }
}

const MyCommand = {
  describe () {
    return (
      <sequence>
        <literal text='open' />
        <MyPhrase name='homepage' />
      </sequence>
    )
  }
}
```

# `lacona-phrases`

Lacona has a number of built-in phrases provided in the `lacona-phrases` module.

You can (and should!) use these rather than implementing them yourself. Commands
and extensions that use these phrases can make proper use of extension.

If you believe that there are more general-purpose phrases that should be added
to `lacona-phrase`, please submit a
[Github Issue](https://github.com/laconalabs/lacona-phrases/issues).

### Top-level phrases (for extending)

- Command
- BooleanSetting

### Literal Phrases

- String
- Integer
- Decimal
- Ordinal
- DigitString

### Common Formats

- URL
- EmailAddress
- PhoneNumber

### Dates and Times

- DateTime
- Date
- Time
- Range
- Day
- Duration
- TimeDuration
- DateDuration

### System Phrases

- Application
- PreferencePane
- RunningApplication
- ContentArea
- MountedVolume
- File
- ContactCard
