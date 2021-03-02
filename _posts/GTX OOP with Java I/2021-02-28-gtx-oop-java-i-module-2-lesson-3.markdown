---
layout: post
title: "Module 2 Lesson 3"
date: 2021-02-28
categories: gtx cs1331xI
permalink: /:categories/:date/:title
---

Course URL: [https://learning.edx.org/course/course-v1:GTx+CS1331xI+2T2020/block-v1:GTx+CS1331xI+2T2020+type@sequential+block@e65719bfc19d4f629e19bf14d4496c24/block-v1:GTx+CS1331xI+2T2020+type@vertical+block@dc75dfe85b444741bbf321cb23fee344](https://learning.edx.org/course/course-v1:GTx+CS1331xI+2T2020/block-v1:GTx+CS1331xI+2T2020+type@sequential+block@e65719bfc19d4f629e19bf14d4496c24/block-v1:GTx+CS1331xI+2T2020+type@vertical+block@dc75dfe85b444741bbf321cb23fee344)

---

## SUMMARY

### Module 2 Lesson 3: Back to basics

- Understand how the following fundamental programming concepts are manifested in Java: whitespace, commenting, errors, variables, types, expressions, and casting.

---

## Whitespace

- def. blanks, tabs, and newline characters
- Any whitespace added beyond separating words and symbols is ignored by the compiler

## Errors

- Three types
  - compiler
  - runtime
  - logical
- Syntax vs. semantics
  - Syntax: rules of the language
  - Semantics: what a statement does or its meaning
  - Code that is syntactically correct is not always semantically correct. Code may compile, but may not do what you want it to do.

#### Compile and runtime errors

- Compiler errors represent syntax violations found in your code
- Compiler cannot compile and will return an error instead of a .class file
- Semantic errors will compile, but the JVM will generate errors when it attempts to run the code

#### Logical errors

- Semantic errors that may not terminate the program, but produces unexpected results
- To avoid: Incrementally compile and test

## Comments

- Java has three forms of comments to help document your code:
  - Line comments
    - //
  - Block (or multi-line) comments
    - /\* \*/
  - Javadoc comments
    - /\*\* \*/
    - Precedes class, field, constructor, or method declaration
    - Consists of a description and block tags

```java
// Sample javadoc

**
* Returns an Image object that can then be painted on the screen.
* The url argument must specify an absolute <a href="#{@link}">{@link URL}</a>. The name
* argument is a specifier that is relative to the url argument.
* <p>
* This method always returns immediately, whether or not the
* image exists. When this applet attempts to draw the image on
* the screen, the data will be loaded. The graphics primitives
* that draw the image will incrementally paint on the screen.
*
* @param  url  an absolute URL giving the base location of the image
* @param  name the location of the image, relative to the url argument
* @return      the image at the specified URL
* @see         Image
*/
public Image getImage(URL url, String name) {
    try {
        return getImage(new URL(url, name));
    } catch (MalformedURLException e) {
        return null;
    }
}
```

## Variables and constants

- Class variables must be declared inside class (outside any function). They can be directly accessed anywhere in class.
  - Exists as long as object is around.
- Local or method variables exists on method body entry, disappears on method body exit.
- BLock variable exists only within the brackets it was declared.
- The keyword "final" makes a variable constant -- can only be assigned once

## Primitive Types

- Integer-based values
  - byte
  - short
  - int
    - default
  - long
- Real number values
  - float
  - double
    - default
- Individual characters
  - char
- Logic-based values
  - boolean

#### Default Types

- You can override default types by appending a letter to the end
- Upper case is preferred
  e.g.

```java
long reallyBigNum = 999999999999999; // illegal
long reallyBigNum = 999999999999999L; // converts default int to long

float fraction1 = 0.1331; // illegal
float fraction1 = 0.1331F; // legal and preferred casing

double num1 = 2D;

double saturdayCelsius = (5D/9) * (saturdayFahrenheit - 32);
// equivalent to:
double saturdayCelsius = (5.0/9) * (saturdayFahrenheit - 32);
```

- You may want to force smaller types to save memory

#### Other primitive types

- char

  - characters don't necessarily follow a semantically logical order
    - the term "set" is prefrred over range
  - ASCII set
  - Unicode set
    - superset of ASCII
    - includes symbols and letters in many languages
  - e.g. char highestGrade = 'A';
    - Note wrapped in single quotes NOT double quotes

- Escape sequences
  - To use the single quote in a string, use an escape sequence \ + a descriptor of what you want to escape
    - e.g.
      - \t | tab
      - \n | new line
      - \\ | \
      - \' | '

## Arithmetic Expressions

+, -, /, \*, %

- Note that + can be used to concatenate and Java will convert ints concatenated with strings to string.

#### Integer division

- When you divide two integers in Java, the decimal part of the result is omitted. Note that there is no rounding, it's simply truncated.
  - Use a floating point value as either the numerator or denominator of an operation to preserve decimal or append an F or D. e.g.

```java
// All are good
9.0 / 2 = 4.5
9 / 2.0 = 4.5
9.0 / 2.0 = 4.5

9D / 2 = 4.5
9 / 2D = 4.5
9D / 2D = 4.5
```

- Casting is also an option.

#### String arithmetic

- concatenates strings

#### Order of operations

- is retained

#### Mixed type expressions

- Promotion

  - data conversion where Java creates floating point versions of int values in mixed type expressions

- String wins! Even when string has no characters ("")
  - String + int = String
  - String + char = String
  - double + String = String
  - boolean + String = String
  - String + boolean = String
  - int + String = String
  - String + char = String

#### Assignment conversion

- Conversion happens automatically
- Occurs during variable assignment, rather than within an expression

e.g.

```java
int average = 4;
double gpa = average; // 4.0, but original average doesn't change

double average = 4.0;
int gpa = average; // ERROR: incompatible types: possible lossy conversion from double to int
```

- _Determining the legality of a possible assignment conversion is not based on the number of bits that are used to represent a value and the number of bits a variable can hold._ Instead, they are based on whether the range of numbers of the variable’s type can fit the range of numbers of the value’s type.
  - An int’s lowest value is -2,147,483,648, which easily falls within the range of allowable values for longs. Similarly, a byte’s lowest value is -128, which falls within the range allowed by shorts.
  - Long values can fit in float variables since the minimum and maximum long values are 9,223,372,036,854,775,808 and 9,223,372,036,854,775,807 respectively; a float’s minimum and maximum are 3.40282347 x 1038, 1.40239846 x 10-45 respectively.
  - While both float and int are 32 bit types, their ranges significantly differ; i.e., the float’s range is much wider. A float variable can store int values but not the reverse.

```java
float x = 133I; // OK
int y = 1331F; // bad
```

See the box analogy of what fits inside what:
![https://courses.edx.org/assets/courseware/v1/76320839ea08d2d031e8162dd6f6a387/asset-v1:GTx+CS1331xI+2T2020+type@asset+block/legal_assignment_conversions.png](https://courses.edx.org/assets/courseware/v1/76320839ea08d2d031e8162dd6f6a387/asset-v1:GTx+CS1331xI+2T2020+type@asset+block/legal_assignment_conversions.png)

#### Casting

- Force a conversion
- e.g. (double)5/9 avoids integer division
- Cast operator has higher precedence over everything except parentheses (double)5/9 != (double)(5/9)
- Casting overrides any data loss errors
- Casting does not apply only to numeric types and is important in polymorphism
