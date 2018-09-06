#Contents

- [Item 1: Consider static factory methods instead of constructors](#consider-static-factory-methods-instead-of-constructors)
- [Item 2: Consider a builder when faced with many constructor parameters](#consider-a-builder-when-faced-with-many-constructor-parameters)
- [Item 3: Enforce the singleton property with a private constructor or an enum type](#enforce-the-singleton-property-with-a-private-constructor-or-an-enum-type)
- [Item 4: Enforce noninstantiability with a private constructor](#enforce-noninstantiability-with-a-private-constructor)
- [Item 5: Prefer dependency injection to hardwiring recourses](#prefer-dependency-injection-to-hardwiring-recourses)
- [Item 6: Avoid creating unnecessary objects](#avoid-creating-unnecessary-objects)
- [Item 7: Eliminate obsolete object references](#eliminate-obsolete-object-references)
- [Item 8: Avoid finalizers and cleaners](#avoid-finalizers-and-cleaners)
- [Item 9: Prefer try-with-resources to try-finally](#prefer-try-with-resources-to-try-finally)

## Consider static factory methods instead of constructors

**Advantages:**
- One advantage of static factory method is that, unlike constructors they have names.
- A second advantage of static factory methods is that, unlike constructors, they are not required to create a new object each time they're invoked.
- A third advantage of static factory methods is that, unlike constructors, they can return an object of any subtype of their return type.
- A fourth advantage of static factories is that the class of the returned object can vary from call to call as a function of the input parameters.
- A fifth advantage of static factories is that the class of the returned object need not exist when the class containing the method is written.

**Disadvantages:**
- The main limitation of providing only static factory methods is that classes without public or protected constructors cannot be subclassed.
- A second shortcoming of static factory methods is that they are hard for programmers to find.

**Some common names for static factory methods:**

**from();**

A type-conversion method that takes a single parameter and returns a corresponding instance of this type, for example:

```java
Date d = Date.from(instance);
```

**of();**

An aggregation method that takes multiple parameters and returns and instance of this type that incorporates them, for example:

```java
Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
```

**valueOf();**

A more verbose alternative to from and of, for example:

```java
BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
```

**instance();** or **getInstance();**

Returns an instance that is described by its parameters(if any) but cannot be said to have the same value, for example:

```java
StackWalker luke = StackWalker.getInstance(options);
```

**create();** or **newInstance();**

like instance or getInstance , except that the method guarantees that each call returns a new instance , for example:

```java
Object newArray = Array.newInstance(classObject, arrayLen);
```

**getType();**

Like getInstance, but used if the factory method is in a different class. Type is the type of object returned by the factory method for example:

```java
FileStore fileStore = Files.getFileStore(path);
```

**newType();**

Like newInstance, but used if the factory method is in a different class. Type is the type of object returned by the factory method, for example:

```java
BufferedReader bufferedReader = Files.newBufferedReader(path);
```

**type();**

A concise alternative to getType and newType, for example:

```java
List<Complaint> litany = Collections.list(legacyLitany);
```

```
In summary, static factory methods and public constructors both have their uses, and it pays to understand their relative merits. Often static factories are preferable, so avoid the reflex no provide public constructors without first considering static factories.
```

## Consider a builder when faced with many constructor parameters

## Enforce the singleton property with a private constructor or an enum type

## Enforce noninstantiability with a private constructor

## Prefer dependency injection to hardwiring recourses

## Avoid creating unnecessary objects

## Eliminate obsolete object references

## Avoid finalizers and cleaners

## Prefer try-with-resources to try-finally
