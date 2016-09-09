# Qualifiers

Sometimes, the same word can mean multiple things depending on the context. Because Lacona commands are a largely context-free environment, this capability can be handled through Qualifiers.

Imagine the Lacona command "call Vicky" this is a simple command, unless "Vicky" has multiple phone numbers, or I have multiple contacts named "Vicky". This is when you would use qualifiers. If I have contacts "Vicky A" and "Vicky B", who each have mobile and home numbers, Lacona would display:

- call Vicky (A, Mobile)
- call Vicky (A, Home)
- call Vicky (B, Mobile)
- call Vicky (B, Home)

These qualifiers are only used if multiple options exist with the exact same text. If there is only one option, qualifiers will not be displayed.

## An Example

To use this functionality, simply assign the `qualifier` or `qualifiers` prop to any element, or an `item` used by a `<list />`. If multiple qualifiers are assigned together, only the first unique qualifier will be displayed. 

```js
describe () {
  return (
    <sequence>
      <literal text='call ' />
      <choice>
        <literal text='Vicky' qualifiers={['A', 'Mobile']} />
        <literal text='Vicky' qualifiers={['A', 'Home']} />
        <literal text='Vicky' qualifiers={['B', 'Mobile']} />
        <literal text='Vicky' qualifiers={['B', 'Home']} />
      </choice>
    </sequence>
  )
}
```
