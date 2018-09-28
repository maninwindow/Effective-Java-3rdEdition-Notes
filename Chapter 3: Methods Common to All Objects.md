
- [Item 10: Obey the general contract when overriding equals](obey-the-general-contract-when-overriding-equals)


## Obey the general contract when overriding equals

> **Each instance of the class is inherently unique.** This is true for classes such as `Thread` that represent active entities rather than values. The `equals` implementation provided by `Object` has exactly the right behavior for these classes.

> **There is no need for the class to provide a "logical equality" test.** For example, `java.util.regex.Pattern` could have overriden `equals` to check whether two `Pattern` instances represented exactly the same regular expression, but the designers didnt' think that clients would need or want this functionality. Under these circumstances, the `equals` implementation inherited from `Object` is ideal.

> **A superclass has already overridden equals, and the superclass behavior is appropriate for this class.** For example, most `Set` implementation inherit their `equals` implementation from `AbstractSet, List` implementation from `AbstractList`, and `Map` implementation from `AbstractMao`.

> **The class is private or package-private, and you are certain that its equals method will never be invoked.** If you are extrmely riskaverse, you can override the `equals` method to ensure that it isn't invoked accidentally.

