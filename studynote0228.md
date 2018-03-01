# Understanding shallow copy and deep copy

## Ruby dup and clone methods will make a shallow copy of the object, but what is a shallow copy?

The core is to understand shallow copy does not copy everything from the original object. It just share the reference with the original object. For exmaple, considering the following code:
```ruby
a = ["a", "b"]
b = a.dup
```
How to understand these two lines?

["a", "b"] is an object while variable a is not an object. a merely stores an address referece value to the object ["a", "b"]. a.dup creates a brand new object with which is a shallow copy of the object ["a", "b"]. 

How is the copied object ["a", "b"] different from the original object ["a", "b"]?

The difference is that, while the a[0]("a") and a[1]("b") in the original object is real object "a" and "b", the b[0]("a") and b[1]("b") in the copied object are not real object, but the address reference to the real object in the original array object. Which means while the object_id of a and b are different, the object_id of a[0], a[1] is the same as b[0], b[1]. Therefore, any changes to a[0], a[1], b[0], b[1] will be reflected in their respective counterpart. For details, see Question1 below.

### Question 1: Why does between b[0] = "c" not change the original array while b[0].replace("c") do?

Why b[0] = "c" not affecting the a[0]?

Because Ruby first creates a real object "c". b[0] is actually a variable of address reference. Now, this address reference is assigned to a new address, which is the address of the first character address of string "c". Until this point, b[0] reference address is not the real object "a" in the original array object, but changed to the address of "c", but a[0] still holds the address value of the real object "a". Therefore, b[0] is changed to "c" while a[0] still remains as "a".

Why b[0].replace("c") affects the a[0] value?

Firstly we need to understand the method "replace()". It is a mutating method, which means it will mutates the original object, or it is a non-mutating object, which means it make a copy of the original object and operating on the copied object and then returns the modified version of the copied object. Obviously, replace is a mutating method and it operates directly on the real object.

Then, we consider what is b[0]? Is b[0] a real object that replace can operate on directly? The answer is no. b[0] is not a object, it is a just a address reference holder, which points to the a[0], and a[0] is also an address reference which points to the real object "a". So we find the real object that method replace() will operate on. 

Finally, we replace the original object "a" to "c". Because we have already changed the original object but not changing the reference address value in either a[0] or b[0]. Therefore, bothe a[0] and b[0] are now pointing the newly created real object "c". 



### Question 2: a = ["a", "b", ["c", "d"]]; b = a.dup; Why b[0] = "x" not affecting the original array while b[2][0] = "x" do?

The answer to the reason why b[0] = "x" not affecting the original object is the same as the above question. I also conclude that we can call b[0] = "x" is a "shallow change", because it is just one level down, and "shallow change" will not affect the value of the original object. We can call b[2][0] a "deep change", because it is two levels down, and "deep change" will affect the value of the original object. Because two levels down "deep change" actually touches the real object, but one level donw "shallow change" only touches the addresss reference value, not the real object. Therefore, not affecting the corresponding value.

### Conclusino:

"Shallow change" ==> only touches the reference value, not the real object ==> will not affect the value of counterpart
"Deep change"  ==> Actually touches the real object ==> will affect the value of the counterpart.


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
