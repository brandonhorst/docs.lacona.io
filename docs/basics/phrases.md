# Phrases

Lacona commands and extensions are built using Phrases. A Phrase is just an
object with a `describe` function that returns a single `Element`.

```jsx
/** @jsx createElement */
import {createElement} from 'elliptical'

const MyPhrase = {
  describe () {
    return <literal text='my homepage' />
  }
}
```

Phrases are composed out of other phrases. In this example, `MyCommand`
makes use of `MyPhrase`.

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

You can pass values to Elements by using `props`. These props will be made
available to the Phrase's `describe` function.

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

## Elliptical (lowercase) Phrases

The Lacona parser is built on top of a library called
[Elliptical](http://elliptical.laconalabs.com/).

It comes with many useful phrases built-in, which you can use in your Phrases.
All of these phrases have lowercase names, and do not need to be imported.

In the above examples, we use the `sequence` and `literal` phrases.

These phrases are:

### Basic Content Phrases

- `literal`
- `list`
- `freetext`

### Groups

- `choice`
- `sequence`
- `repeat`
- `placeholder`

### Advanced Phrases

- `dynamic`
- `map`
- `filter`
- `tap`
- `raw`

## `lacona-phrases`

Lacona has a number of pre-built phrases to accomplish certain tasks. These
are not installed automatically, but must be imported from the
`lacona-phrases` module.

You can (and should!) use these rather than implementing them yourself. Commands
and extensions that use these phrases can fully utilize the powers of extension.

If you believe that there are more general-purpose phrases that should be added
to `lacona-phrase`, please submit a
[Github Issue](https://github.com/laconalabs/lacona-phrases/issues).

### Top-level phrases (for extending)

- Command
- BooleanSetting
- BooleanCommand

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
