---
title: "Improving inheritance hierarchy in Ruby"
---

Classical inheritance is one of the most common code-sharing techniques. Every CS graduate knows it. It's also easier to shoot yourself into the foot while implementing it and making the code harder to modify and reuse. I was recently going through [Practical Object-Oriented Design, An Agile Primer Using Ruby](https://www.poodr.com/) which discusses it in depth. I'll explain a few techniques I learned from the book that are easy to put into practice.

Consider this somewhat contrived example:
```ruby
class Animal
  def eat
    'Eating...'
  end

  def characteristics
    [:legs, :tail]
  end
end


class Dog < Animal
  def speak
    'Woof!'
  end
end

class Cow < Animal
  def speak
    'Moo!'
  end

  def characteristics
    super + [:horns]
  end
end
```
`Animal` is an abstract superclass; `Cow` and `Dog` are concrete subclasses. Each subclass of `Animal` implements its version of `speak`. Subclasses can add additional characteristics that are specific to them.   

## Template method pattern

`Animal` imposes a requirement upon its subclasses that is not obvious by looking at it - every subclass must implement `speak`. To improve this, the superclass - `Animal` must provide an implementation for every message it expects as part of the common contract. Even if the only reasonable implementation is raising an exception. 

```ruby
class Animal
  # ...

  def speak
    raise NotImplementedError
  end
end
```

## Hook Messages

Subclasses of `Animal` can add characteristics that are specific to that animal by appending them to `Animal#characteristics`. Thus all the subclasses need to be aware of the algorithm to add subclass-specific characteristics. If that algorithm changes, all subclasses will have to be updated even though their specializations are not affected. Also, if a subclass forgets to call super, the resulting characteristics are wrong and this wrongness may not be apparent immediately since nothing breaks. This can be fixed using hook messages.

```ruby
class Animal
  # ...

  def characteristics
    [:legs, :tail] + additional_characteristics
  end

  def additional_characteristics
    []
  end
end

class Cow < Animal
  # ...

  def additional_characteristics
    [:horns]
  end
end
```


## Testing Inheritance Code

The first goal of testing is to prove that all objects in this hierarchy honor their contract. **The Liskov Substitution Principle** declares that subtypes should be substitutable for their supertypes. Violations of Liskov result in unreliable objects that don’t behave as expected. The easiest way to prove that every object in the hierarchy obeys Liskov is to write a shared test for the common contract and include this test in every object.


```ruby
module AnimalInterfaceTest
  def test_responds_to_eat
    assert_respond_to(@object, :eat)
  end

  def test_responds_to_speak
    assert_respond_to(@object, :speak)
  end

  def test_responds_to_characteristics
    assert_respond_to(@object, :characteristics)
  end
end


class AnimalTest < MiniTest::Unit::TestCase
  include AnimalInterfaceTest

  def setup
    @object = Animal.new
  end
end

class DogTest < MiniTest::Unit::TestCase
  include AnimalInterfaceTest

  def setup
    @object = Dog.new
  end
end

class CowTest < MiniTest::Unit::TestCase
  include AnimalInterfaceTest

  def setup
    @object = Cow.new
  end
end
```
<br/>

Because there are many subclasses, they should share a common test to prove that each meets the requirements:
```ruby
module AnimalSubclassTest
  def test_responds_to_additional_characteristics
    assert_respond_to(@object, :additional_characteristics)
  end

  def test_responds_to_speak
    assert_respond_to(@object, :speak)
  end
end

class DogTest < MiniTest::Unit::TestCase
  include AnimalInterfaceTest
  include AnimalSubclassTest

  # ...
end

class CowTest < MiniTest::Unit::TestCase
  include AnimalInterfaceTest
  include AnimalSubclassTest

  # ...
end
```
<br/>

In addition to the interface tests, we should test behaviours that are specific to superclass and subclasses in their respective tests:
```ruby
class AnimalTest < MiniTest::Unit::TestCase
  # ...

  def setup
    @animal = @object = Animal.new
  end

  def test_forces_subclasses_to_implement_speak
    assert_raises(NotImplementedError) { @animal.speak }
  end
end


class CowTest < MiniTest::Unit::TestCase
  # ...

  def setup
    @cow = @object = Cow.new
  end

  def test_adds_additional_characteristics
    assert_includes @cow.characteristics, :horns
  end
end
```

That's all for this post. This is a small subset of all the inheritance-related things covered in the book. Do read [the book](https://www.poodr.com/) to understand inheritance and other OO design techniques in depth; and take your Ruby OO chops to the next level.
