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

## Ruby **seems** passing value by value

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

## Ruby **seems** passing value by reference, too

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


## 
