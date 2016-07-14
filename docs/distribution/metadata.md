# Metadata

Lacona collects various pieces of information from your `package.json`
file to display to users. They are:

- Name: `name`
  - This is the module name published to npm. It should start with `lacona-`, but this
    is not enforced. It must follow
    [npm naming rules](https://docs.npmjs.com/files/package.json#name)
- Version: `version`
  - The npm version (using [Semantic Versioning](http://semver.org/))
- Title: `lacona.title` OR `name`
  - The human-readable name of the addon. In most cases, this should **not**
    contain the word "Lacona"
- Description: `lacona.description` OR `description`
  - A human-readable description, explaining what the addon
    does and how to use it. There is no need to say thing like
    "A Lacona Addon to..."
- Homepage: `lacona.homepage` OR `homepage` OR `https://npmjs.com/package/${name}`
  - A website where users can find more information about an addon
- Author: `lacona.author` OR `author` OR `maintainers[0]` (published automatically)
  - A name, plus an optional email address and website of the addon author,
    for contact purposes. This is always public. In
    [npm person format](https://docs.npmjs.com/files/package.json#people-fields-author-contributors).
