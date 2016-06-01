# Atlas - or, what the hell are all these Github Repos?

Lacona is built very modularly - perhaps to a fault. This document describes
what the different components do, what is proprietary and what is open source,
and where to go to find code.

## 5000 Foot View

Lacona is a proprietary OSX Application developed by
[Lacona Labs](https://laconalabs.com), but built on much open source technology.

At the moment, Lacona Labs is an solo effort by Brandon Horst
([Twitter](https://twitter.com/brandonhorst),
[GitHub](https://github.com/brandonhorst)). If you want to get involved
more than you want to work a job that pays money,
[contact me](docs/appendices/contact.md).

Lacona is composed of 7 major parts:

1. The Lacona View, which collects user input and displays output to the user
4. The Lacona Settings Page, which allows users to customize settings,
  and passes those values to the individual Commands and Extensions
2. The Lacona Commands and Extensions, which tell Lacona how to understand
  language and what to do with it. This includes both the built-in commands
  and extensions and User Addons
3. The Lacona Parser, which analyzes input from the Lacona View according to the
  schemas specified by the Commands and Extensions
5. The Lacona API, which allows the Commands and Extensions, which are
  implemented in Javascript, to interact with the Operating System
6. The OSX App, which ties all of the first 5 components together
7. The `lacona-cli` development tool, which is distributed independently on
  npm

### The LaconaApp Repo

The [laconalabs/LaconaApp](https://github.com/laconalabs/LaconaApp)
repo does not contain any code. It is only used to host the
[Changelog](https://github.com/laconalabs/LaconaApp/blob/master/CHANGELOG.md),
to track [Issues](https://github.com/laconalabs/LaconaApp/issues), and to
serve as a home for the [Gitter Chat](https://gitter.im/laconalabs/LaconaApp).

### The Lacona View

The Lacona View is actually a webpage, with lots of transparency and no border.
The page itself is implemented using [React](https://facebook.github.io/react/).
The page source is proprietary, but it is built largely on top of
[react-lacona](https://github.com/laconalabs/react-lacona), which is OSS.
react-lacona can be forked to build similar interfaces for other applications
using [Elliptical](https://github.com/laconalabs/elliptical) technology.

### The Lacona Settings

The Lacona Settings is a proprietary webpage, also implemented using
[React](https://facebook.github.io/react/). However, the functionality
is built on another LaconaLabs-developed OSS technology called
[Confine](https://github.com/laconalabs/confine). It normalizes settings
based upon the the JSON-based
schemas that the Commands and Extensions provide, so that they can
rely on the settings having a particular format.

The advantage of using a JSON schema is that a user interface can be developed
automatically to handle it. This is done with
[react-confine](https://github.com/laconalabs/react-confine).

Lacona also uses
[confine-key](https://github.com/laconalabs/confine-key/settings) and
[react-confine-key](https://github.com/laconalabs/react-confine-key)
to handle Keyboard Shortcut functionality.

### The Lacona Parser + Commands and Extensions

The most important part of the app is the parser - the brain that actually
parses the input according to the dynamic schemas. The actual parser logic
is proprietary, but it is composed almost entirely of OSS; namely,
Elliptical and the built-in Commands and Extensions.

#### Elliptical

The parser is built on top
of an Open Source Interactive Language Parsing library called
[Elliptical](https://elliptical.laconalabs.com).

Elliptical was developed by Lacona Labs in tandem with Lacona, but it is
completely independent. It is pure Javascript, and has no ties to OSX or
desktop productivity. Other companies are using the technology for other
purposes (eg. [HealthReactor](http://healthreactor.io/)).

- [GitHub: laconalabs/elliptical](https://github.com/laconalabs/elliptical)
- [Docs](https://elliptical.laconalabs.com)

Elliptical is a very flexible framework, and its functionality can be
extended using `processors`. Lacona makes use of 4 of these processors:

- [elliptical/observe](https://github.com/laconalabs/elliptical-observe),
  which provides Observable-based sideways data loading. This is how Lacona
  Sources are built.
- [elliptical/extend](https://github.com/laconalabs/elliptical-extend),
  which provides Linguistic Extension. This is how Lacona Extensions work.
- [elliptical/translate](https://github.com/laconalabs/elliptical-translate),
  which provides Internationalization. This allows Lacona Commands and
  Extensions to be used in multiple languages.
- [elliptical/wormhole](https://github.com/laconalabs/elliptical-wormhole),
  which provides "wormhole" data to phrases, that do not need to be passed
  down the tree. This is how phrases get access to Lacona Config objects.

Lacona also makes use of a number of phrases that have been built in
Elliptical.

- [elliptical-number](https://github.com/laconalabs/elliptical-number), which
  is exposed in `lacona-phrases` as `Integer`, `Decimal`, `DigitString`, and
  `Ordinal`
- [elliptical-string](https://github.com/laconalabs/elliptical-string), which
  is exposed in `lacona-phrases` as `String`
- [elliptical-email](https://github.com/laconalabs/elliptical-email), which
  is exposed in `lacona-phrases` as `EmailAddress`
- [elliptical-phone](https://github.com/laconalabs/elliptical-phone), which
  is exposed in `lacona-phrases` as `PhoneNumber`
- [elliptical-url](https://github.com/laconalabs/elliptical-url), which
  is exposed in `lacona-phrases` as `URL`
- [elliptical-datetime](https://github.com/laconalabs/elliptical-datetime),
  which is exposed in `lacona-phrases` as `Date`, `Time`, `DateTime`,
  `Range`, `Day`, `DateDuration`, `TimeDuration`, and `Duration`

#### Commands and Extensions

The Lacona App has 8 built-in "Addons". That's an oxymoron, but they are
packages built using exactly the same technology as custom user Addons - just
they are bundled with the system.

- [lacona-communicate](https://github.com/laconalabs/lacona-communicate),
  which provides `call`, `text`, `email`, and similar commands
- [lacona-command](https://github.com/laconalabs/lacona-command),
  which defines what to do with `BooleanSetting`s
- [lacona-finder](https://github.com/laconalabs/lacona-finder),
  which provides `open`, `switch to`, `quit`, `eject`, and similar commands
- [lacona-events](https://github.com/laconalabs/lacona-events),
  which provides `schedule` and `create reminder`
- [lacona-itunes](https://github.com/laconalabs/lacona-itunes),
  which provides `play`, `pause`, `stop`, and similar commands
- [lacona-osx](https://github.com/laconalabs/lacona-osx),
  which extends basic `lacona-phrases` with OSX-specific functionality,
  mostly gathered from OSX technologies like Spotlight and Applescript. It also
  provides `shutdown`, `restart`, `turn on screensaver`, and similar commands
- [lacona-search-internet](https://github.com/laconalabs/lacona-search-internet),
  which provides `search`
- [lacona-settings](https://github.com/laconalabs/lacona-settings),
  which provides `turn off`, `turn on`, and `toggle`
- [lacona-translate](https://github.com/laconalabs/lacona-translate),
  which provides `translate`

#### Misc

The parser also makes use of

- [lacona-utils](https://github.com/laconalabs/lacona-utils)

### The Lacona API

Integral to Lacona is the API which exposes OS functionality to the
JavaScript commands. This functionality lives in
[lacona-api](https://github.com/laconalabs/lacona-api). Unfortunately,
this module is not self-sufficient - it must be executed in the global context
of the Lacona Parser, as the logic is actually implemented in (proprietary)
Objective-C code. It is *not* a general-purpose Node library.

Most commands interact with one another by using Phrases. Anybody can create
new Phrases, but Lacona has a bunch built-in. These phrase live in
[lacona-phrases](https://github.com/laconalabs/lacona-phrases). Some phrases
are simple wrappers around Elliptical phrases, while others have no
implementation at all and must be extended (for example, by
[lacona-osx](https://github.com/laconalabs/lacona-osx)).

In addition to the proprietary Objective-C integrations, Lacona also ships with
a complete copy of the [Node.js](https://nodejs.org/) binary, for use with
`lacona-api#callNode`.

### The `lacona-cli` Module

The [lacona-cli](https://github.com/laconalabs/lacona-cli) module is entirely
OSS, and is distributed independently of Lacona on [npm](http://npmjs.com/).

It is only used for developing new Lacona Addons, and is not required for
normal application use.

### The Lacona Website

The [Lacona Website](https://lacona.io) has a full copy of Lacona running on it.
The `lacona-api` functionality is stubbed out, but the rest of the code
is the full Lacona Commands and Extensions, Elliptical, react-lacona, and
all the rest. Isomorphic JavaScript is pretty cool, huh? It lives at
[laconalabs/www.lacona.io](https://github.com/laconalabs/www.lacona.io).

This [GitBook](https://www.gitbook.com/book/laconalabs/lacona/details)
docs page lives at
[laconalabs/docs.lacona.io](https://github.com/laconalabs/docs.lacona.io).
