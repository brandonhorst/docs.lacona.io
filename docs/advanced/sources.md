# Sources

Sources provide a way to fetch external information into Lacona phrases,
while still keeping `describe` calls fast and pure. Sources abstract
out complexity and asynchronicity, and automatically ensure that work
isn't duplicated. 

A Source looks much like a Phrase, but rather than a `describe` method,
it has a `fetch` method. This method must return an `Observable`

```js
const MySource = {
  fetch () {
    return watchApplications() // returns an Observable
  }
}
```

## Observables

Observables are an ECMAScript proposal - they may one day be a part
of Javascript proper, but they're not yet. However, the abstraction fits
perfectly with Lacona sources.

An Observable is an object that can be subscribed to. Once it is subscribed to,
it will start passing values to its subscriber. These values can be spread
out over time asyncronously. They are documented
[here](https://github.com/zenparsing/es-observable).

In order to create Observables, you will need to use a module such as
[zen-observable](https://github.com/zenparsing/zen-observable) or
[ReactiveX/rxjs](https://github.com/ReactiveX/RxJS).

If your source returns a single piece of data rather than a stream (such
as a network request), you should make use of a function like
[ReactiveX/rxjs::fromPromise](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/frompromise.md) or
[ReactiveX/rxjs::from](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/of.md).

## Using Sources (`observe`)

Phrases, in addition to `props`, are also passed a function called `observe`.
This function can be called at any time, and passed an `Element`. However,
rather than an `Element` that references a `Phrase`, this `Element` references a
`Source`.

```js
const MySource = {
  fetch () {
    return watchApplications() // returns an Observable
  }
}

const MyPhrase {
  describe ({observe}) {
    const currentApplications = observe(<MySource />)
    return <list items={currentApplications} />
  }
}
```

`observe` syncronously returns whatever the current value of the observable is. 
Moreover, if the Observable's value ever changes, it will automatically reparse
and return the new value. In this way, you can simply call `observe` and
never need to worry about its implementation.

You can also pass props to sources, in much the same way they can be passed
to phrases.

```js
const MySource = {
  fetch ({props}) {
    return watchApplicationsIn(props.directories) // returns an Observable
  }
}

const MyPhrase {
  describe ({observe}) {
    const currentApplications = observe(
      <MySource directories={['/Applications']} />
    )
    return <list items={currentApplications} />
  }
}
```

## Why So Complicated?

Javascript already has lots of great ways to deal with asyncronous functionality,
why does Lacona introduce a new abstraction?

First, it allows us to keep the `describe` method as simple as possible. If the
method is simple and pure, it is easier to understand.

More importantly, sources prevent us from duplicating work. If multiple phrases,
`observe` the same Source with the same props, the Source's `fetch` function
will only be called once, and only one Observable will be created.

This provides a sort of intelligent, general-purpose caching mechanism that is
necessary for Lacona because Phrases are referenced in many different places.
If each instance of a phrase loaded its data independently, system resources
would quickly get overloaded and performance decrease. Using sources keeps things
fast and simple, for only a small bit of additional overhead.

## Examples

Please see [examples](docs/examples.md).