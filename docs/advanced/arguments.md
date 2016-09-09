# Placeholders and Arguments

## Arguments

Arguments are the fundamental grouping of Lacona commands. They allow users to understand how their commands are being understood and what each input represents. In Lacona, words are colored and labeled according to their arguments. Every component of a command except for the verb and any necessary "grammar" words should have an argument.

In a Lacona grammar, setting an argument is as easy as setting the `argument` or `arguments` property on any Element. `arguments` can also be set on the `items` of a `<list />`. However, most often, you will only be using `argument` on a `<placeholder />`.

## Placeholders

When a user is in the process of typing a sentence, there are a million directions that the sentence could go. This means that enumerating all possible options for a sentence gets exponentially harder as the sentence gets longer. Humans can't process millions of options at once. The user is only concerned with "what is the next choice I have to make" and only needs a vague idea of sentence options beyond that.

In Lacona, this is handled through the `<placeholder />` element.

When a grammar contains a `<placeholder />`, that instructs Lacona to stop presenting options and only show some placeholder text until the input reaches the placeholder.

If a `placeholder` has an `argument`, that argument will be displayed as the placeholder text and applied to the placeholder itself. If you need the placeholder text to be different from the argument name, or you wish to have a `placeholder` that does not set an argument, you may use the placeholder's `label` prop instead.