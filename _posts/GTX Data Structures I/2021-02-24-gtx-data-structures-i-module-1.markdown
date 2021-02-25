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
- If we know what data type the array will store, then we know exactly how much memory is required for storage of each instance of the data (for example, the int typing in Java is always 32-bits: even if the number is small, 32-bits is always allocated for an int). With this information, we know the starting memory address (index 0), the number of memory addresses each index occupies, and each index is contiguous in memory. This allows us to find the memory address of an index with some arithmetic called pointer arithmetic. If we want to access index 𝑖 in the array, then the memory address we want is

𝚗𝚎𝚠*𝚊𝚍𝚍𝚛𝚎𝚜𝚜 = s𝚝𝚊𝚛𝚝*𝚊𝚍𝚍𝚛𝚎𝚜𝚜 +𝑖∗𝚍𝚊𝚝𝚊_𝚜𝚒𝚣𝚎

which is an 𝑂(1) operation since it's just 3 arithmetic operations (as opposed to 𝑛 , the size of the array).
