# Ada-Basics

# Ada Programming Guide

## Table of Contents
1. [Introduction](#introduction)
2. [First Steps in Ada](#first-steps-in-ada)
3. [Subprograms](#subprograms)
4. [Arrays](#arrays)
5. [Elementary Data Structures](#elementary-data-structures)
6. [Packages and Abstract Data Types](#packages-and-abstract-data-types)
7. [Type Extension and Inheritance](#type-extension-and-inheritance)
8. [Generics](#generics)
9. [Exceptions and Run-Time Checks](#exceptions-and-run-time-checks)
10. [Composite Types](#composite-types)
11. [Access Types](#access-types)
12. [Numeric Types](#numeric-types)
13. [Input-Output](#input-output)
14. [Program Structure](#program-structure)
15. [Concurrency](#concurrency)
16. [Systems Programming](#systems-programming)
17. [Real-Time Systems](#real-time-systems)
18. [Distributed and High Integrity Systems](#distributed-and-high-integrity-systems)
19. [Glossary of ARM Terms](#glossary-of-arm-terms)

## Introduction

Ada is a structured, statically typed, imperative, and object-oriented high-level computer programming language, extended from Pascal and other languages. Ada improves code safety and maintainability by leveraging the compiler to find errors in favor of runtime checks.

### Key Features of Ada:
- Strong typing
- Modular programming
- Packages
- Exception handling
- Concurrency
- Generics
- Object-oriented programming

## First Steps in Ada

### Hello World Program

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Hello is
begin
   Put_Line ("Hello, world!");
end Hello;
```

### Compilation and Execution
To compile and execute an Ada program, you can use the GNAT compiler:

```sh
gnatmake hello.adb
./hello
```

## Subprograms

### Procedures and Functions
In Ada, subprograms can be either procedures or functions. Procedures are similar to functions but do not return a value.

#### Procedure Example

```ada
procedure Greet (Name : in String) is
begin
   Ada.Text_IO.Put_Line ("Hello, " & Name & "!");
end Greet;
```

#### Function Example

```ada
function Add (X, Y : Integer) return Integer is
begin
   return X + Y;
end Add;
```

### Parameter Modes
- `in`: The parameter is read-only.
- `out`: The parameter is write-only.
- `in out`: The parameter can be read and written.

## Arrays

### Declaring Arrays

```ada
type Int_Array is array (1 .. 10) of Integer;
My_Array : Int_Array;
```

### Initializing Arrays

```ada
My_Array : Int_Array := (1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
```

### Accessing Array Elements

```ada
My_Array(1) := 42;
Ada.Text_IO.Put_Line(Integer'Image(My_Array(1)));
```

## Elementary Data Structures

### Records

```ada
type Person is record
   Name : String (1 .. 50);
   Age  : Integer;
end record;

John : Person := (Name => "John Doe", Age => 30);
```

### Access Types (Pointers)

```ada
type Person_Access is access all Person;
John_Ptr : Person_Access := new Person'(Name => "John Doe", Age => 30);
```

## Packages and Abstract Data Types

### Package Example

```ada
package Stack is
   procedure Push (Item : in Integer);
   procedure Pop (Item : out Integer);
   function Is_Empty return Boolean;
end Stack;
```

### Package Body

```ada
package body Stack is
   Stack_Array : array (1 .. 10) of Integer;
   Top         : Integer := 0;

   procedure Push (Item : in Integer) is
   begin
      if Top < 10 then
         Top := Top + 1;
         Stack_Array(Top) := Item;
      end if;
   end Push;

   procedure Pop (Item : out Integer) is
   begin
      if Top > 0 then
         Item := Stack_Array(Top);
         Top := Top - 1;
      end if;
   end Pop;

   function Is_Empty return Boolean is
   begin
      return Top = 0;
   end Is_Empty;
end Stack;
```

## Type Extension and Inheritance

### Tagged Types and Inheritance

```ada
type Shape is tagged record
   Color : String (1 .. 20);
end record;

type Circle is new Shape with record
   Radius : Float;
end record;

Circle1 : Circle := (Color => "Red", Radius => 5.0);
```

### Polymorphism

```ada
procedure Draw (S : in Shape) is
begin
   -- Drawing code here
end Draw;

procedure Draw (C : in Circle) is
begin
   -- Drawing code for circle here
end Draw;

C : Circle := (Color => "Blue", Radius => 10.0);
Draw(C);
```

## Generics

### Generic Packages

```ada
generic
   type Element_Type is private;
package Stack_Generic is
   procedure Push (Item : in Element_Type);
   procedure Pop (Item : out Element_Type);
   function Is_Empty return Boolean;
end Stack_Generic;
```

### Instantiating Generics

```ada
package Int_Stack is new Stack_Generic (Element_Type => Integer);
```

## Exceptions and Run-Time Checks

### Declaring and Raising Exceptions

```ada
My_Exception : exception;

raise My_Exception;
```

### Handling Exceptions

```ada
begin
   -- Some code that might raise an exception
exception
   when My_Exception =>
      Ada.Text_IO.Put_Line("My_Exception was raised.");
end;
```

## Composite Types

### Discriminated Records

```ada
type Shape_Kind is (Circle, Rectangle);
type Shape (Kind : Shape_Kind) is record
   case Kind is
      when Circle =>
         Radius : Float;
      when Rectangle =>
         Length, Width : Float;
   end case;
end record;
```

## Access Types

### General Access Types

```ada
type Int_Access is access all Integer;
P : Int_Access := new Integer'(10);
```

### Access-to-Subprogram Types

```ada
type Subprogram_Access is access procedure;
```

## Numeric Types

### Integer Types

```ada
type Integer_Type is range -100 .. 100;
```

### Floating Point Types

```ada
type Float_Type is digits 6;
```

## Input-Output

### Text_IO

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Main is
begin
   Put_Line("Hello, Ada!");
end Main;
```

### File_IO

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Main is
   File : File_Type;
begin
   Open (File, Out_File, "output.txt");
   Put_Line (File, "Hello, Ada!");
   Close (File);
end Main;
```

## Program Structure

### Compilation Units

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Main is
begin
   Put_Line("Hello, Ada!");
end Main;
```

## Concurrency

### Tasks

```ada
task type Worker is
   entry Start;
end Worker;

task body Worker is
begin
   accept Start do
      -- Work here
   end Start;
end Worker;
```

### Protected Objects

```ada
protected type Counter is
   procedure Increment;
   function Value return Integer;
private
   Count : Integer := 0;
end Counter;

protected body Counter is
   procedure Increment is
   begin
      Count := Count + 1;
   end Increment;

   function Value return Integer is
   begin
      return Count;
   end Value;
end Counter;
```

## Systems Programming

### Representation Clauses

```ada
type Byte is mod 256;
for Byte'Size use 8;
```

### Interfaces to Other Languages

```ada
procedure C_Function;
pragma Import (C, C_Function);
```

## Real-Time Systems

### Scheduling and Priorities

```ada
pragma Priority (10);
```

### Task Dispatching

```ada
pragma Task_Dispatching_Policy (FIFO_Within_Priorities);
```

## Distributed and High Integrity Systems

### Distributed Systems

```ada
pragma Partition_Elaboration_Policy (Sequential);
```

### High Integrity Systems

```ada
pragma Restrictions (No_Recursion);
```

## Glossary of ARM Terms

- **Access Type**: A type that provides a reference to an object of a given type.
- **Attribute**: A characteristic of an Ada entity that can be queried, such as the 'First attribute of an array type.
- **Constraint**: A restriction on the values of a type, such as a range of integers.
- **Discriminant**: A parameter that affects the structure or behavior of a composite type.
- **Elaboration**: The process of preparing an Ada unit for execution, including storage allocation and initialization.
