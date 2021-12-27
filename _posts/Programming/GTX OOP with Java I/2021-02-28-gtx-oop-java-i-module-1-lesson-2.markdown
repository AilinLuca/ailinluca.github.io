---
layout: post
title: GTX OOP with Java I - Module 1 Lesson 2
categories: gtx cs1331xI
permalink: pretty
---

Course URL: [https://learning.edx.org/course/course-v1:GTx+CS1331xI+2T2020/block-v1:GTx+CS1331xI+2T2020+type@sequential+block@8d5ddaca3fc64141b9b5c9928ca64881/block-v1:GTx+CS1331xI+2T2020+type@vertical+block@33dd330d1001467ea930ba71c085083e](https://learning.edx.org/course/course-v1:GTx+CS1331xI+2T2020/block-v1:GTx+CS1331xI+2T2020+type@sequential+block@8d5ddaca3fc64141b9b5c9928ca64881/block-v1:GTx+CS1331xI+2T2020+type@vertical+block@33dd330d1001467ea930ba71c085083e)

---

## SUMMARY

### Module 1 Lesson 2: Why object-oriented programming?

- Gain a basic understanding of the motivation behind object-oriented programming
- Understand how to begin thinking of problem solutions in terms of classes and objects

---

## Demo

```java
public class FahrenheitToCelsius {
  public static void main(String[] args) {
    int saturdayFahrenheit;
    int sundayFahrenheit;
    saturdayFahrenheit = 78;
    sundayFahrenheit = 81;
    double saturdayCelsius = (5.0/9) * (saturdayFahrenheit - 32);
    double sundayCelsius = (5.0/9) * (saturdayFahrenheit - 32);
    System.out.println("Weekend Averages");
    System.out.println("Saturday: " + saturdayCelsius);
    System.out.println("Sunday: " + sundayCelsius);
  }
}

// use double to account for decimals in the division results
```

#### Identifiers

- identifiers can include letters, digits, \_, and $
- a digit cannot be starting character
- reserved words cannot use
- Java is case sensitive

#### Variable types

- Integer-based:
  - byte
  - short
  - int
  - long

#### Demo Running the program

```bash
javac FahrenheitToCelsius.java
java FahrenheitToCelsius

// will println to console
```

## Shifting to objects

- In OOP objects have:
  - Attributes: represent state
  - Actions: reprsent behavior
- Objects can interact with each other

#### Representing objects

- Classes represent objects
  - Variables represent state
  - Methods represent behavior
- Each object created from a class has its own copy of the classes state variables

#### Benefits

- Classes organize functions and attributes
- They are easy to modify -- just don't forget to recompile

#### Alternatives

- procedural programming
  - a program without classes, but built from well-crafted methods
  - makes sense for small programs
  - for large programs, classes reduce complexity by grouping not just statements but multiple methods and related data at a time
