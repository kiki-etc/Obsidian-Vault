At its core, ***Generics*** means ***parameterized*** types. Parameterized types enable the creation of classes, interfaces and methods in which the type of data upon which they operate is specified as a parameter.
<br>A class, interface, or method that operates on a parameterized type is called **generic**, as in *generic class* or *generic method*.

>[!question]- Why the name generic?
>Generic classes are templates suitable for any parameter type

```Java
// a non-generic ArrayList (Java 1.4)
class ArrayList {
	add (Object o);
	Object get(int index);
}

// a generic ArrayList (from Java 1.5)
class ArrayList<T> {
	add (T o);
	T get(int index);
}

// as per runtime, requirement T is replaced with provided type
ArrayList<String> l = new ArrayList<String>();
// the statement is interpreted as followed, based on the above code
class ArrayList<String> {
	add (String o);
	String get(Int index);
}
```

>[!note] The Need for Generics
>Generics are introduced to:
>- provide type safety
>- resolve type-casting problems
>
>Code reusability: helps in designing classes, methods and interfaces suitable to several data types
>The key benefit of generics is enabling errors to be detected at compile time rather than runtime. 

## Basic Rules
<br>As a _base type_ a parent reference (superclass) can be used to hold an object of its child class, but not for parameter types.

```Java
ResearchMethods.presents(Nyameye + "Kiki" + Akumia)
```
<br>The _parameter types_ should be reference types, **not** primitive types.

```Java

```
<br>Creating and testing a generic class:

```Java

```

