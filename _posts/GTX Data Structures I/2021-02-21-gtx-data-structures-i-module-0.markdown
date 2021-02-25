---
layout: post
title: "Module 0"
date: 2020-01-26
categories: gtx cs1332x1
permalink: /:categories/:date/:title
---

Course URL: [https://app.pluralsight.com/library/courses/writing-clean-code-humans/table-of-contents](https://app.pluralsight.com/library/courses/writing-clean-code-humans/table-of-contents)

---

## SUMMARY

### Module 0: Introduction. Review of important Java principles involved in object-oriented design; Iterator/Iterable design patterns, Comparable/Comparator interfaces, basic “Big-Oh” notation, and asymptotic analysis.

---

## References

- Writing JUnits (https://drive.google.com/file/d/1Pg4Ity9tbClmaGsHJUBErNrnObiOqy9e/view)[https://drive.google.com/file/d/1Pg4Ity9tbClmaGsHJUBErNrnObiOqy9e/view]
- Running JUnits (https://drive.google.com/file/d/15WWI7ioKjWQTKbL0PFgMq46H_rYcTeyU/view)[https://drive.google.com/file/d/15WWI7ioKjWQTKbL0PFgMq46H_rYcTeyU/view]
- Data Structures and Algorithms Visualizations (https://csvistool.com/)[https://csvistool.com/]

---

## Java review

#### Visibility modifiers:

- public
- protected
- package private
- private
  Allow varying levels of access from the following:
- world
- subclasses
- package
- class and inner classes

#### this

this refers to the current instance of the class.
this is used inside methods and constructors of a class.

#### constructor chaining

Constructor chaining is using the "this()" keyword to reference a constructor in the current class.

- Constructor chaining cannot be used to call a constructor in the super class. For that, use the super() keyword.
- Must be the first line in the constructor.
- Can only call once.
- Constructors cannot be chained in a way that would cause a loop. Straight line is best practice.

Constructor examples below:

```java
public class Point {
  private double x;
  private double y;

  // example 1: full constructor
  public Point(double a, double b) {
    x = a;
    y = b;
  }

  // example 2: full constructor
  // use "this" to refer to the current instance, to differentiate multiple variables of the same name
  public Point(double x, double y) {
    this.x = x;
    this.y = y;
  }

  // example 3: constructor chaining
  // refer to an existing constructor with this(), set only one variable -- the rest can be set later
  public Point (double a) {
    this(a, 0);
  }
}

```

### reference vs. value equality

- reference equality:

  - checks if two objects occupy the same memory space.
  - == operator
    - primarily used to check if an object is null

- value equality:

  - checks if two objects are equal based on user's use case and design
  - .equals() method from the Object class
    - defaults to same result as ==
    - primitives do not extend the Object class, so you must use == with them and there is no differentiation between reference and value equality

- Key difference: Reference equality is important to check for uniqueness

#### String literals

- String literals are created by declaring a string in quotes "This is a string".
- In this case, each string is a literal/constant stored in the String pool for faster access.
- Like primitives, string literals have no difference between reference and value equality.
- String literals DO extend Object, so you can check with .equals();
- e.g. String literal = "This is a string."

- Strings can also be created with the new keyword, like other Objects.
- In this case, the string does have a difference between reference and value equality.
- e.g. String object = new String("This is a string.");

- Note that using == will only return true amongst string literals.
- Using .equals() will return true between literals and objects because the function handles the variations.

#### Null checking

- Invoking a method or a field of a null object yields a NullPointerException
  - Passing null as a parameter to the .equals() method will yield a NullPointerException
- This is why == is used for null checking.
  - e.g. nullObject == null; // => true

#### Wrapper objects

- Like String objects, each of the primitive types have a wrapper Object class (i.e. int -> Integer, double -> Double, etc.)
- While primitives are optimized, there are times where you need an Object for the code to work.
- Generic typings implictly require the type to inherit from the Object class, meaning that if you wanted to make some Collection of ints, then you'd need to make Integer objects to add rather than primitives.

#### Pass by value and pass by reference

- Pass by value:
  - The helper method takes in the value of what was passed in. Changing its value will change the value of the local variable, but not the value of the original variable (because they reference two different locations in memory).
  - Java is a pass by value language.
  - The original variable and the parameter variable are unrelated to each other besides the value they store
- Pass by reference:
  - The helper method takes in the reference for what was passed in. Changing its value will change the value of both the local variable and the original variable (because they reference the same location in memory).

```java
// Unlike javascript, java is a pass by reference language
// This doesn't work
public static void main(String[] args) {
    Container count = new Container(0);     // step 1: create new Container named count
    helper(count);                          // step 2: call the helper method on count
    System.out.println(count.getItem());    // step 4: print out the value of count
}

public static void helper(Container x) {
    x = new Container(x.getItem() + 1);     // step 3: create a new object reference with
                                            // its item set to x's item + 1
}


// This does work.
// Note how helper must explicitly return a value that must be set directly to count
public static void main(String[] args) {
    int count = 0;                      // step 1: create a new int named count
    count = helper(count);              // step 2: call helper on count
                                        // step 5: assign the returned value to count (still on the line above, but after the function call)
    System.out.println(count);          // step 6: print the value of count
}

public static int helper(int x) {
    x = x + 1;                          // step 3: increment the value of x
    return x;                           // step 4: return the value of x
}

// To pass by reference an object
// The duplicate count object points to the same object so the value x within both objects can be directly modified
public static void main(String[] args) {
    Container count = new Container(0);     // step 1: create new Container named count
    helper(count);                          // step 2: call helper on count
    System.out.println(count.getItem());    // step 4: print the value of count
}

public static void helper(Container x) {
    x.setItem(x.getItem() + 1);             // step 3: set x's item to x's item + 1, modifying the original object
}

//
```

### Generics

Sample generic class:

```java
// T is the type parameter
public class Container<T> {
    private T t;

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }
}

Container<String> c1 = new Container<String>();
c1.set("hello");
String s = c1.get(); // You do not have to cast the result of get, it must be String

Container<String> c2 = new Container<String>();
c2.set("hello");
Integer i = (Integer)c2.get(); // Attempting to cast get to Integer will result in a compile-time error

```

- Declare generic classes with one (or more) type parameters in angle brackets after the class name
- When we declare instances of generic classes, we use angle brackets to invoke the generic type. When we initialize instances of generic classes, we also use angle brackets:

```java
ArrayList<Integer> listOne = new ArrayList<Integer>();

```

- Java allows us to remove the explicit type argument from the initialization, as the compiler can infer the type argument from the declaration. This can be useful to prevent repetition when writing code. The empty angle brackets are commonly referred to as "the diamond":

ArrayList<Integer> listOne = new ArrayList<Integer>();

Notice the usage of angle brackets on both sides of the equals sign.

```java
ArrayList<String> list = new ArrayList<>();
```

- Note java does not allow us to create an array of generic types

```java
// Error
T[] backingArray = new T[10];

// Create a new array of Objects and then cast to an array of Ts.
T[] backingArray = (T[]) new Object[10];
```

- Multiple type parameters are declared and instantiated similar to classes with single type parameters

```java
class HashMap<K, V> { ... }
HashMap<String, Integer> map = new HashMap<>();

```

- Generic classes as Type Parameters
- You might need to "nest" type parameters when declaring and instantiating collections of generic types:

Note that nodeList is an ArrayList which contains instances of the BSTNode class, which itself uses T as its type parameter.

```java
ArrayList<BSTNode<T>> nodeList = new ArrayList<>();
```

---

## Iterator/Iterable design patterns

#### The Iterable interface

- Handles the task of iterating through data.
- Is an abstraction that allows the implementing class to be iterated through with a for-each loop.
- Has abstract method called Iterator() that returns an Iterator object to handle traversing the data structure.
- Is contained in the java.lang package, does not have to be explicitly imported.

#### Abstract Methods

- To implement the iterator interface, a class must override the next and hasNext methods

```java
// Returns next data in iteration order
public abstract T next();

// Returns whether there is more data
public abstract boolean hasNext();

// Optional remove method will remove the data last retrieved using the next method
public void remove();
```

#### Cursor variable

- Keeps track of the current data
- Within the iterator object
- Contained in the java.util package, which must be imported to iterate
- The next method keeps track of the current data that the cursor is tracking, then moves the cursor to the next element
- The hasNext method returns true when the cursor is not null

#### How to iterate through a class

```java
// 1. import Iterator from java.util
import java.util.Iterator;
public class BookList<Book> implements Iterator<Book> {
  // Implementations omitted
  // 2. Override the next and hasNext methods
  public void next() {...}
  public boolean hasNext() {...}
}

// Assuming we have a BookList object called bookList:
// 3. Implement a while loop!
while (bookList.hasNext()) {
  System.out.println(bookList.next());
}

// You can also print data using Iterable
// 1.
import java.util.Iterator;
public class BookList<Book> implements Iterable<Book> {
  // implementation omitted
  public Iterator<Book> iterator() {...}
}

// Assuming we have a BookList object called bookList:
Iterator<Book> bookIterator = bookList.iterator();
while(bookIterator.hasNext()) {
  System.out.println(bookIterator.next());
}

// When the BookList object is iterable, we can now use a for each loop to print out all of the values without using any of the iterator methods
for (Book book : bookList) {
  System.out.println(book);
}
```

#### Iterable

- Allows for direct control of object iteration for more custom behavior
- Depends on both data structure and iterator
- Iterable is dependent on iterator

#### Iterator

- Abstracts control away from user to allow the convenience for more generic needs
- Has no dependencies besides data structure to iterate through
- Iterator is independent of iterable

#### Summary

Iterable allows a class to be iterated over, either by the returned Iterator from the iterator() method or by a for-each loop (which internally, uses an Iterator).

- You might have to use a for-each loop to iterate efficiently through Java collections like ArrayList and LinkedList. These, among other classes, implement Iterable for us already. You can see which classes implement Iterable in Java's documentation: (https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)[https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html]

```java
Using a for-each loop can be sometimes more efficient than a regular for loop.

List<String> foods = new LinkedList<>();
structures.add("pasta");
structures.add("pizza");
structures.add("soup");

// this for-each loop is more efficient...
for (String food: foods) {
    System.out.println("I love eating " + food + "!");
}

// ...rather than this regular for loop
for (int i = 0; i < foods.size(); i++) {
    String food = foods.get(i); // this line requires Java to internally iterate through a portion of the linkedlist again
    System.out.println("I love eating " + food + "!");
}
```

#### Additional Iterator Resource

- Slides (https://courses.edx.org/assets/courseware/v1/0459269ca52191135b92f49642611b57/asset-v1:GTx+CS1332xI+2T2020+type@asset+block/Iterators.pdf)[https://courses.edx.org/assets/courseware/v1/0459269ca52191135b92f49642611b57/asset-v1:GTx+CS1332xI+2T2020+type@asset+block/Iterators.pdf]

---

## Comparable/Comparator interfaces

- Comparable and comparator are tools we use to compare object.
- Both are independent of each other, unlike Iterable and Iterator.

#### Comparable

- An object that implements the comparable interface can directly compare itself to another object using the compare to method.
- Allows the implementing class to define a "natural ordering."
  - In Java, the natural ordering for integers is to be in ascending order
  - e.g.

```java
public int compareTo(T y);
x.compareTo(y)

// x < y => negative
// x = y => 0
// x > y => positive
```

#### Comparator

- The user can use a Comparator to compare two objects of the same type using the compare method
- Allows the implementing class to define a custom "ordering" that is different from the natural ordering defined by the Comparable compare to method
  - e.g. We can use a Comparator to redefine integers to be in descending order

```java
public int compare(T x, T y);
comp.compare(x, y);

// x < y => negative
// x = y => 0
// x > y => positive
```

- e.g. Student Objects using Comparator

```java
// Given a student class
public class Student {
  String name;
  int age;
  String major;
  // Implementation omitted
}

// NameComparator
// Can define x < y if x's name comes first alphabetically

// AgeComparator
// Can define x < y if x's age is less than y's age

// MajorComparator
// Can define x < y if x's major comes first alphabetically

```

- e.g. HDTV class

```java
// Implementing comparable interface
public class HDTV implements Comparable<HDTV> {
  private int size;
  private String brand;
  public int getSize() { return size; }
  public String getBrand() { return brand; }
}

// To correctly implement the Comparable interface, we need to override the int compareTo method
public int compareTo(HDTV tv) {
  // We can compare based on any of the fields; this example uses size
  if (size < tv.size) { return -1; }
  else if (size > tv.size) { return 1; }
  else { return 0; } // if sizes are equal
}

// The above method can be simplified
public int compareTo(HDTV tv) {
  return size - tv.size;
}

// To implement the same as above with comparator instead
import java.util.Comparator // not in the java.lang library like comparable is
public class HDTV {
  private int size;
  private String brand;
  public int getSize() { return size; }
  public String getBrand() { return brand; }
}

// Overriding the compare object
// Because outside of the class, needs getter methods
class SizeComparator implements Comparator<HDTV> {
  public int compare(HDTV tv1, HDTV tv2) {
    return tv1.getSize() - tv2.getSize();
  }
}
```

## Summary

We know now that we can define a natural ordering of objects of a certain class by either having the class implement Comparable or by writing a different class that implements Comparator.

In this course, some data structures we encounter might use one of these two "comparing" classes to maintain an internal sorted order. For example, when we talk about binary search trees, we'll see how their nodes implement Comparable so they can maintain an ordering between nodes.

To take a closer look, let's consider the following MenuItem class:

```java
class MenuItem implements Comparable {
    private String name;
    private int price;

    public MenuItem(String name, int price) { ... }

    int compareTo(MenuItem otherItem) {
        return otherItem.price - this.price;
    }
}
```

It's important to remember that we can should call compareTo() on a MenuItem object rather than using the Java operators < (less than) or > (greater than):

```java
MenuItem item1 = new MenuItem("cheesesteak", 4);
MenuItem item2 = new MenuItem("cheesecake", 7);

// we should compare these items like this:
if (item1.compareTo(item2) < 0) {
...
}

// this DOES NOT work:
if (item1 < item2) {
...
}
```

It's also important to remember that the values returned by compareTo() are not guaranteed to be [-1, 0, 1]

```java
// this works properly
if (item1.compareTo(item2) < 0) {
System.out.println(item1.name + " is cheaper than " + item2.name);
} else if (item1.compareTo(item2) > 0) {
System.out.println(item1.name + " is more expensive than " + item2.name);
} else {
System.out.println(item1.name + " and " + item2.name + " are the same price!");
}

// this does NOT work properly: (item2.price - item1.price) is 3, so the first two conditions are false
if (item1.compareTo(item2) == -1) {
System.out.println(item1.name + " is cheaper than " + item2.name);
} else if (item1.compareTo(item2) == 1) {
System.out.println(item1.name + " is more expensive than " + item2.name);
} else {
System.out.println(item1.name + " and " + item2.name + " are the same price!");
}
```

## Big O Notation

#### Description of Big O

- Measure both time and space efficiency
- Platform/hardware independent
- High level description of algorithm
- Performance scales in relation to the input sizes
- The complexity of an algorithm is how efficient the algorithm is in terms of input sizes

#### How to calculate Big O

- Primitive operations execute in constant time, so just count them:

  - Assign value
  - Arithmetic operations
  - Comparing two entities
  - Method call or return from method
  - Access element or follow a reference

  - Note: The assumption that these operations can be done quickly is only valid if you are working with manageable, small units. For example, suppose we are multiplying two integers, which pretty simple to write code for in Java.
    - If your numbers are small enough that they fit into an int (32-bit) or a long (64-bit) typing, then the multiplication will be fast since Java optimizes computations with primitive types. However, if your numbers are very long, then the length of your numbers may pose a significant computational task since there is no longer a simple low-level instruction that can do it for you!

#### Measuring efficiency as a function

- f(n) represents the function of primitive operations on an input of size n
- There are 3 cases:
  - Worst case - _ This is Big O for this course _
    - The algorithm running with the worst set of data, worst performance
  - Best case
    - The algorithm running with the best designed set of data, fastest performance
  - Average case
    - Somewhere between and difficult to compute

#### Big O Notation

- Denoted O(f(n)) where n is the size of our inputs, f(n) is a function representing the scaling of the algorithm with n
- Typically represents the upper bound, but this course focuses on the most acccurate upper bound
  - e.g. everything in this course is O(n^4) but this is not helpful
  - Here we are looking for the tightest upper bound on the performance of an algorithm

---

## Asymptotic analysis

#### Common complexities (from optimal, descending order)

- Constant O(1)
- Logarithmic O(log(n))
- Linear O(n)
- Log-Linear O(nlog(n))
- Quadratic O(n^2)
- Cubic O(n^3)
- Exponential O(a^n) where a is a constant

- This difference between polynomial and exponential time is the highlight of the famous P != NP problem in computer science.

#### Other measures of asymptotic complexity

- 𝑂(𝑓(𝑛)) : The algorithm's long-term performance cost is upper bounded by 𝑓(𝑛). This is the most common asymptotic notation that you will see, and you will become very familiar with it in this course.
- 𝑜(𝑓(𝑛)): The algorithm's long-term performance cost is significantly upper bounded by 𝑓(𝑛). This measure is often used if we want to emphasize how small a quantity is or how slowly it grows. For example, in many numerical methods, we may want to approximate some quantity within a reasonable error. It's common to denote the error term using Little-oh notation to show that the error term grows slowly.
- Ω(𝑓(𝑛)): The algorithm's long-term performance cost is lower bounded by 𝑓(𝑛). This measure is interesting because it requires us to flip our line of thinking. Rather than thinking about how badly our algorithm can perform like in Big-O, this measure is often times used to tell us what performance costs are reasonable to begin with for a problem. If we have an algorithm that has time complexity 𝑂(𝑛4), we may think to ourselves "yikes...," but if the problem the algorithm is solving has a complexity of Ω(𝑛4), then we realize that our algorithm wasn't so bad, it's just that the problem is very hard to solve!
- Θ(𝑓(𝑛)): The algorithm's long-term performance cost is both lower bounded and upper bounded by 𝑓(𝑛). In other words, it is both 𝑂(𝑓(𝑛)) and Ω(𝑓(𝑛)). Not all algorithms will have a valid Big-Theta since it requires the algorithm to have consistent enough behavior that we can sandwich it from both above and below.

#### Conventions

- Drop constant factors
  - O(5n) -> O(n)
  - O(0.5n^2) -> O(n^2)
  - Although they don't matter theoretically, always keep them in mind in practice as they may influence which processes are more efficient for the scale of the specific task
- Drop lower order terms
  - O(n^2 + 1000n + 3) -> O(n^2)
  - O(3n + 2log(n) + nlog(n)) -> O(nlog(n))

#### Examples

- O(1)
  - Given an array of length n, returning the first element
- O(n)
  - Given an array of length n, summing up all elements in the array
- O(log(n))
  - The base doesn't matter due to change of base (m constant)
  - Given a sorted array of length n and performing a binary search

#### Additional Resources Analysis of Algorithms

- Slides (https://studio.edx.org/assets/courseware/v1/997d3accfcbe528362cba9071513a565/asset-v1:GTx+CS1332xI+2T2020+type@asset+block/AnalysisOfAlgorithms.pdf)[https://studio.edx.org/assets/courseware/v1/997d3accfcbe528362cba9071513a565/asset-v1:GTx+CS1332xI+2T2020+type@asset+block/AnalysisOfAlgorithms.pdf]
