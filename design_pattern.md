# _**DESIGN PATTERNS**_

## _**INTRODUCTION :**_
**Design Patterns**  are typical solutions to commonly occurring problems in software design. They are like pre-made blueprints that we can customize to solve a recurring design problem in our code. Design Patterns differ by their complexity, level of detail and scale of applicability to the entire system which is being designed.  

##  _**CLASSIFICATION OF DESIGN PATTERNS:**_

Design Patterns can be categorized based on their intent. Those are
  * **Creational Patterns:** It provides object creation mechanism that increases flexibility and reuse of code.
  * **Structural Patterns:** It explains how to assemble objects and classes into large structures while keeping the structures flexible and efficient.
  * **Behavioural Patterns:** It takes care of effective communication and the assignment of responsibilities between objects.
  
There are different types of Design Patterns under these categories. 5 different types among them are:
 1. Singleton
 2. Factor Method
 3. Abstract Factory
 4. Builder
 5. Adapter
   
### _**Singleton design pattern :**_
**Singleton Design Pattern** comes under creational design pattern.

According to the Gof, the intent of this  pattern is to :
> Ensure that class has one object, and provide a global point of access to it.

[Structure of Singleton Design pttern](https://refactoring.guru/images/patterns/diagrams/singleton/structure-en.png)

This design pattern creates only single instance which is accessible to multiple clients. examples, where this scenario is required are like file system, system manager, etc.
### **Code:**
```ruby
# The Singleton class defines the `instance` method that lets clients access the unique singleton instance.
class Singleton
  @instance = new

  private_class_method :new

  # The static method that controls the access to the singleton instance.
  #
  # This implementation lets you subclass the Singleton class while keeping just one instance of each subclass around.
  def self.instance
    @instance
  end

  # Finally, any singleton should define some business logic, which can be
  # executed on its instance.
  def some_business_logic
    # ...
  end
end

# The client code.

s1 = Singleton.instance
s2 = Singleton.instance

if s1.equal?(s2)
  print 'Singleton works, both variables contain the same instance.'
else
  print 'Singleton failed, variables contain different instances.'
end

```
### **Output :** 
```
Singleton works, both variables contain the same instance.
```

### _**Factor Method Design Pattern :**_
**Factory pattern**  is the most used Creational Pattern in the OOP world because it saves a lot of time in the future when there is a need to modify your existing classes.

According to GoF, the intent of this pattern is to,
>Define an interface for creating objects, but lets subclasses decide which class to instantiate and refers to the newly created object through a common interface.

**Key points:**
* Creat a object without revealing the creation logic to the client and refer the newly created object using a common interface.
* To get an object of similar type dynamically, based on a parameter we passed.
* If we create a object by using new operator inside the whole application, and if due to some reason like object creation configuration parameter are changed, then it is very hard to perform changes in every place and in such scenario factory method play a vital role to avoid dependencies.

[Structure of Factory method Design pattern](https://bogdanvlviv.com/images/posts/ruby/patterns/design-patterns-in-ruby/factory-method_pattern.png)

### **Code:**
```ruby
class Animal
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def eat
  end

  def speak
  end

  def sleep
  end
end

class Duck < Animal
  def eat
    puts "Duck #{name} is eating."
  end

  def speak
    puts "Duck #{name} says Quack!"
  end

  def sleep
    puts "Duck #{name} sleeps quietly."
  end
end


class Frog < Animal
  def eat
    puts "Frog #{name} is eating."
  end

  def speak
    puts "Frog #{name} says Crooooaaaak!"
  end

  def sleep
    puts "Frog #{name} doesn't sleep, he croaks all night!"
  end
end

class Plant
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def grow
  end
end

class Algae < Plant
  def grow
    puts "The Algae #{name} soaks up the sun and grows."
  end
end

class WaterLily < Plant
  def grow
    puts "The water lily #{name} floats, soaks up the sun and grows."
  end
end

class Pond
  def initialize(number_animals: 0, number_plants: 0)
    @animals = []
    number_animals.times do |i|
      animal = new_organism(:animal, "Animal#{i}")
      @animals << animal
    end

    @plants = []
    number_plants.times do |i|
      plant = new_organism(:plant, "Plant#{i}")
      @plants << plant
    end
  end

  def simulate_one_day
    @plants.each { |plant| plant.grow }
    @animals.each { |animal| animal.speak }
    @animals.each { |animal| animal.eat }
    @animals.each { |animal| animal.sleep }
  end
end

class DuckWaterLilyPond < Pond
  def new_organism(type, name)
    if type == :animal
      Duck.new(name)
    elsif type == :plant
      WaterLily.new(name)
    else
      raise "Unknown organism type: #{type}"
    end
  end
end

class FrogAlgaePond < Pond
  def new_organism(type, name)
    if type == :animal
      Frog.new(name)
    elsif type == :plant
      Algae.new(name)
    else
      raise "Unknown organism type: #{type}"
    end
  end
end

DuckWaterLilyPond.new(number_animals: 0, number_plants: 1)
```
### **Output***
```
The Algae Algae1 soaks up the sun and grows.
```

### _**Abstract Factory Design Pattern:**_

According to GoF, the intent of this pattern is to,
>Provide an interface for creating families of related or dependent objects without specifying their concrete class.

**Key points:**
* This design pattern is simply an extension of factory method pattern with one additional layer of abstraction,
* It is used to create more than one type of product
* It is best suitable when there is a need of multiple families of products. The key characteristic is that the pattern will decouple the concrete classes from the client.
* Abstract factory contains a set of factory classes having its own factory method, which is responsible for creating the concrete product of related types. Therefore according to the client's request, Abstract factory returns factory classes, which contains factory method and client will get the concrete product through a factory class to perform a specific operation. 

[Structure of Abstract Factory design pattern](https://bogdanvlviv.com/images/posts/ruby/patterns/design-patterns-in-ruby/abstract-factory_pattern.png)

### **Code:**
```ruby
class Animal
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def eat
  end

  def speak
  end

  def sleep
  end
end

class Tiger < Animal
  def eat
    puts "Tiger #{name} is eating anything it wants."
  end

  def speak
    puts "Tiger #{name} Roars!"
  end

  def sleep
    puts "Tiger #{name} sleeps anywhere it wants."
  end
end

class Plant
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def grow
  end
end

class Tree < Plant
  def grow
    puts "The tree #{name} grows tall."
  end
end

class Habitat
  def initialize(organism_factory, number_animals: 0, number_plants: 0)
    @organism_factory = organism_factory

    @animals = []
    number_animals.times do |i|
      animal = @organism_factory.new_animal("Animal#{i}")
      @animals << animal
    end

    @plants = []
    number_plants.times do |i|
      plant  = @organism_factory.new_plant("Plant#{i}")
      @plants << plant
    end
  end

  def simulate_one_day
    @plants.each { |plant| plant.grow }
    @animals.each { |animal| animal.speak }
    @animals.each { |animal| animal.eat }
    @animals.each { |animal| animal.sleep }
  end
end

class JungleOrganismFactory<Habitat
  def new_animal(name)
    Tiger.new(name)
  end

  def new_plant(name)
    Tree.new(name)
  end
end

class PondOrganismFactory<Habitat
  def new_animal(name)
    Frog.new(name)
  end

  def new_plant(name)
    Algae.new(name)
  end
end

jungle = Habitat.new(JungleOrganismFactory.new, number_animals: 0, number_plants: 1)
jungle.simulate_one_day
```
### **Output**

```
The tree tree1 grows tall.
```

### _**Builder Design Pattern:**_
According to GoF, the intent of this pattern is to,
>Separate the construction of a complex object from its representation so that the same construction processes can create different representations.

**Key points:**
* Builder pattern is a part of Creational  Pattern as it is helpfull in building the objects step by step.
* When we are creating the object of a particular class using the constructor, we have to remember all the parameters and it's right sequence. It will be problem when there are so many parameters and when we dont want to set all the parameter at the same time. At that time Builder pattern is used.
* Here, we will use setter for each parameter, where we are proving the object as input and getting the same object as output after setting the value.
* Final step will return the object by using GetProduct method.
* The process of constructing an object should be generic so that it can be used to create different representations of the same object.

[Structure of Builder Design pattern](https://bogdanvlviv.com/images/posts/ruby/patterns/design-patterns-in-ruby/builder_pattern.png)

### **Code:**
```ruby
class CPU
end

class BasicCPU < CPU
end

class TurboCPU < CPU
end

class Motherboard
  attr_accessor :cpu
  attr_accessor :memory_size

  def initialize(cpu=BasicCPU.new, memory_size=1024)
    @cpu = cpu
    @memory_size = memory_size
  end
end

class Drive
  attr_reader :type # either :cd, :dvd or :hard_disk
  attr_reader :size # in Mb
  attr_reader :writable # true if this drive is writable

  def initialize(type, size, writable)
    @type = type
    @size = size
    @writable = writable
  end
end

class Computer
  attr_accessor :display
  attr_accessor :motherboard
  attr_reader   :drives

  def initialize(display=:crt, motherboard=Motherboard.new, drives=[])
    @motherboard = motherboard
    @drives = drives
    @display = display
  end
end

class ComputerBuilder
  attr_reader :computer

  def initialize
    @computer = Computer.new
  end

  def basic_cpu
    computer.motherboard.cpu = BasicCPU.new
  end

  def turbo_cpu
    computer.motherboard.cpu = TurboCPU.new
  end

  def display=(display)
    computer.display = display
  end

  def memory_size=(size_in_mb)
    computer.motherboard.memory_size = size_in_mb
  end

  def add_cd(writer=false)
    computer.drives << Drive.new(:cd, 760, writer)
  end

  def add_dvd(writer=false)
    computer.drives << Drive.new(:dvd, 4000, writer)
  end

  def add_hard_disk(size_in_mb)
    computer.drives << Drive.new(:hard_disk, size_in_mb, true)
  end
end

builder = ComputerBuilder.new
builder.turbo_cpu
builder.add_hard_disk(1_000_000)
builder.memory_size = 16000
builder.add_cd(true)
builder.add_dvd
computer = builder.computer
```
### _**Adapter Design Pattern:**_
It is a Structural Pattern that allows objects with incompatible interfaces to collaborate.

**Key points:**
* It converts the interface of a class into another interface that  client's expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
* Wrap an existing class with a new interface.
* Impedance match an old component to a new system.

[Structure of Adapter pattern ](https://bogdanvlviv.com/images/posts/ruby/patterns/design-patterns-in-ruby/adapter_pattern.png)

### **Code**
```ruby
class Encrypter
  def initialize(key)
    @key = key
  end

  def encrypt(reader, writer)
    key_index = 0
    while not reader.eof?
      clear_char = reader.getc
      encrypted_char = clear_char.ord ^ @key[key_index].ord
      writer.putc(encrypted_char.chr)
      key_index = (key_index + 1) % @key.size
    end
  end
end

class StringIOAdapter
  def initialize(string)
    @string = string
    @position = 0
  end

  def getc
    if @position >= @string.length
      raise EOFError
    end
    ch = @string[@position]
    @position += 1
    return ch
  end

  def eof?
    return @position >= @string.length
  end
end

encrypter = Encrypter.new('SECRET_KEY')

reader = File.open('message.txt')
writer = File.open('message.encrypted','w')
encrypter.encrypt(reader, writer)

reader = StringIOAdapter.new('We attack at dawn')
writer = File.open('out.txt', 'w')
encrypter.encrypt(reader, writer)

```
## _**CONCLUSION:**_
These design patterns make software development easier by providing us with a tool box of solutions to common problems we encounter in object-oriented design. Based on every software or application and the problem we are facing while developing them, we have to select the particular design pattern.

## _**REFERENCES:**_
* https://bogdanvlviv.com/posts/ruby/patterns/design-patterns-in-ruby.html
* https://refactoring.guru/design-patterns/factory-method/ruby/example#lang-features
* https://sourcemaking.com/design_patterns
* https://youtu.be/mOajJ5LnLFE