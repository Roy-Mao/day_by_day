# Ruby strings under microscope

## How Ruby interpret strings in Ruby 1.8 is different from 1.9

### Keyword: 24

### One sentence summerize:
  
  > It turns out that the MRI Ruby 1.9 interpreter is optimized to hanle tings containing 23 characters or less more quickly than longer strings. Thi is not true for Ruby 1.8

For example, considering the following two strings, are they identical?

```ruby
str = "1234567890123456789012" + "x"
str = "12345678901234567890124" + "x"
```

They look identical, but actually quite different in Ruby 1.9 Interpreter. The first line creates a string of 23 characters while the second creates a string of 24 characters.

## Different types of strings in Ruby

1. Heap Strings
2. Shared Strings
3. Embedded Strings

Because Ruby (the RString struc to be exactly) is implemented by C language, but C has no concept of Object, only Struct, before we dive into the details of Heap Strings, Shared Strings and Embedded Strings, we would better to consider what is a struct in C and how is Struct different from Object.

## Now, what is a Struct in C?
	

One sentence answer: C structure is a collection of different data types which are grouped together and each element in a C structure is called memeber

## What is the difference between a C variable, C array and C struct?

* C variable can only hold one single data of only one type

* C arrays can hold multiple data of the same type

* C struct can hold multiple data of different types




