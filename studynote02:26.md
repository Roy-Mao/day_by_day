# Ruby pass by reference or pass by value?

## Here is a great example I find on StackFlow to answer this question

```ruby
  str = "foo"

  def mutate(str2)
    puts "str2: #{str2.object_id}"
    str2.replace "bar"  # This replace method changes the original object
    str2 = "baz"
    puts "str2: #{str2.object_id}"
  end

  str.object_id # => 2004
  mutate(str) # str2: 2004, str2: 2006
  str # => "bar"
  str.object_id # => 2004
```

## Ruby **seems** passing value by value for immutable object (like some Float or Integer)

Looking at the following example

```ruby
  x = 10
  def change_value(val)
    val = 20
  end
  puts x #10
  change_value(x)
  puts x #10
```

This example above seems like Ruby passes value by value, right? But wait, consider the following code

## Ruby seems passing value by reference, too for mutable object (like arrays and string)

Looking at the following example

```ruby
  x = '10'
  def change_value(val)
    val << '20'
  end

  puts x #10
  change_value(x)
  puts x #1020
```

Hah, now it **seems** that Ruby passes value by reference from the above example, right?

## You have to remember this sentence for thousands times

> Immediate values are not passed by referencebut but are passed by value: nil, true, false, Fixnums, Symbols, and some Floats

Then considering if the following code result in true or false

```ruby
  Object.new.object_id == Object.new.object_id     # => false
  (21 * 2).object_id == (21 * 2).object_id         # => true
  "hello".object_id == "hello".object_id           # => false
  "hi".freeze.object_id == "hi".freeze.object_id   # => true
```


## Mutating methods and Non-mutating method

When using a method, it is very important to think if a method is a mutating method (meaning it will change the original string in place) or a non-mutating method (meaning to produce a copy of the modification). Consider the following demonstrating code.

Example 1 with a non-mutating method "+" to concatenate two string

```ruby

def change_str(str)
  puts str.object_id # => 70232513267740
  str = str + 'bar'
  puts str.object_id # => 70232513267320
end

s = 'foo'
change_str(s)
puts s # => foo

```

Example 2 with a mutating method "concat" to concatenate two string

```ruby

def change_str(str)
  puts str.object_id # => 70151978493860
  str.concat('bar')
  puts str.object_id # => 70151978493860
end

s = "foo"
change_str(s)
puts s  # => foobar

```

Example 3

```ruby
my_string = "roy"

def change_string(str)
  puts "the object id of #{str} is #{str.object_id}"
  str = "changed"   # "change" string is a mutable object, thus what stored in str is a reference (address) number to the string "changed", which is different from the (str) as the method argument
  puts "then, the object id of #{str} is #{str.object_id}"
end

puts my_string

change_string(my_string)

puts my_string
```

Exmaple 4

```ruby
my_string = "roy"

def change_string(str)
  puts "the object id of #{str} is #{str.object_id}"
  str = str << "!"   #str here is also a local variable, which has nothing to do with the (str) in the method argument. The thing stroed in the local str is a reference to the original object
  puts "then, the object id of #{str} is #{str.object_id}"
end

puts my_string

change_string(my_string)

puts my_string

```


## Why use "seems" in the statement that Ruby seems to pass by value for immutable object? Because actually , it still passes by reference for immutable object

Take a look at the following example

```ruby
  def print_id number
    puts "In method object id = #{number.object_id}"
  end

  value = 33

  puts "Outside method object id = #{value.object_id}"

  print_id value

  # Outside method object id = 67
  # Inside method object id = 67
```

If Ruby passes by value for immutable object like a Fixnum, then why the object id in method is the same as the object id outside method? 

What is 'value'? It is a variable? What does it store? Does it store the actual content of the object Fixnum 33? No. All variables in Ruby are merely a reference (memory address number) to the actual object. Therefore value stroes the address value for the Integer 33. Then, when we pass teh value varible to the print_id method, we are not making a copy of the immutable object 33, instead, we are making a copy of the variable value(a memory address), which is a reference to the original object, so that number and value are both referenced to the integer 33. From this point of view, we can conclude that Ruby actually passes by reference all the way down exclusively.

## Conclution: Ruby passes by reference value

  We can say that ruby is an exclusively pass value by reference language. But there is one point worth mentioning. That is the **assignment\(=\)** operation. In most purely pass by reference languages, the assignment operation is a mutating method. However, in Ruby, it is not. Why? Because the variables and constant in Ruby are not actually object, they are references to acutal object. When we assign an actual value or object to a variable in ruby, we are actually just to rebound the variable to a new object.

  Since pass by value passes copies of arguments into a method, ruby appears to be making copies of the references, then passing those copies to the method. The method can use the references to modify the referenced object, but since the reference itself is a copy, the original reference cannot be changed


## Final method model

  Based on above coclusions, there are actually three answers to the question of what object passing strategy ruby uses:
  
  1. **Pass by reference value** is probably the most accurate answer, but it is a hard anser to swallow when learning ruby, and it is not particularly helpful when trying to decide what will happen if a method modifies an argument - at least not until you fully understand it.

  2. **Pass by reference** is accurate so long as you account for assignment and immutability

  3. Ruby acts like **pass by value** for immutable objects, **pass by reference** for mutable objects is a reasonable answer when learning about ruby, so long as you keep in mind that ruby only appears to act like this.


