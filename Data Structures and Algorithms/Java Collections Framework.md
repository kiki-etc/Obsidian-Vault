A sophisticated ***hierarchy of interfaces and classes*** that provide state-of-the-art tech for managing ***groups of objects***.
It is a set of classes and interfaces that implement commonly reusable collection ***data structures***.
It resides in the `java.util` package.
<br>Interfaces: These define the types of collections we can work with: `List`, `Set`, `Queue`, `Map`.
<br>Implementations: These are the concrete implementations: `ArrayList`, `LinkedList`, `HashSet`, `TreeSet`, `PriorityQueue`, `HashMap`, etc.
<br>Algorithms: These are the methods that perform useful operations on collections, such as sorting and searching.
# Collection(s) `implements Iterable`
<br>The `Iterable` interface in Java represents a collection of elements that can be iterated over. It provides a way to obtain an iterator, which allows sequential access to the elements in the collection. Any class that implements the Iterable interface can be used in an enhanced for loop (for-each loop).
<br>The `Collection` interface in Java represents a group of objects known as elements. It is the root interface in the Java Collections Framework hierarchy. Collection interfaces extend the Iterable interface, allowing collections to be iterated over using the enhanced for loop.
<br>**Key Characteristics**
- **Size**: Provides methods to get the size of the collection.
- **Addition and Removal**: Provides methods to add and remove elements from the collection.
- **Containment**: Provides methods to check if an element exists in the collection.
- **Iteration**: Supports iteration over the elements in the collection using an iterator.
<br>**Methods**
- `int size()`: Returns the number of elements in the collection.
- `boolean isEmpty()`: Returns true if the collection contains no elements.
- `boolean contains(Object o)`: Returns true if the collection contains the specified element.
- `Iterator<E> iterator()`: Returns an iterator over the elements in the collection.
- `boolean add(E e)`: Adds the specified element to the collection.
- `boolean remove(Object o)`: Removes the specified element from the collection, if it is present.
## ArrayList
<br>The `ArrayList` class extends `AbstractList` and implements the `List` interface.
A non-generic ArrayList can hold any type of object because it stores elements as `Object` references.
```Java
ArrayList list = new ArrayList();
```
<br>A generic ArrayList allows you to specify the type of elements it can contain.
```Java
ArrayList<String> stringList = new ArrayList<>();
```
<br>In both cases, you should ==`import java.util.ArrayList`==. Primitive types cannot be used with generic ArrayLists, unless their upper class types are used.

### ArrayList operations

```Java
// for non-generic ArrayList
list.add("Hello"); // add method adds input to the end of ArrayList
Object element = list.get(0); // retrieve object at index (casting required)
String str = (String) list.get(0); // casting to specific type

// for generic ArrayList
stringList.add("Hello"); // add method
String str = stringList.get(0); // no casting required

// loop
for (String item : stringList) {
	System.out.println(item);
}

// other essential operations
boolean isEmpty = stringList.isEmpty();
stringList.remove("Orange");
stringList.clear();
```
<br>Iterable collections also allow for added functionality using methods from the interfaces `Iterable` and `Iterator`. You should ==`import java.util.Iterator`==
For example, assuming a non-generic ArrayList called `mixedList`, you can loop through the list and print each item as follows:

>[!example]- Example of looping through a non-generic ArrayList
>```Java
>Iterator iterator = mixedList.iterator();
>while (iterator.hasNext()) {
>	Object element = iterator.next();
>	System.out.println(element);

## LinkedList
<br>A non-generic LinkedList can hold any type of object because it stores elements as `Object` references.
```Java
LinkedList list = new LinkedList();
```
<br>A generic LinkedList allows you to specify the type of elements it can contain.
```Java
LinkedList<String> stringList = new LinkedList<>();
```
<br>In both cases, you should ==`import java.util.LinkedList`==. Primitive types cannot be used with generic LinkedLists, unless their upper class types are used.

### LinkedList operations

```Java
// for non-generic LinkedList
list.add("Hello"); // add method adds input to the end of LinkedList
Object element = list.get(0); // retrieve object at index (casting required)
String str = (String) list.get(0); // casting to specific type

// for generic LinkedList
stringList.add("Hello"); // add method
String str = stringList.get(0); // no casting required

// loop
for (String item : stringList) {
	System.out.println(item);
}

// other essential operations
boolean isEmpty = stringList.isEmpty();
stringList.remove("Orange");
stringList.clear();
```
<br>The interfaces `Iterable` and `Iterator` apply here, similar to `ArrayList`.

> [!example]- Example of looping through a non-generic LinkedList
> ```Java
> Iterator iterator = mixedList.iterator();
> while (iterator.hasNext()) {
> 	Object element = iterator.next();
> 	System.out.println(element);
> }
> ```



## HashSet
<br>A HashSet is an implementation of the `Set` interface in Java. It does not allow duplicate elements. Elements are stored using a hash table and their order is not maintained. Because of its implementation, it provides constant-time performance for basic operations like add, remove, contains, and size (assuming the hash function disperses the elements properly among the buckets).

>[!warning] HashSet is not thread-safe
>HashSet is not synchronized, so it's not thread-safe. If multiple threads access a hash set concurrently, and at least one of the threads modifies the set structurally, it must be synchronized externally.

You should ==`import java.util.HashSet`==. Primitive types cannot be used with generic LinkedLists, unless their upper class types are used.

### HashSet Operations

```Java
hashSet.add("Orange"); // add items to HashSet
hashSet.contains("Banana"); // check if element exists
hashSet.remove("Banana"); // remove element
hashSet.clear(); // emptying the set
hashSet.size(); // retrieving the size

// looping through the HashSet
for (String fruit : hashSet) {
	System.out.println(fruit);
}
```
<br>The interfaces `Iterable` and `Iterator` apply here, similar to `ArrayList`.

> [!example]- Example of looping through a generic HashSet
> ```Java
> Iterator iterator = hashSet.iterator();
> while (iterator.hasNext()) {
> 	Object element = iterator.next();
> 	System.out.println(element);
> }
> ```

## TreeSet
<br>TreeSet is an implementation of the `SortedSet` interface in Java. It stores elements in a sorted order (natural ordering or by a comparator provided at construction time). They do not allow duplicate elements. (TreeSet is implemented using a Red-Black tree, which ensures that the elements are always sorted.)

You should ==`import java.util.TreeSet`==. Primitive types cannot be used with generic LinkedLists, unless their upper class types are used.

>[!caution] Remember
>- Elements in a TreeSet must either implement the `Comparable` interface or be provided with a custom comparator during TreeSet creation.
>- Operations like `add`, `remove`, and `contains` have a time complexity of O(log n), where n is the number of elements in the TreeSet.

### TreeSet Operations

```Java
treeSet.add("Orange"); // add items to HashSet
treeSet.contains("Banana"); // check if element exists
treeSet.remove("Banana"); // remove element
treeSet.clear(); // emptying the set
treeSet.size(); // retrieving the size

// looping through the HashSet
for (String fruit : treeSet) {
	System.out.println(fruit);
}
```
<br>The interfaces `Iterable` and `Iterator` apply here, similar to `ArrayList`.

> [!example]- Example of looping through a generic TreeSet
> ```Java
> Iterator iterator = treeSet.iterator();
> while (iterator.hasNext()) {
> 	Object element = iterator.next();
> 	System.out.println(element);
> }
> ```



# Map(s)

# Iterator
<br>The `Iterator` interface in Java provides a way to iterate over a collection of elements. It allows sequential access to the elements of a collection, one at a time. Iterator provides methods to check if there are more elements available, retrieve the next element, and remove elements during iteration.
<br>**Key Characteristics**:
1. **Traversal**: Allows traversal of a collection of elements.
2. **Forward-Only**: Iterators are generally forward-only, meaning they cannot move backward.
3. **Removal**: Supports removal of elements from the underlying collection during iteration.
<br>**Methods**:
- `boolean hasNext()`: Returns true if the iteration has more elements.
- `E next()`: Returns the next element in the iteration.
- `void remove()`: Removes from the underlying collection the last element returned by this iterator (optional operation).