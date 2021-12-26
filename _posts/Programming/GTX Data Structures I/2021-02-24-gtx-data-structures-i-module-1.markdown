---
layout: post
title: GTX Data Structures I - Module 1
categories: gtx cs1332xI
permalink: /:title
---

Course URL: [https://learning.edx.org/course/course-v1:GTx+CS1332xI+2T2020/block-v1:GTx+CS1332xI+2T2020+type@sequential+block@1b64b5c8d73f444a94a18c3a03207684/block-v1:GTx+CS1332xI+2T2020+type@vertical+block@92e7ec2a2a414912b911744535a5640b](https://learning.edx.org/course/course-v1:GTx+CS1332xI+2T2020/block-v1:GTx+CS1332xI+2T2020+type@sequential+block@1b64b5c8d73f444a94a18c3a03207684/block-v1:GTx+CS1332xI+2T2020+type@vertical+block@92e7ec2a2a414912b911744535a5640b)

---

## SUMMARY

### Module 1: Arrays, ArrayList, Iterators, Comparators, time complexity, and present recursion specific to basic data structures.

---

## Arrays

- Not primitive.
- Consists of a sequence of cells; each cell has an associated index that is unique to that cell.
- Each cell stores a single element.
- The storage for the array is allocated in a contiguous space in the memory.
  - Memory is allocated statically.
  - If you exceed the capacity, you must manually resize the array. The time complexity of this is O(n). You must copy in all of the old array elements to the new array.
- All elements in the array must be the same type.
  - Arrays can store, for example: Objects, references, strings
- Accessing an element in the array with a known index is O(1)
- Searcing for an element in the array with an unknown index is O(n)

#### To create an array

- When you declare an array, it doesn't have to immediately construct an array
- You can initialize an array with declared values
- You can create an array to specific capacity and initialize with default values

#### Pointer arithmetic

- What is needed to achieve O(1) array access?
  1. The memory allocated needs to be contiguous; meaning that each array index occupies adjacent memory locations.
  2. The array needs to know the memory address of the very first entry in the array, index 0.
  3. The array's data typing needs to be defined beforehand.

If we know what data type the array will store, then we know exactly how much memory is required for storage of each instance of the data (for example, the int typing in Java is always 32-bits: even if the number is small, 32-bits is always allocated for an int).

With this information, we know the starting memory address (index 0), the number of memory addresses each index occupies, and each index is contiguous in memory. This allows us to find the memory address of an index with some arithmetic called pointer arithmetic. If we want to access index 𝑖 in the array, then the memory address we want is:

```java
new_address = start_address + i  * data_size;
```

which is an 𝑂(1) operation since it's just 3 arithmetic operations (as opposed to 𝑛 , the size of the array).

---

## ArrayLists

#### Abstract Data Types (ADTs)

- An abstract data type (ADT) is a model description of a data type that is defined by its behaviors and operations.
- Analogous to Java's interfaces in that they set the framework for what operations (methods) are available as well as what these operations do, but they leave the actual implementation details abstract.
- Actual concrete implementations of data handling are called data structures.

#### List ADT

- Defined as a sequence of data values that are accessible via indexing.
- At minimum, the following operators are required:
  - addAtIndex(int index, T data):
    - Adds the data to the list at the specified index. Any data with index 𝑖≥ index has a new index 𝑖+1 to make room for the new data.
  - removeAtIndex(int index):
    - Removes the data at the specified index from the list. Any data with index 𝑖> index has a new index 𝑖−1 to maintain the sequence.
  - get(int index):
    - Returns the data at the specified index.
  - isEmpty():
    - Returns whether the list is empty or not.
  - clear():
    - Resets the list to an initial configuration with no data.
  - size():
    - Returns the number of data currently stored in the list.
- How these methods are implemented and how efficient they are are delegated to the data structures themselves.
- Example data structures that implement the List ADT:
  - ArrayLists
  - LinkedLists

#### ArrayList

- Has the properties of an array: contiguous indexed cells, storage allocated contiguously
- A list backed by an array
- Lists are an ADT.
- ArrayLists are NOT an ADT. ArrayLists are an example of a data structure since their implementation is well-defined. They implement the List ADT and are a concrete fulfillment of the contract given by the List ADT. There may be variations of ArrayLists and their details out there, but in general, we are referring to a concrete data structure here.
- Generic data structures like ArrayLists can only store objects, e.g. like strings
  - To have an ArrayList of primitives, you would need their wrapper classes
- Must import the java utility package to use ArrayLists
- The default size of the Java ArrayList is 10

#### Creating ArrayLists

```java
// The data type must be an object
// Generic typing is implied from declaration <>
// Name is list
ArrayList<String> list = new ArrayList<>();
```

#### Dynamic Allocation

- ArrayList is considered dynamic
- An ArrayList is an abstraction/ a wrapper for an array
- Resize is handled automatically by the implementation without user knowledge
- Dynamically allocated in that the array is reallocated and copied when more space is necessary

#### ArrayList Terminology

- Size is the number of objects stored in the ArrayList
- Capacity is the number of cells in the ArrayList instantiated to accept objects without a resize

#### ArrayList Requirements

- Data must be contiguous
  - Not the same as contiguously allocated memory, which is also a requirement
  - No null spaces between elements
- Data must be zero-aligned
  - All cells must be populated beginning with the cell at index 0
- For efficient operations, the size should be stored. Index size gives us access to the next empty spot in O(1)

#### Adding to the back

- Whenever an add is done, we increment size
- If we reach capacity and have to resize before we add, this costs O(n) behind the scenes. This is the worst case scenario, but rare.
- Amortized analysis: Look at the cost over time rather than the cost per add operation. Under this analysis, the cost of a single add is O(1).

#### Adding elsewhere

- The cost is O(n).
- We must shift all data down by 1 cell in order to add the new data.
- Adding an element requires data shifts because we do not want to overwrite existing data with new data without the user's permission. It is not because we are maintaining contiguity. If the user tried to add data to an index that would break contiguity, then you would throw an error/exception since it breaks the requirements of an ArrayList.

#### Removing

- Removing from the back is O(1)
- Removing from an arbitrary point requires data shifts, and is O(n).
- For the example shown of removing at index 0, the shift is required not due to contiguity but due to the zero-aligned requirement.

---

## Amortized Analysis

- "amortized" means to "spread cost out over time".
- In an amortized analysis, rather than looking at the per operation cost, we instead consider all sequences of 𝑛 operations and average the cost over the number of operations.

```java
// For adding to the back of an array list
//  Under a worst case amortized analysis, we are looking for the sequence of  𝑛  operations that gives us the worst performance. For adding to the back, there are only 2 cases to consider: there is a resize or there isn't. If there isn't a resize, then there is  𝑂(1)  work to be done, say 2 units of time: putting the data at index size and incrementing size.
Amortized Cost = (Total cost of all operations) / (# of operations) = 2n / n = 2 = O(1)
```

- Clearly any worst case sequence will involve a resize since it's an 𝑂(𝑛) operation, right? The question is, "how many resize operations can occur in 𝑛 operations?" Well, in most ArrayList implementations, when a resize occurs, the capacity is doubled.

```java
// After  𝑛  operations, we cannot possibly trigger another resize in that window, meaning for every sequence of  𝑛  operations, we can only trigger a resize at most once.
// The resize cost we can say is 2𝑛 units of time for accessing the old array entries, and placing them in the new array. With this in mind, our new amortized cost becomes:
Amortized Cost = (Total cost of all operations) / (# of operations)
Amortized Cost = (Resize Cost + Normal Operation Cost) / n
Amortized Cost = (2n + 2n) / n = 4 = O(1)

```

#### Soft vs. Hard Removals

- Hard Removal
  - Ensure that the data you removed is completely removed from the backing structure (i.e. if you searched the entire array, you would not find the data). In an ArrayList, this often happens naturally in the shifting process, but when necessary, you may need to manually set an array position to null in order to perform a hard removal.
  - Preferred because:
    - Removes sensitive data
    - Makes data structure implementation details easier
- Soft Removal

  - Leave the data in the data structure unless it is absolutely necessary to get rid of it. For example, in the ArrayList, the "end" of the ArrayList is controlled by the size variable. So, it may not be strictly necessary to completely remove the data at the end if it's removed since the end can just be controlled by the size if it's done carefully.

- Removing from an array is O(1) process. You can do it by setting the array to an empty array and setting size to 0.

---

## Recursion

#### Requirements for recursion

- _Base case(s):_ Termination condition(s) that ensure all branches of recursion stop eventually
- _Recursive case(s):_ Method calls to itself
- _Progress to Base Case:_ Each recursive call must move towards the base case in some way

#### Basic structure of recursion

```java
public return_type recursiveEx(param_1, ..., param_n) {
    if (base_case1) {
        // termination logic for base case 1
    } else if (base_case2) {
        // termination logic for base case 2
    }
    ...// other base cases if necessary
    else {
        // logic for setting up recursive calls
        return recursiveEx(newParam_1, ..., newParam_n);
    }
}
```

- Base cases may have a return statement depending on method and return type
- Base cases never have a recursive call within them
- There can be multiple recursive calls
- There can be multiple recursive calls in a single statement
- There can be computation statements before or after recursive calls
- In recursion call, parameter must change toward base case

#### Tips for finding the subproblem

- E.g. Write a power function to calculate b^x where b is an integer and x is a power of 2
  - Substructure 1: b^x = b \* b^(x-1), b^0 = 1
  - Substructure 2: b^x = b^(x/2)b^(x/2), b^0 = 1
- Avoid overlapping base case(s) if possible
  - [arms-length recursion](<https://en.wikipedia.org/wiki/Recursion_(computer_science)#:~:text=Short%2Dcircuiting%20the%20base%20case%2C%20also%20known%20as%20arm's%2D,checking%20for%20the%20base%20case.>): Short-circuiting the base case, also known as arm's-length recursion, consists of checking the base case before making a recursive call – i.e., checking if the next call will be the base case, instead of calling and then checking for the base case. Short-circuiting is particularly done for efficiency reasons, to avoid the overhead of a function call that immediately returns.
- Separate your code clearly into cases for the recursion and base cases

#### Recursion example

- e.g. The greatest common divisor of two or more integers, x and y, where x and y are not both zero, is the largest integer dividing both x and y with no remainder, denoted gcd(x,y).
  - Look at all numbers less than x and y
  - Subproblem paths:
    1. Euclidian division version where the larger is divided by the smaller and keeping the remainder.
    2. Euclid's original subtraction-based version where the smaller is subtracted from the larger repeatedly
    3. Binary GCD version where both numbers are divided by 2 and tracks the count of binary divisions.

#### Modulo example:

- Given x > y, x = q \* y + r, r = x mod y, y > r
  - That is to say, mathematically x = quotient \* y + r
- Recursive case: gcd(x,y) = gcd(y,r)
- Base case: gcd(z,0) return z

- Example in action:
  1. gcd(1482, 819) -> 1482 % 819 = 663
  2. gcd(819, 663) -> 819 % 663 = 156
  3. gcd(663, 156) -> 663 % 156 = 39
  4. gcd(156, 39) -> 156 % 39 = 0
  5. gcd(39, 0) -> 39 // base case
  6. GCD is 39

```java
public int gcd(int x, int y) {
    if (y == 0) { // Base case
        return x;
    } else {
        int r = x % y;
        return gcd (y, r);
    }
}
```

#### Exponentiation by repeated squaring

- We are trying to calculate 𝑏𝑥 where 𝑏 is an integer and 𝑥 is a power of 2.
  - Substructure 1: b^x = b \* b^(x-1), b^0 = 1
    - O(x)
  - Substructure 2: b^x = b^(x/2)b^(x/2), b^0 = 1
    - O(log(x))
    - More efficient

## Modeling Recursion

- structural recursion
  - recursion based on some underlying structure or object rather than numbers like in the factorial, fibonacci, exponentiation, and gcd examples

---

## Assignment
