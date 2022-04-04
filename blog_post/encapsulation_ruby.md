## Encapsulation in Ruby 

Ruby is an object oriented programming language which works on the principle that almost everything is an object wherein objects are instances of classes. 

Here, using IRB (interactive ruby shell) when you call the class method on a string, it returns its class, which is String.
```ruby=
$ irb
> "this is a string".class
=> String
```

This means that this string is an instance of the String class object and therefore inherits its methods. You can view all the methods available on a Class by calling `.methods.sort` which will return a sorted Array with all the method names that the object has.

```ruby=
$ irb
> "this is a string".methods.sort
=> [:unicode_normalized?, :unicode_normalize,...]
```

A `Class` can hold its own state and can contain methods which can be used to manipulate that state.Think of methods as the objects behaviour.  This initial state is created every time the object is instantiated when calling `new` on a class via the in-built `initialize` method. This state is then accessible only within that class.

```ruby=
class Board
    def initialize
      @grid = %w[1 2 3 4 5 6 7 8 9]
    end
end
```

When a new instance of this class is created it will create an initial state of grid. 

```ruby=
board = Board.new 
# <Board:0x00000001067b4600 @grid=["1", "2", "3", "4", "5", "6", "7", "8"...
```
Now if we wanted to amend the state of the grid, say re-assign its value, at this current moment we would not be able to do so. This is because  each object encapsulates its data which is private by default but exposes it's behaviour through it's public methods. The objects data and methods are available only within it's scope, object scope, whereby methods have their own scope. 

> encapsulate *verb*
> : to enclose in or as if in a capsule

In order to access or read a class's instance variable outside of it's class we need to pass it into a method, namely a getter method.    

```ruby=
def grid
  @grid
end
```
Now if we were to call on the grid outside of it's class we would be able to read it's contents.

```ruby=
grid = board.grid
p grid #  ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
```
In order to be able to amend the state of grid we would need a setter method.

```ruby=
def grid=(grid)
 @grid = grid
end
```
In ruby it is not necessary to write these getter and setter methods explicitly since it has in-built methods, attribute reader & writer which will generate them automatically.Furthermore it has a method which allow for both read & write permissions; attribute accessor.   

The word attribute is used because an object's instance variables are seen as it's attributes, the things that set it apart from other objects of the same class. 

Before you allow access to an object's private data it is important to think of the implications of other classes having access and the ability to amend data.For this can have unforeseen consequences and conflict with object oriented design principles, where classes know to much about each other. 

## attr_write, attr_reader, attr_accessor

In order to utilse this shortcut ruby provides, simply call the method at the start of the class along with the name of the instance variable. It is important to use a Symbol(`:`) and not a String (`''`) as they are immutable and require less memory as they only have once instance. However if you do use strings ruby will convert them into symbols.
```ruby=
class Board
  attr_accessor :grid
    def initialize
      @grid = %w[1 2 3 4 5 6 7 8 9]
    end
end
```

To summarize
- `attr_reader` can read the value but not change it
- `att_writer` can over write the value but not read it
- `att_accessor` can read and write the value. 

You can define multiple permissions in the same object. 

```ruby=
class Player
  attr_reader :marker
  attr_writer :name
    def initialize(marker, name)
      @marker = marker
      @name = name
    end
end
```

## `private`

As default all a classes methods are public and so accessible by other classes. However you can make them private, so that they are only called inside the class where they are defined using the `private` keyword. When using this keyword it is important to note where you place it, as all method declared beneath it will become private methods. 

```ruby=
class Board
  attr_accessor :grid
    def initialize
      @grid = %w[1 2 3 4 5 6 7 8 9]
    end
  
  private
    def print_grid
      puts grid
    end
end

board = Board.new
board.print_grid 

# NoMethodError: private method 'print_grid'
```

A `NoMethodError` is thrown whenever a private method is called. 

An advantage of encapsulation is data hiding which allows the class more control over it's attributes and methods which inturn make it more secure and less likely to create bugs. Another advantage is that as the application grows the code base is more maintainable and new changes can be easy implemented. It is also easier to unit test encapsulated code and testing ensures that the application is running as it should be. 

### References
- [RubyDocs - attr_accessor](https://ruby-doc.org/core-2.4.0/Module.html#method-i-attr_accessor)
-[RubyMontas Ruby for Beginners](https://ruby-for-beginners.rubymonstas.org/objects.html)
- [GeeksForGeeks - Encapsualtion](https://www.geeksforgeeks.org/ruby-encapsulation/)
- [Learn how to program](https://www.learnhowtoprogram.com/ruby-and-rails/test-driven-development-with-ruby/encapsulation)
- [RubyGuides - How to use attr_accessor, attr_writer & attr_reader ](https://www.rubyguides.com/2018/11/attr_accessor/)