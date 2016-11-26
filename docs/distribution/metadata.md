# Metadata

Lacona collects various pieces of information from your `package.json`
file to display to users. They are:

- Name: `name`
  - This is the module name published to npm. It should start with `lacona-`, but this
    is not enforced. It must follow
    [npm naming rules](https://docs.npmjs.com/files/package.json#name).
- Version: `version`
  - The npm version (using [Semantic Versioning](http://semver.org/))
- Icon: `lacona.icon` or `icon.png`
  - An path to an icon representing the Addon. If the Addon provides an integration with
    a well-known service, then using that service's icon is usually sufficient.
    Icons should be square.
  - This path will be evaluated relative to the `package.json` file.
- IconURL: `lacona.iconURL`
  - A remote path to the addon icon. This is used by the Addons page
    when displaying the list of addons, as downloading every icon to extract
    the icon file would be prohibitively expensive.
- Examples: `lacona.examples`
  - An Array of strings representing some common use cases for this Addon. If your
    Addon is an [Extensions](../basics/extensions.md) (rather than a
    [Command](../basics/commands.md)), these should show the behavior of
    your Extension with the context of a built-in command, if possible.
  - Examples do not support arbitrary HTML, but they do support some very simple
    markdown-esque functionality, which is used to display the examples with
    proper Lacona formatting and colorization.
      - Use `[user input](argument name)` to reference colorized arguments
      - Use `[](placeholder name)` to reference placeholders
      - Use `![](http://test.com/image.jpeg)` to reference image annotations
      - Use `*symbolic text*` or `_symbolic text_` to reference italized symbols
      - `open ![](img/clipboard.png)[*clipboard*](clipboard url) in [](application)`
        results in ![open clipboard in application](../img/example.png)
- Title: `lacona.title`
  - The human-readable name of the Addon. In most cases, this should **not**
    contain the word "Lacona". It should be short, but give a solid clue about
    what the Addon does. If `lacona.title` is not provided, `name` is used.
- Description: `lacona.description` OR `description`
  - A human-readable description, explaining what the addon
    does and how to use it. There is no start with the phrase
    "A Lacona Addon to...".
- Homepage: `lacona.homepage` OR `homepage`
  - A website where users can find more information about an addon. If a homepage
    is not provided, users will be linked to Addon's page on NPM.
- Author: `lacona.author` OR `author`
  - A name, plus an optional email address and website of the addon author,
    for contact purposes. This is always public. In
    [npm person format](https://docs.npmjs.com/files/package.json#people-fields-author-contributors).
    If an author is not provided, it will be automatically added with the
    publisher's NPM information.
