# Getting Started

Developing Lacona commands requires some very basic knowledge of the command
line, and some knowledge of Javascript. If you have questions that are not
cleared up by this guide, please submit an issue.

There is a simple command line utility that will help kickstart your
Lacona addon development. To use it, you'll need node.js v4.0.0+ and npm.
You'll only have to do this once.

If you don't already have node and npm installed.

```sh
$ brew install node
```

Once `node` is installed, you can use `npm` to install the `lacona-cli` package.

```sh
$ npm install -g lacona-cli
```

**Note: This tool is brand new, and not yet perfected. If you find bugs, please [report them](https://github.com/brandonhorst/lacona-cli/issues)**

Lacona addons must be open source. You'll probably want to store your code
on [Github](https://github.com). Go to Github and create a new repo, then
clone it to your local system.

```sh
$ git clone https://github.com/brandonhorst/lacona-my-first-command.git
$ cd lacona-my-first-command
```

If that all worked, you're ready to create your new Lacona addon!

```sh
$ lacona init
```

This will ask you a few questions and automatically generate
the boilerplate of your project. The questions are explained below:

### Basic Information

`lacona init` will prompt you for an "Addon Title", a "Brief Description,
and a "Package Name". The Title and Description can be arbitrary strings -
they will be used to display Addon information to users.

The Package Name is the name to use to upload the package to
[npm](http://npmjs.com), so it must follow
[npm package name rules](https://docs.npmjs.com/files/package.json#name).
Try to stick to lowercase characters, underscores, dashes, and dots.
By convention, the package name should start with `lacona-`, though this is
not enforced.

`lacona init` will also prompt you for the URL of a Git repository and
a license. Your addon will get rejected if your code is unlicensed or
not publicly available.

### Commands vs Extensions

`lacona init` will ask you if you want to "create a new command" or
"extend existing commands".

Commands are simple - they are sentences with a verb that do something.
"open google.com", "shutdown", and "remind me to feed the dog tomorrow" are
all commands.

Extensions are trickier, but very powerful. Extensions allow you add
functionality to existing commands by extending individual phrases. For example,
If you create a phrase "my homepage" that extends the basic `URL` phrase, then
any command that uses the `URL` phrase can now accept "my homepage". You could
then use "open my homepage" or "translate my homepage to Chinese".

Read more at [commands](commands.md) and [extensions](extensions.md).

### User Preferences

`lacona init` will ask you if your addon needs user preferences.

User preferences are specified in a JSON-based schema language called
[confine](https://github.com/brandonhorst/confine). It's really simple, don't
be scared.

Preferences are presented to the user through the standard Lacona Preferences
page, and exposed to the Javascript code as an object.

Read more at [config](config.md).

### Transpilation

`lacona init` will ask you if you want to use transpilation in your addon.
Transpilation is highly recommended - it will allow you to use a nice syntax
for your code, separate your code into modules, and make use of external
npm modules.

You should always use transpilation unless you have a really good reason not to.

Read more at [transpilation](transpilation.md).

### Initialization

Now that you understand what the questions mean, work your way through
`lacona init`. Give your addon a name and a description, and accept
the defaults. This will create a Command with User Preferences that uses
Transpilation.

```sh
$ lacona init
? Addon Title [for humans]: My First Command
? Package Name [for computers]: lacona-my-first-command
? Brief Description: Just a basic introductory command
? Type: command
? Include User Preferences? Yes
? Use Transpilation? [recommended, required to use npm packages] Yes
? git repository: https://github.com/brandonhorst/lacona-my-first-command.git
? license: MIT
? Look good? Yes
```

`lacona init` will generate a directory structure that
looks something like this:

```
.
├── package.json
├── config.json
└── src
    └── extensions.jsx
```

- `package.json` provides information about your package to `npm`. It defines
build scripts, dependencies, version information, documentation links, and more.
Additionally, an additional property is defined - `lacona`. This property is
only used by Lacona, and it provides additional information about the addon.
- `config.json` defines any settings to display in User Preferences.
  If you told `npm init` that you do not need user settings, this
  would only contain an empty object.
- `src` contains the source files which you will be editing.
  - `extensions.jsx` is the javascript code of your addon. It should export
    an Array of Phrases called `default`.

Once you install your packages, two new directories will be automatically
created. You won't need to worry about these, but just know that they exist.

- `build` contains the transpiled and browserified files. It should not be
  edited by hand.
- `node_modules` contains the dependencies required to run and develop your
addon. You should not need to modify this manually.

### Installation

The default files generated are very simple but fully functional, so let's
install them and try them out right away. You can do this with

```sh
$ lacona install
```

This will automatically install necessary dependencies, build the project,
install it into the Lacona Addons directory, and tell Lacona to reload addons.
Your addon should be usable immediately.

### Usage

Call up Lacona and type `test my new command`.
You should see a new option, which would look like this:

If you select it, it should show an alert message.

If you modify the new User Preferences item, you should see the Alert message
change.

### Debugging

As you are doing development, errors happen. Sometimes, you just want to add
a few `console.log` statements for debugging purposes. Because these commands
are run through Lacona, you will not see log statements printed directly
to the console. In order to view logs, just run

```sh
$ lacona logs
```

Our command has a `console.log` statement, so if you run `test my new command`
and then `lacona logs`, you should the log printed to the console.

This will show all Lacona-related logs from the system, so there may be
logs that you don't understand - that's OK.

### Go Forth

You now have a Lacona addon up and running. You can modify the `describe` and
`execute` methods to make it do anything you want. For more information,
you'll want to see the docs for [phrases](phrases.md),
[grammar](grammar.md), and [api](api.md).

Go build something awesome!
