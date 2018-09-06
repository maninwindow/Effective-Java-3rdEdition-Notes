- [Consider static factory methods instead of constructors](#consider-static-factory-methods-instead-of-constructors)
- [Item 2: Consider a builder when faced with many constructor parameters](#Consider-a-builder-when-faced-with-many-constructor-parameters)
- [Item 3: Enforce the singleton property with a private constructor or an enum type](#Enforce-the-singleton-property-with-a-private-constructor-or-an-enum-type)
- [Item 4: Enforce noninstantiability with a private constructor](#Enforce-noninstantiability-with-a-private-constructor)
- [Item 5: Prefer dependency injection to hardwiring recourses](#Prefer-dependency-injection-to-hardwiring-recourses)
- [Item 6: Avoid creating unnecessary objects](#Avoid-creating-unnecessary-objects)
- [Item 7: Eliminate obsolete object references](#Eliminate-obsolete-object-references)
- [Item 8: Avoid finalizers and cleaners](#Avoid-finalizers-and-cleaners)
- [Item 9: Prefer try-with-resources to try-finally](#Prefer-try-with-resources-to-try-finally)

# Consider static factory methods instead of constructors

**Advantages:**
- One advantage of static factory method is that, unlike constructors they have names.
- A second advantage of static factory methods is that, unlike constructors, they are not required to create a new object each time they're invoked.
- A third advantage of static factory methods is that, unlike constructors, they can return an object of any subtype of their return type.
- A fourth advantage of static factories is that the class of the returned object can vary from call to call as a function of the input parameters.
- A fifth advantage of static factories is that the class of the returned object need not exist when the class containing the method is written.

**Disadvantages:**
- The main limitation of providing only static factory methods is that classes without public or protected constructors cannot be subclassed.
- A second shortcoming of static factory methods is that they are hard for programmers to find.


# Consider a builder when faced with many constructor parameters

# Enforce the singleton property with a private constructor or an enum type

# Enforce noninstantiability with a private constructor

# Prefer dependency injection to hardwiring recourses

# Avoid creating unnecessary objects

# Eliminate obsolete object references

# Avoid finalizers and cleaners

# Prefer try-with-resources to try-finally
