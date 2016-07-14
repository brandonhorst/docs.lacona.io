# Publishing

Lacona Addons are published and distributed using [npm](https://npmjs.com).

To publish a command first make sure that your `package.json` has the
`lacona-addon` keyword. Lacona will not be able to
find modules lacking this keyword.

Then, simply run

```sh
npm publish
```

If it succeeds, you should be able to see
your package on the [npmjs.com](https://npmjs.com) website.

Within a few minutes, your addons will be added to the full list at
[addons.lacona.io](https://addons.lacona.io) and will be available to all
Lacona users.

## Updating

Likewise, updating Lacona Addons also uses npm. If you have changed your addon,
simply bump the version number in your `package.json` file or by using

```sh
npm version minor|major|patch
```

Then, republish. With a few minutes, you should see the updated version on
[addons.lacona.io](https://addons.lacona.io) and the update will be available
to all Lacona users.

## Metadata

To modify the metadata that is displayed to users, see
[Metadata](metadata.md)

## Blacklist

Lacona Labs reserves the right to blacklist packages for any reason. We will
get in touch if your package was blacklisted, and let you know why. If you
believe that there was an error, please contact us at
[app@lacona.io](mailto:app@lacona.io) to make things right.
