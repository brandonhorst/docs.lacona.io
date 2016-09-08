# Learning By Example

Lacona Addons are written in pure Javascript. However, first-time authors may
find the APIs somewhat complex. To understand why the API was built this way,
please read the [Rationale](../advanced/rationale.md).

For many people, the best way to learn is by looking at examples. Let's
look through a Lacona Addon, `lacona-convert-currency`, and see how it
was built.

## `package.json`

Lacona Addons are npm modules, so they must contain a
[package.json file](https://github.com/brandonhorst/lacona-convert-currency/blob/master/package.json).
Let's take a look at this file, item-by-item.

- `name`: This is the npm package name. It can be any valid npm name, but by convention, it should start with `lacona-`. **Required by npm**.
- `version`: The [semver](http://semver.org/) version. **Required by npm**.
- `description`: This description is used by both npm and Lacona, to describe what a given addon does. This can be overridden by `lacona.description`.
- `main`: This references the javascript file that exports the extensions. This can be overridden by `lacona.extensions`. Defaults to `index.js`.
- `lacona.title`: This provides a human-readable name for the Addon. **Required by Lacona**
- `lacona.config`: This references a file that contains a [confine](https://github.com/laconalabs/confine) schema describing the User Preferences configuration.
- `scripts`: NPM helper scripts. The `prepublish` script is automatically called before `npm install` and `lacona install`, so it should be used to perform transpilation.
- `keywords`: All lacona addons must have the `lacona-addon` keyword or they will not be indexed.
- `license`: Specifies the software license. An OSS license (such as MIT, Apache2, or ISC) is recommended. **Required to qualify for scholarships**
- `repository`: Points to the code repository. Hosting the code publicly on Github or Bitbucket is recommend.  **Required by qualify for scholarships**
- `devDependencies`: npm modules that are required to build or test this addon. These are installed with `npm install` and `lacona install`, but are not installed on end-user systems.
- `dependencies`: npm modules that are required to run this addon. These are installed with `npm install` and `lacona install`, and are also automatically installed on end-user systems.
- `babel`: Configuration object that tells `babel-cli` how to transpile this module. See [babel docs](https://babeljs.io/docs/usage/babelrc/) for more information.

## `config.json`

The file specified in `package.json::lacona.config` is a JSON file that defines a
[confine](https://github.com/laconalabs/confine) schema. All configuration options
will be presented to the user in the Preferences page, and the user's data will be
exposed to our Command as a the `config` variable. More detailed information is
available [here](../advanced/config.md).

```json
{
  "currencyConversion": {
    "type": "object",
    "properties": {
      "defaultCurrency": {
        "type": "string",
        "description": "This will be used if you do not specify a 'to' currency.",
        "default": "USD",
        "enum": [ "AUD", "BGN", "BRL", "CAD", "CHF", "CNY", "CZK", "DKK", "EUR", "GBP", "HKD", "HRK", "HUF", "IDR", "ILS", "INR", "JPY", "KRW", "MXN", "MYR", "NOK", "NZD", "PHP", "PLN", "RON", "RUB", "SEK", "SGD", "THB", "TRY", "USD", "ZAR" ]
      }
    }
  } 
}
```

The top-level property designates that a new tab called `Currency Conversion`
should be established for these preferences. It has a single preference -
`defaultCurrency`, which is a string that has a set list of options and a description.
It will be rendered to the user as a dropdown selector. The user's selection
will be made available as `config.currencyConversion.defaultCurrency`.

Information about using the `config` variable is available below.

## `src`

`lacona-convert-currency` uses transpilation, so the source code is all contained
in the `src` directory. Babel will transpile this code to the `lib` directory when `npm run build` is run.

## `src/extensions.jsx`

This is the meat and potatoes of the addon. Let's step through this file section-by-section.

### Pragma and Imports

```js
/** @jsx createElement */
import { createElement } from 'elliptical'
import { Command, Decimal } from 'lacona-phrases'

import _ from 'lodash'
import fetch from 'node-fetch'
import { setClipboard } from 'lacona-api'
import { fromPromise } from 'rxjs/observable/fromPromise'
import { interval } from 'rxjs/observable/interval'
import { mergeMap } from 'rxjs/operator/mergeMap'
import { concat } from 'rxjs/operator/concat'
import { filter } from 'rxjs/operator/filter'

import currencies from './currencies'
```

Lacona addons make use of JSX, an XML-in-JS syntax addition borrowed from
[React](https://facebook.github.io/react/). However, because we are not React,
we need to add a `/** @jsx createElement */` pragma and import the
`createElement` function from `elliptical` to make it do what we want it to.
`createElement` takes a Phrase or a Source and returns an Element.

We also need to import a few `lacona-phrases`, which we will user later.

We also import the `setClipboard` function from `lacona-api`. This module only
works within Lacona Addons, but it will allow us to write to the user's clipboard.

The rest of the imports are just standard npm modules. If you are not familiar
with [rxjs observables](https://github.com/ReactiveX/rxjs), you may want to
read up on them.

### The Source

Lacona Addons need to pull data from external sources, but they need to do it in
a way that does not interfere with normal processing.

This is done through Sources. You can read more [here](../advanced/sources.md).

A source is an object with a `fetch` function that returns an
`Observable`. Observables are an [ES7 proposal](https://tc39.github.io/proposal-observable/)
and are not currently a part of the language, but they can be used through libraries
like [rxjs](https://github.com/ReactiveX/rxjs) or [zen-observable](https://github.com/zenparsing/zen-observable).
Observables are a composable abstraction that describe data that changes
over time.

```js
function fetchRates () {
  return fetch(FIXER_URL)
    .then(res => res.json())
    .then(body => {
      if (body && body.rates) {
        body.rates[BASE_CURRENCY] = 1
        return body.rates
      }
    })
    .catch(e => console.error(`Error connecting to fixer.io: ${e}`))
}

const CurrencySource = {
  fetch () {
    return fromPromise(fetchRates())
      ::concat(interval(60 * 60 * 1000)::mergeMap(() => {
        return fromPromise(fetchRates())
      }))
      ::filter(_.identity)
  }
}
```

Our source, `CurrencySource` may look complicated, but it is fairly simple.
It fetches data from [fixer.io](http://fixer.io/), and then fetches again every
hour to make sure that it is ready if the data changes. The `::filter(_.identity)`
call ensures that any failures are ignored.

This Observable system many seem complicated, but ultimately, it provides
the best resource management available for the language processing system.
If you have any trouble getting Observables to work, please don't hesitate to
seek support on the [Gitter chat](https://gitter.im/laconalabs/LaconaApp).


#### The `Currency` Phrase

We start with a phrase called `Currency`. Phrases are the best way to develop
Addons in a modular way. This particular phrase allows an input of any different
name for global currency - for example, "Ruble", "Renminbi", "JPY", or "Bucks".

This Phrase does not do anything on its own - it is used by the `ConvertCurrency` Command below.

```js
const Currency = {
  describe () {
    const currencyLists = _.map(currencies, ({singular, plural, qualifiers, annotations}, code) => {
      return <list
        limit={1}
        items={singular.concat(plural || [], code)}
        qualifiers={qualifiers}
        annotations={annotations}
        value={code} />
    })

    return (
      <placeholder argument='currency'>
        <choice>{currencyLists}</choice>
      </placeholder>
    )
  }
}
```

It has a single function, `describe`, which returns an [Elliptical grammar](https://elliptical.laconalabs.com/).
For more information,
please read the [Elliptical docs](https://elliptical.laconalabs.com/).

It has some useful features - in particular, [qualifiers](../advanced/qualifiers.md),
[annotations](../advanced/annotations.md), a [placeholder and an argument](../advanced/arguments.md).

In short, qualifiers are used if multiple concepts are described using the same word, e.g. "Dollars (US)" vs. "Dollars (Canada)". Annotations are used to add non-textural supplimental information to the output - in this case, flag emoji. Arguments are used to specify the *lexical category* of the input - in this case, the word "currency". Placeholders are used to limit suggestions until the user begins to type them, to improve parsing performance and decrease noise.

This command would be somewhat usable without any of these features, so it is recommended that when you are creating your own commands, you develop incrementally - developing the core grammar before adding convenient additions.

### The `ConvertCurrency` Command

The command is the central part of the addon - it is what the user interacts with.
A command is, fundamentally, an object that has `extends: [lacona-phrases#Command]`,
a `describe` function, and an `execute` function. It may also have a `preview` function,
and ours does. You can learn more about Commands [here](commands.md).

The `ConvertCurrency` Command is the command that the user actually interacts with. 

```jsx
export const ConvertCurrency = {
  extends: [Command],

  execute (result, {observe, config}) {
    const rates = observe(<CurrencySource />)
    if (rates) {
      const converted = convert(result, rates, config.currencyConversion.defaultCurrency)
      let output

      if (converted.length === 1) {
        output = `${+converted[0].toAmount.toFixed(2)}${converted[0].to}`
      } else {
        output = _.map(converted, ({from, to, fromAmount, toAmount}) => {
          return `${fromAmount} ${from} = ${+toAmount.toFixed(2)} ${to}`
        }).join('\n')
      }

      setClipboard({text: output})
    }
  },

  preview (result, {observe, config}) {
    const rates = observe(<CurrencySource />)
    if (rates) {
      const converted = convert(result, rates, config.currencyConversion.defaultCurrency)

      const strings = _.map(converted, ({from, to, fromAmount, toAmount}) => {
        return `${fromAmount} ${from} = ${+toAmount.toFixed(2)} ${to}`
      })

      const html = strings.join('<br />')

      return {type: 'html', value: html}
    }
  },

  describe ({observe}) {
    const rates = observe(<CurrencySource />)
    if (rates) {
      return (
        <sequence>
          <list items={['convert ', 'calculate ', 'compute ']} limit={1} />
          <placeholder label='currency conversion' merge>
            <sequence>
              <Decimal id='amount' argument='amount' />
              <literal text=' ' optional limited preferred />
              <Currency id='from' ellipsis />
              <list items={[' to ', ' in ']} limit={1} />
              <repeat separator={<list items={[' and ', ', ', ', and ']} limit={1} />} id='to'>
                <Currency />
              </repeat>
            </sequence>
          </placeholder>
        </sequence>
      )
    }
  }
}
```
#### `describe`

The `describe` function determines *what kind of language a command can understand*.

Its `describe` function is fairly simple - first, it `observe`s our
`CurrencySource`, and then it returns a grammar based upon that observation.

`observe` is a special function that takes an Element describing a Source,
and returns its current value. Every time the Source changes, `describe` will
be called again and the `observe` call will return a different value.

In our case, `observe(<CurrencySource />)` will return the data from our source,
or `undefined` if no data has been fetched yet.

If `observe` returned data, `describe` will return an Phrase Element. If it didn't,
`describe` will return nothing, so the user will not be able to use this command.

The returned Phrase Element describes a [Elliptical grammar](https://elliptical.laconalabs.com/).
In particular, this Element allows input in the form

```
("convert" | "calculate" | "compute") <Decimal> <Currency> [to <Currency>]
```

There is a lot of complexity at work in the grammar, and you can learn more
by reading the [Elliptical Documentation](https://elliptical.laconalabs.com/).

#### `execute`

`execute` gets called when the user presses Return on a command. Commands
can only be executed when their `placeholder`s have been entirely filled in.
This function should *do something* in response to the user's input.

`execute` gets passed `results` as its first argument. This is the same `results`
object that comes from the [Elliptical parse](https://elliptical.laconalabs.com/doc/basics/grammars.html).

It also gets passed the `observe` function and the `config` variable.
We use `observe` to get the current conversion rates (in the same way that `describe` does),
and we use `config.currencyConversion.defaultCurrency` to get the default
currency if the user has not specified a "to" currency.

Then, we set the result to the clipboard so that the user can paste it as they
see fit.

#### `preview`

`preview` determines the output that is displayed in the popover window
to the side of selected results. If `preview` returns a value, it will be
displayed in the popover window.

Like `execute`, `preview` can only be called once a Command's `placeholder`s
have been entirely filled in.

Our `preview` function returns an object that looks something like this:

```js
{type: 'html', value: '30 USD = 199.86 CNY<br />30 USD = 22.46 GBP'}
```

Learn more in [Preview](../advanced/preview.md).

## Exporting the Command

```js
export const extensions = [ConvertCurrency]
```

We export `extensions`, an array of Phrases that have an `extends` property.
This allows Lacona to make use of our `ConvertCurrency` command.
