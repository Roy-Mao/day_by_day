# Understanding shallow copy and deep copy

## Ruby dup and clone methods will make a shallow copy of the object, but what is a shallow copy?

The core is to understand shallow copy does not copy everything from the original object. It just share the reference with the original object. For exmaple, considering the following code:
```ruby
a = ["a", "b"]
b = a.dup
```
How to understand these two lines?


### Question 1: Why does between b[0] = "c" not change the original array while b[0].replace("c") do?

### Question 2: a = ["a", "b", ["c", "d"]]; b = a.dup; Why b[0] = "x" not changing the original array while b[2][0] do?




Example:
```ruby
a = ["a", "b"]
=> ["a", "b"]
a.object_id
=> 233440
a[0].object_id
=> 233480
a[1].object_id
=> 233460
b = a.dup
=> ["a", "b"]
b.object_id
=> 147926
b[0].object_id
=> 233480
b[1].object_id
=>233460
```
