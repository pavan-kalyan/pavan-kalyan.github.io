---
layout: post
title:  "Guide to Optionals in Java 8"
excerpt: "This article covers the different use cases of Optionals introduced in Java 8"
date:   2020-08-09 00:00:00 +0530
author: Pavan Kalyan Damalapati
---


### Introduction
NullPointerExceptions are some of the worst kind of issues to experience especially in production. This is a constant pain point of Java Developers (and developers in general). This [article](https://dzone.com/articles/the-worst-mistake-of-computer-science-1) goes into great detail on why null causes problems everywhere.
But the main take aways are:
- Using null values lead to error prone and buggy code.
- Handling null values leads to ugly and unecessarily verbose code.
- When we do get a NullPointerException, it's not always clear, why a particular object is null as the exception is thrown usually when a method is called on it, not when the object itself is created.

This article goes through the different methods available in Optionals.java and how they can be used to mitigate some of the issues associated with nulls.



### What exactly is an Optional?
Simply put it is a container object which tells you whether or not it currently contains a value.



### How to create an Optional object.
There are three main ways to create an Optional object

```Java
//Can be used to indicate a non-value
Optional.empty()
//wraps any non-null object in an optional container
Optional.of()
//wraps any object in an optional container
Optional.ofNullable()
```
Let's see how useful each of these are when dealing with nulls.

#### Use Cases

#### To Catch NullPointerException early

The following code demonstrates that it's difficult to know exactly when an object was made null, since the NullPointerException can happen much later.
```Java
//Address Constructor example
public class Address {
	private ObjectThatCanBeNull objectThatCanBeNull;

	Address(ObjectThatCanBeNull object) {
		this.objectThatCanBeNull = object;
	}
	ObjectThatCanBeNull getObjectThatCouldBeNull() { return ObjectThatCanBeNull; }
}
//possible incorrect construction with a null value for a nested object.
Address address = new Address(getObjectWhichCanBeNull);

// many lines of code later

//The following line throws a NullPointerException, and to debug it, requires a lot of backtracking.
System.out.println(address.getObjectThatCouldBeNull().getValue());
```

This issue can be caught early, if we use Optionals

```Java
public class Address {
	private ObjectThatCanBeNull objectThatCanBeNull;

	Address(ObjectThatCanBeNull object) {
		this.objectThatCanBeNull = object;
	}
	Optional<ObjectThatCanBeNull> getObjectThatCouldBeNull() { return Optional.of(ObjectThatCanBeNull); }
}

// Throws NullPointerException here rather than later and makes it easier to debug
Address address = new Address(getObjectWhichCanBeNull);

// many lines of code later

System.out.println(address.getObjectThatCouldBeNull().getValue());
```

#### To Clearly Indicate That A Function Can Return A Non-value

Sometimes returning a null is a valid return value for a function. The calling function handles a null (absence of value) differently.
**Note**: using null as a valid return value should be avoided if possible.
```Java
Address locateAddress(Input input) {
	if (input.isOfTypeAddress()) {
		return input.getAddress();
	}
	return null;
}

//Somewhere else the function might be called, where the engineer might not be aware that locateAddress() can return null
void newFunction(Input input) {
	// throws a NullPointerException
	locateAddress(input).displayAddress();
}
```

These kinds of situations can be avoided by using Optionals as it makes it explicitly clear that locateAddress() can return an absence of value;

```Java
Optional<Address> locateAddress(Input input) {
	if (input.isOfTypeAddress()) {
		return input.getAddress();
	}
	return Optional.empty();
}

void newFunction(Input input) {
	Optional<Address> optionalAddress = locateAddress(input);

	if (optionalAddress.isPresent()) {
		optionalAddress.get().displayAddress();
	} else {
		// we can either log it as an error or display some actionable error message
		System.out.println("Address does not exist for this user, please contact Support.");
	}
}
```
### Different Methods available in Optionals.java

```Java
// simply returns the object contained in the Optional
T get();

// returns whether or not the Optional contains anything or nothing (equivalent to Optional.ofEmpty())
boolean isPresent();

// do an action if the Optional actually contains an object
void ifPresent(Consumer<? super T> consumer);

// either return the object contained or return other
T orElse(T other);

// either return the object contained or return what the supplier supplies
T orElseGet(Supplier<? extends X> exceptionSupplier);

// either return the object contained or throw an exception
<X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier);

/*
If a value is present, apply the provided mapping function to it,
     * and if the result is non-null, return an describing the
     * result.  Otherwise return Optional.empty().
     */
<U> Optional<U> map(Function<? super T, ? extends U> mapper);

//similar to map but for flattening it
<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper) {
```

#### Use Cases

#### Handle long chain of null calls in a clean way

If there is a chain of calls, of which any can be null, without using optionals it would be quite ugly
```Java
//not null safe
System.println(obj1.getObj2().getObj3().getObj4());

//null safe
if (obj1.getObj2() != null) {
	if (obj1.getObj2().getObj3() != null) {
		System.println(obj1.getObj2().getObj3().getObj4());

	}
}
```

**Note**: Avoid passing Optional objects as a parameter to a function, it pushes the burden of checking to the function.

### References

- [Article regarding the problems with null](https://dzone.com/articles/the-worst-mistake-of-computer-science-1)
- [The answers in this StackOverflow question](https://stackoverflow.com/questions/23454952/uses-for-optional)
