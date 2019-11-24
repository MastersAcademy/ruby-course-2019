
## Tips
```ruby
var1, var2 = ARGV                         # assign from array
var1, var2 = var2, var1                   # flip variables
var1, var2 = 'some string'                #=> var1 = 'some string', var2 = nil
var1, var2 = ['a', 'b', 'c']              #=> var1 = 'a', var2 = 'b'
var1, var2, *other = ['a', 'b', 'c', 'd'] #=> var1 = 'a', var2 = 'b', other = ['c', 'd']
```
```ruby
a = a + n # same as "a += n"
a = a - n # a -= n
a = a || n # a ||= n
a &&= n
a *= n
a /= n
```
```ruby
if cond1 && cond2
  'true logic'
else
  'false logic'
end

cond1 && cond2 ? 'true logic' : 'false logic'
```

```ruby
if cond1 || cond2
  'some logic'
end

'some logic' if cond1 || cond2
```
## Lesson
### Inheritance & Variables
All parent methods is accessible(for both: class & object)
```ruby
class Animal
  attr_reader :name
  def initialize(name)
    @name = name # this variable stored in each object of Animal/Mammal/Monkey separatelly
  end
end

class Mammal < Animal
  def formatted_name
    "#{@name.to_s[1].to_s.upcase}#{@name.to_s[1..-1].to_s.downcase}"
  end

  def self.new(*_attrs)
    new_animal = super
    @animals ||= [] # this variable stored in each class separatelly
    @animals << new_animal
    @@all_animals ||= [] # this variabled stored in all child classes as one
    @@all_animals << new_animal
    new_animal
  end

  def self.count
    @animals&.count || 0
  end

  def self.total_count
    @@all_animals&.count || 0
  end
end

class Monkey < Mammal
end

Animal.count             #=> NoMethodError (undefined method `count' for Animal:Class)
Animal.total_count       #=> NoMethodError (undefined method `total_count' for Animal:Class)
Mammal.count             #=> 0
Mammal.total_count       #=> 0
Mammal.new('test')
Mammal.count             #=> 1
Mammal.total_count       #=> 1
Monkey.count             #=> 0
Monkey.total_count       #=> 1 < here is one Mammal
a = Animal.new('jane')   #=> #<Animal:0x000056383082d3e0 @name="jane">
a.name                   #=> 'jane'
a.formatted_name         #=> NoMethodError (undefined method `formatted_name' for #<Animal:0x000056383082d3e0 @name="jane">)
m = Mammal.new('john')   #=> #<Mammal:0x0000562e33338210 @name="John">
m.formatted_name         #=> 'John'
m2 = Monkey.new('johny') #=> #<Monkey:0x0000563830822580 @name="johny">
m2.formatted_name        #=> 'Johny'
```
### Include
Adds methods to object of current class
```ruby
module PrettyName
  def formatted_name
    "#{@name.to_s[0].to_s.upcase}#{@name.to_s[1..-1].to_s.downcase}"
  end
end

class Animal
  include PrettyName
  def initialize(name)
    @name = name
  end
end

Animal.new('john').formatted_name #=> 'John'
```
### Extend
Adds methods to current object
```ruby
module Counter
  def count
    @animals&.count || 0
  end
end

module PrettyName
  def formatted_name
    "#{@name.to_s[0].to_s.upcase}#{@name.to_s[1..-1].to_s.downcase}"
  end
end

class Animal
  extend Counter
  def initialize(name)
    @name = name
    extend PrettyName
  end

  def self.new(*_attrs)
    new_animal = super
    @animals ||= []
    @animals << new_animal
    new_animal
  end
end

3.times { Animal.new }
Animal.count #=> 3
```
### Instance Eval
Executes code in current object
```ruby
class Animal
  # code for examples
  def initialize(name)
    @name = name # this variable stored in each Animal.new
  end

  def self.new(*_attrs)
    new_object = super
    @animals ||= [] # this variable stores in class Animal
    @animals << new_object
    new_object
  end
  # === #
end

Animal.count #=> NoMethodError (undefined method `count' for Animal:Class)
Animal.instance_eval do
  # class method
  def count
    @animals&.count || 0
  end
end
Animal.count #=> 0

a = Animal.new('john doe')
a.instance_eval do
  # instance_method only for current object
  def formatted_name
    "#{@name.to_s[0].to_s.upcase}#{@name.to_s[1..-1].to_s.downcase}"
  end
end

a.formatted_name #=> 'John doe'

a2 = Animal.new('john doe #2')
a2.formatted_name #=> NoMethodError (undefined method `formatted_name' for #<Animal:0x000055c9f037c6d8>)
```
### Class Eval
Executes code as in class
```ruby
class Animal
  def initialize(name)
    @name = name
  end

  # class eval will be executed here
end

Anima.class_eval do
  def self.new(*_attrs)
    new_animal = super
    @animals ||= []
    @animals << new_animal
    new_animal
  end

  def self.count
    @animals&.count || 0
  end

  def formatted_name
    "#{@name.to_s[0].to_s.upcase}#{@name.to_s[1..-1].to_s.downcase}"
  end
end


Animal.count #=> 0
Animal.new('john').formatted_name #=> 'John'
Animal.count #=> 1
```

### Defining methods
define_singleton_method - adds method to current object
define_method - adds method to object of current class
```ruby
class Animal
  def initialize(name)
    @name = name
  end

  def self.new(*_attrs)
    new_animal = super
    @animals ||= []
    @animals << new_animal
    new_animal
  end
end

Animal.define_singleton_method(:count) { @animals&.count || 0 }
Animal.define_method(:formatted_name) do
  "#{@name.to_s[0].to_s.upcase}#{@name.to_s[1..-1].to_s.downcase}"
end

Animal.count #=> 0
a = Animal.new('johN')
a.formatted_name #=> 'John'
a.define_singleton_method(:formatted_name) { @name.to_s.downcase }
a.formatted_name #=> 'john'
a2 = Animal.new('johN #2')
a2.formatted_name #=> 'John #2'
```
### Eigenclass
possible deffinitions of same method
```ruby
class Animal
  def self.new(*_attrs)
    new_animal = super
    @animals ||= []
    @animals << new_animal
    new_animal
  end
  
  class << self
    def new(*_attrs) # without self
      new_animal = super
      @animals ||= []
      @animals << new_animal
      new_animal
    end
  end
end
```
### Method super priority
extend(on object) -> prepend(on class) -> object method -> include(on class) -> parent
any step can be automatically skipped if method on that step not defined
```ruby
module BlankExtend; end
module BlankPrepend; end
module BlankInclude; end

module Formatter1 # extend
  def name
    puts '1'
    super #-> to prepend
  end
end

module Formatter2 # prepend
  def name
    puts '2'
    super #-> to object
  end
end

module Formatter3 # include
  def name
    puts '4'
    super #-> to parent
  end
end

class Parent
  def name
    puts '5'
    @name
  end
end

class Child < Parent
  prepend Formatter2
  prepend BlankPrepend
  include Formatter3
  include BlankInclude
  
  def initialize(name)
    @name = name
    extend Formatter1
    extend BlankExtend
  end
  
  def name
    puts '3'
    super #-> to includes
  end
end

c = Child.new('john')
c.name #=> 1, 2, 3, 4, 5, 'john'
```
### Included, extended & prepended
If you want to do extra actions on include/extend/prepend
```ruby
module A
  def self.prepended(base)
    puts "#{base}-prepend"
  end

  def self.extended(base)
    puts "#{base}-extend"
  end

  def self.included(base)
    puts "#{base}-include"
  end
end

class TestClass
  prepend A
  extend A
  include A
end #=> TestClass-prepend, TestClass-extend, TestClass-include
```
So you can use one method to update your class
```ruby
module RecordsSaver
  def self.included(base)
    class << base
      prepend PrependClassMethods
    end
    base.include InstanceMethods
    base.extend ExtendClassMethods # extend on class has lower priority than class method & can be overwritten
  end
  
  module PrependClassMethods
    def new(*_attr)
      new_object = super
      @objects ||= []
      @objects << new_object
      new_object
    end
  end

  module ExtendClassMethods
    def count
      @objects&.count || 0
    end
  end

  module InstanceMethods
    def formatted_name
      "#{@name.to_s[0].to_s.upcase}#{@name.to_s[1..-1].to_s.downcase}"
    end
  end
end

class Animal
  include RecordsSaver

  def self.new(*_attrs)
    AnotherClass.new # even if we don't call super @objects will be updated cause we used prepend to update this method
  end
  
  def self.count
    (@objects&.count || 0) + 12 # can be easily overwritten because was added by extend
  end
 
  def pretty_name
    @name # also can be overwritte because was added by include
  end
end
```
