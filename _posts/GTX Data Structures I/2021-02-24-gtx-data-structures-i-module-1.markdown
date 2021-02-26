---
layout: post
title: "Module 1"
date: 2021-02-24
categories: gtx cs1332x1
permalink: /:categories/:date/:title
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
