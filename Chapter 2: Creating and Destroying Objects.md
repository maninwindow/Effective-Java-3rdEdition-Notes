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

> In summary, static factory methods and public constructors both have their uses, and it pays to understand their relative merits. Often static factories are preferable, so avoid the reflex no provide public constructors without first considering static factories.


## Consider a builder when faced with many constructor parameters

   Static factories and constructors share a limitation they do not scale wel to large numbers of optional parameters. Consider the case of a class representing the Nutrition Facts label that appears on packaged foods. These labels have a few required fields -- serving size, servings per container, and calorise per serving -- and more than twenty optional fields -- total fat, saturated fat , trans fat, cholesterol, sodium, and so on. Most products have nonzero values for only a few of these optional fields.

**Telescoping constructor pattern**

```java
// Telescoping constructor pattern - does not scale well! - Pages 11-12
package org.effectivejava.examples.chapter02.item02.telescopingconstructor;

public class NutritionFacts {
	private final int servingSize; // (mL) required
	private final int servings; // (per container) required
	private final int calories; // optional
	private final int fat; // (g) optional
	private final int sodium; // (mg) optional
	private final int carbohydrate; // (g) optional

	public NutritionFacts(int servingSize, int servings) {
		this(servingSize, servings, 0);
	}

	public NutritionFacts(int servingSize, int servings, int calories) {
		this(servingSize, servings, calories, 0);
	}

	public NutritionFacts(int servingSize, int servings, int calories, int fat) {
		this(servingSize, servings, calories, fat, 0);
	}

	public NutritionFacts(int servingSize, int servings, int calories, int fat,
			int sodium) {
		this(servingSize, servings, calories, fat, sodium, 0);
	}

	public NutritionFacts(int servingSize, int servings, int calories, int fat,
			int sodium, int carbohydrate) {
		this.servingSize = servingSize;
		this.servings = servings;
		this.calories = calories;
		this.fat = fat;
		this.sodium = sodium;
		this.carbohydrate = carbohydrate;
	}

	public static void main(String[] args) {
		NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 0, 35, 27);
	}
}
```
   Typically this constructor invocation will require many parameters that you don't want to set, but you're forced to pass a value for them anyway. In this case, we passed a value of 0 for fat. With "only" six parameters this may not seem so bad, but it quickly gets out of hand as the number of parameters increases.

   In short, the telescoping constructor pattern works, but it is hard to write client code when there are many parameters, and harder still to read it.

**JavaBeans Pattern**

```java
// JavaBeans Pattern - allows inconsistency, mandates mutability - Pages 12-13
package org.effectivejava.examples.chapter02.item02.javabeans;

public class NutritionFacts {
	// Parameters initialized to default values (if any)
	private int servingSize = -1; // Required; no default value
	private int servings = -1; // "     " "      "
	private int calories = 0;
	private int fat = 0;
	private int sodium = 0;
	private int carbohydrate = 0;

	public NutritionFacts() {
	}

	// Setters
	public void setServingSize(int val) {
		servingSize = val;
	}

	public void setServings(int val) {
		servings = val;
	}

	public void setCalories(int val) {
		calories = val;
	}

	public void setFat(int val) {
		fat = val;
	}

	public void setSodium(int val) {
		sodium = val;
	}

	public void setCarbohydrate(int val) {
		carbohydrate = val;
	}

	public static void main(String[] args) {
		NutritionFacts cocaCola = new NutritionFacts();
		cocaCola.setServingSize(240);
		cocaCola.setServings(8);
		cocaCola.setCalories(100);
		cocaCola.setSodium(35);
		cocaCola.setCarbohydrate(27);
	}
}
```

   Unfortunately, the JavaBeans pattern has serious disadvantage of its own Because construction is split across multiple calls, **a JavaBean may be in an inconsistent state partway throuhg its construction.** The class does not have the option of enforcing consistency merely by checking the validity of the constructor parameters. Attempting to use an object when it's in an inconsistent state may cause failures that are far removed from the code containing the bug and hence difficult to debug. A related disadvantage is that **the JavaBean pattern precludes the passibility of making a class immutable** and requires added effort on the part of the programmer to ensure thread safety.

**Builder pattern**

```java
// Builder Pattern - Pages 14-15
package org.effectivejava.examples.chapter02.item02.builder;

public class NutritionFacts {
	private final int servingSize;
	private final int servings;
	private final int calories;
	private final int fat;
	private final int sodium;
	private final int carbohydrate;

	public static class Builder {
		// Required parameters
		private final int servingSize;
		private final int servings;

		// Optional parameters - initialized to default values
		private int calories = 0;
		private int fat = 0;
		private int carbohydrate = 0;
		private int sodium = 0;

		public Builder(int servingSize, int servings) {
			this.servingSize = servingSize;
			this.servings = servings;
		}

		public Builder calories(int val) {
			calories = val;
			return this;
		}

		public Builder fat(int val) {
			fat = val;
			return this;
		}

		public Builder carbohydrate(int val) {
			carbohydrate = val;
			return this;
		}

		public Builder sodium(int val) {
			sodium = val;
			return this;
		}

		public NutritionFacts build() {
			return new NutritionFacts(this);
		}
	}

	private NutritionFacts(Builder builder) {
		servingSize = builder.servingSize;
		servings = builder.servings;
		calories = builder.calories;
		fat = builder.fat;
		sodium = builder.sodium;
		carbohydrate = builder.carbohydrate;
	}

	public static void main(String[] args) {
		NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
				.calories(100).sodium(35).carbohydrate(27).build();
	}
}
```

   This client code is easy to write and , more importantly, easy to read. **THe Builder pattern simulates named optional parameters** as found in Python and Scala.

   **The builder pattern is well suited to class hierarchies.** Use a parallel hierarchy of builders, each nested in the corresponding class. Abstract classes have abstract builders; concrete classes have concrete builders. For example, consider an abstract class at the root of a hierarchy representing various kinds of pizza.
   
> In summary, **the Builder pattern is a good choice when designing classes whose constructors or static factories would have more than a handful of parameters,** espacially if many of the parameters are optional or of identical type. Client code is much easier to read and write with builders than with telescoping constrcutors, and builders are much safer than JavaBeans.

## Enforce the singleton property with a private constructor or an enum type

   A singleton is simply a class that is instantiated exactly once. Singletons typically represent either a stateless object such as a function or a system component that is intrinsically unique. **Making a class a singleton can make it difficult to test its clients** because it's impossible to sustitute a mock implementation for a singleton unless it implements an interface that serves as its type.
   
```java
// Singleton with static factory - Page 17
package org.effectivejava.examples.chapter02.item03.method;

public class Elvis {
	private static final Elvis INSTANCE = new Elvis();

	private Elvis() {
	}

	public static Elvis getInstance() {
		return INSTANCE;
	}

	public void leaveTheBuilding() {
		System.out.println("Whoa baby, I'm outta here!");
	}

	// This code would normally appear outside the class!
	public static void main(String[] args) {
		Elvis elvis = Elvis.getInstance();
		elvis.leaveTheBuilding();
	}
}
```

> The main advantage of the public field approach is that the API makes it clear that the class is a singleton: the public static field is final, so it will always contain the same object reference. The second advantage is that it's simpler.**A single-element enum type is often the best way to implement a singleton**

## Enforce noninstantiability with a private constructor

```java
// Noninstantiable utility class
package org.effectivejava.examples.chapter02.item04;

public class UtilityClass {
	// Suppress default constructor for noninstantiability
	private UtilityClass() {
		throw new AssertionError();
	}
}
```

> Attempting to enforce noninstantiability by making a class abstract does not work. The class can be subclassed and the subclass instantiated. Furthermore, it misleads the user into thinking the class was designed for inheritance. There is, however, a simple idiom to ensure noninstantiability. A default constructor is generated only if a class contains no explicit constructors, so **a class can be made noninstantiable by including a private constructor.**

## Prefer dependency injection to hardwiring recourses

**Basic dependency injection is explained with desired example code in page 40.**

> In summary, do not use a singleton or static unility class to implement a class that depends on one or more underlying resources whose behavior affects that of the class, and do not have the class create these resources directly. Instead, pass the resources, or factories to create them, into the constructor (or static factory or builder). This practice, known as dependency injection, will greatly enhance the flexibility, reusability, and testability of a class.

## Avoid creating unnecessary objects

## Eliminate obsolete object references

## Avoid finalizers and cleaners

## Prefer try-with-resources to try-finally
