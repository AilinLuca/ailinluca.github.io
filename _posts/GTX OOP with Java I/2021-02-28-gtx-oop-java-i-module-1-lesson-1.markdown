---
layout: post
title: "Module 1 Lesson 1"
date: 2021-02-28
categories: gtx cs1331xI
permalink: /:categories/:date/:title
---

Course URL: [https://learning.edx.org/course/course-v1:GTx+CS1331xI+2T2020/block-v1:GTx+CS1331xI+2T2020+type@sequential+block@b58811ec56044089a390d0770f6cf0a5/block-v1:GTx+CS1331xI+2T2020+type@vertical+block@8d2be899f78443dfae50b4e79187b2b6](https://learning.edx.org/course/course-v1:GTx+CS1331xI+2T2020/block-v1:GTx+CS1331xI+2T2020+type@sequential+block@b58811ec56044089a390d0770f6cf0a5/block-v1:GTx+CS1331xI+2T2020+type@vertical+block@8d2be899f78443dfae50b4e79187b2b6)

---

## SUMMARY

### Module 1 Lesson 1: Introduction to Java

- The history of Java
- The basic elements of a Java program
- How to execute programs using the language’s virtual machine approach to translation

---

## History

- Java 1.0 was officially released in 1996 by a company called Sun Microsystems.
- Existing languages needed to be safer in order to be deployed on devices, which sometimes controlled critical aspects of daily life.
- Although Java does not guarantee flawless programs, its programmers inherently avoid much of the memory and security issues that are common in languages like C and C++. The reason is primarily due to very powerful mechanisms, like automatic memory management, that its creators built into the language.
- Today, Java is a product of Oracle Corporation, which acquired Sun Microsystems in 2010. Oracle claims that a few billion devices currently run Java--spanning servers, cell phones, ATMs, cable boxes, TVs and much more.
- Applets have been deprecated in the recent versions of the language.

## Basic elements of Java programs

#### Requirements

- Statements can be grouped together using a method
- A program must have at least one or more methods
- In order to be executable, a program must have a method called "main"
- Methods are enclosed in classes
- A program must have one or more classes

#### First program

```java
public class HelloWorld {
    // The main method is automatically executed when you run a Java program
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

#### Saving the code

- File name must match class
- Extension must be .java
- e.g. HelloWorld.java

## Translation: Compilers vs. interpreters

- Compilers

  - Take a program that is written in one high-level language and translates it to a low-level version that's either machine code or something close.
  - Generates a new set of files
  - Examples: C, C++
  - Pros:
    - Faster; compile once and run as many times as needed
  - Cons:
    - To achieve the same level of compatibility as an interpreted language, the programmer needs to compile every written program to every kind of instruction set that exists. This is cumbersome and neverending.

- Interpreter
  - Translates a program on the fly
  - An interpreter language performs translation as the program is running. No intermediate files involved.
  - Examples: Python, PHP
  - Pros:
    - Compatibility.
      - The type of machine code the processor understands (aka its _instruction set_) can be incompatible to another processor. It is easy to find different types of processors from the same manufacturer with incompatible instruction sets. Because interpreters don't require intermediate files to store a particular kind of machine code, they offer greater platform independence.
        - Interpreted languages will execute on any computer without extra work from the programmer. Computer just needs an interpreter available to match the processor's instruction set.
  - Cons:
    - Slower; must translate each time

#### Java's hybrid approach

- Source code is not compiled directly to machine code like in C and C++.
- A compiler generates what is known as bytecode and stores that in one or more files with a “.class” extension
  - Bytecode is not fixed to a specific type of processor’s instruction set. However, it comes close to the low level of machine code without making significant assumptions about the kind of processor that will run it.
- Once the bytecode for a Java program is generated, you can then actually run it using an interpreter that can translate the bytecode to the machine language of the particular processor of the target computer.
  - Since bytecode is already at a low-level, the translation costs are not as significant.
- The Java interpreter is often referred to as the Java Virtual Machine (or just JVM).
  - The “Virtual Machine” part of the name is derived from the fact that compiled Java code is not executed by a real processor. Rather a piece of software, the interpreter, performs the execution.
- Just-In-Time compilation

## The Java development kit

#### Compiling and executing java

To compile Java code in the terminal: 0. To access the necessary tools for translating and executing Java programs using the terminal, install the Java Development Kit (or JDK).

1. In the folder you have the .java file in, execute

```bash
javac HelloWorld.java
```

2. This will generate a compiled file with a .class extension

To initiate the Java interpreter and start execution of the class file:

3. Simply enter the command:

```bash
java HelloWorld
```

Note how the name does not have the .class extension.

#### About the JDK

- The JDK is a large software product that consists of several tools for creating, executing, and managing Java programs. Appropriately, the compiler and interpreter are two such (very important) components.
  - Java Runtime Environment (or JRE) is specifically used to execute Java code, and does not include writing and compiling.
  - The instructions for downloading and installing the JDK version that's required for the demos\* and assignments in this class can be found [HERE](https://courses.edx.org/assets/courseware/v1/e632beecc04ac111f6420bc1a07f9fb8/asset-v1:GTx+CS1331xI+2T2020+type@asset+block/Installing_Java_Guide__JDK_11_.pdf).
    - This particular version is free and avoids any licensing restrictions on the programs you generate with it.
