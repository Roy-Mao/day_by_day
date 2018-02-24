# studynote02/24

## What is s a instance variable? And what it has to do with the attribute accessors?

Instance variable is the instance attributes. It should be distinct to each object, and they are the things that make it differnt from other objects of the same class. In order to write to or read from these distinct attributes, we need attribte accessors.

## How to write getter and setter in ruby?

```ruby
  class Fruit
    def set_kind(k)
      @kind = k
    end
    def get_kind
      @kind
    end
  end 
```

## Can we make the setter and getter above more concise?

```ruby
  class Fruit
    def kind=(k)
      @kind = k
    end
    def kind
      @kind
    end
  end
```

## Attribute accessor: shortcut for getter and setter method

|Shortcut|Effect|
|:-|:-:|
|attr_reader :v|def v; @v; end|
|attr_writer :v|def v=(value);@v = value; end|
|attr_accessor :v|attr_reader :v; attr_writer :v;|
|attr_accessor :v, :w|attr_accessor :v; attr_accessor :w|