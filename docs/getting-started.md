# Getting Started

## Setup

Developing Lacona commands requires some very basic knowledge of the command
line, and some knowledge of Javascript. If you have questions that are not
cleared up by this guide, please submit an issue.

There is a simple command line utility that will help kickstart your
Lacona addon development. To install it, you'll need node.js v4.0.0+ and npm.

```sh
$ brew install node
```

Once `node` is installed, you can use `npm` to install the `lacona-cli` package.

```sh
$ npm install -g lacona-cli
```

If that all worked, create a new directory for your new Lacona Addon, and
run `lacona init`.

```sh
$ mkdir my-new-addon
$ cd my-new-addon
$ lacona init
```

This will ask you a few questions. Then, it will automatically generate
the boilerplate of your project and install the necessary dependencies.

To ahead and tell `lacona init` that you'd like your addon to
"extend existing commands" and that you do want it to have user preferences.

## Aside: Commands vs Extensions

`lacona init` will ask you if you want to "create a new command" or
"extend existing commands". This is one of the most interesting features
of Lacona - Addons can add new functionality to existing commands by
extending `phrases`. We'll try to build an Extension first, and then
we'll try a Command a bit later.

## Your First Addon

`lacona init` will generate 
a directory structure that looks something like this:

```
- package.json
- src
-- addon.js
-- config.json
- node_modules
```

- `package.json` provides information about your package to `npm`. It defines
build scripts, dependencies, version information, documentation links, and more.
Additionally, an additional property is defined - `lacona`. This property is
only used by Lacona, and it provides additional information about the addon.
- `src` contains the source files which you will be editing.
  - `addon.js` is the javascript code of your addon. It should export
    an Array called `extensions`.
  - `config.json` defines any settings to display in User Preferences.
    If you told `npm init` that you do not need user settings, this
    will only contain an empty object.
- `node_modules` contains the dependencies required to run and develop your
addon. You should not need to modify this manually.

The default files generated are very simple but fully functional, so let's
install them and try them out right away. You can do this with

```sh
$ lacona install
```

This will automatically build the project and install it into the Lacona Addons
directory. If the Lacona App is running, it should be usable immediately.
Call up Lacona and type `open my homepage`.
You should see a new option, which would look like this:

![]()

If you select it, it will open `http://lacona.io` in your browser.

Now, try `translate my homepage to Spanish`. Make sure you select the "homepage"
option rather than the "phrase" option.

![]()

It should open a Google-translated version of `lacona.io`.

What happened here? We did not modify the `open` or `translate` commands.
Rather, we **extended* the `URL` phrase. As a result, all commands that make
use of the `URL` phrase can now make use of our `my homepage` extension.

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
