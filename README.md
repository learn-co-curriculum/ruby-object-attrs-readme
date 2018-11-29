# Object Attributes and Methods: A Deeper Dive

## Objectives

1. Use methods to abstract or wrap the attributes of an object.
2. Explain the difference between a setter and a getter method.
3. Build and use setter and getter methods

## Introduction

So far, we've learned how to build classes and even how to give our classes instance methods. Now, we can add the special method `initialize`, which will require certain arguments to be passed when instantiating the class to provide it with initial data. In this example, our Person class has an instance method, `#name`, that is set each time a new Person class is created. This `name` method can be called on an instance of Person (an individual person object) and return that person's name.

```ruby
class Person
  def initialize(name)
    @name = name
  end

  def name
    @name
  end
end

kanye = Person.new("Kanye")
kanye.name #=> "Kanye"

```

But wait! Kanye has decided he wants to be referred to as "Yeezy". Kanye is a huge star so we should probably do what he says. However, as it currently stands, we don't have a way to re-assign Kanye's name. Let's see what happens when we try:

```ruby
kanye.name = "Yeezy" 
#=> NoMethodError: undefined method `name=' for #<Person:0x007fd7cbb0f3d8>
```

We get a no method error! Kanye's name change is just one example of the many common situations in which we might want to alter the information or attributes associated with a given object. Let's move on to the next section to learn how to add that functionality to our classes.

## Setter vs. Getter Methods

Our Person class' `#name` method is referred to as a **"getter"** or reader method. It returns information stored in an instance variable. In order to make a person's name attribute writable, we need to define a **"setter"** or writer method.

### Defining a Setter Method

```ruby
class Person

  def initialize(name)
    @name = name
  end

  def name
    @name
  end

  def name=(new_name)
    @name = new_name
  end

end
```

A setter method is defined with an `=`, equals sign, appended to the name of the method. The `=` is followed by the `(argument_name)`. Now that we've defined our setter method on the Person class, we can change Kanye's name.

### Calling a Setter Method

To call a setter method, you use the `.` notation (dot notation) to call the method and set it equal to a new value.


```ruby
kanye = Person.new("Kanye")

kanye.name
  => "Kanye"

kanye.name = "Yeezy"
kanye.name
  => "Yeezy"
```
Let's break it down. We:

* Instantiate a Person instance and name him "Kanye".
* Call our getter method, `#name` to return his name, `"Kanye"`
* Call our setter method `#name=` to change his name to "Yeezy"
* Call our getter method again and see that `kanye`'s name is now `"Yeezy"`.

You can also call a setter method like this:

```ruby
kanye.name=("Yeezy")
```

But we prefer the first notation.

## The Abstraction of Instance Methods

In Ruby, it is possible to simply set an instance variable with the following method:

```ruby
kanye.instance_variable_set(:@name, "Yeezy")
```

It is also possible to simply retrieve an instance variable from an object with the following method:

```ruby
kanye.instance_variable_get(:@name)
  => "Yeezy"
```

**Since this is the case, why do we even use instance setter and getter methods?** In fact, there are a number of reasons:

### Syntactic Vinegar vs. Syntactic Sugar

The first reason is a stylistic one but it is important. As object-oriented Rubyists, we care about our program's readability and design. The above method is ugly. It places a verb at the end of the method name. The weird grammar of this method should remind us not to use it.  (This technique of purposely naming methods in a hard to use manner is called syntactic vinegar.)  

### Exposing Literal Variables vs. Abstracting Attributes

The `instance_variable_set` method depends on a literal, concrete variable, `@name`. It exposes it directly to the person executing our code. This is bad practice because it forces our program to rely directly on the `@name` variable. Why is this so terrible? Let's take a look at the following use case:

For example, Kanye (who by the way has commissioned you to write this amazing Person class program) has decided that our program should store both a first and last name. Let's do a quick refactor of our Person class.

We'll initialize our `Person` instances with both a first and last name.

```ruby
class Person

  def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end

  #...

end
```

With this change, our program does more than just make Kanye happy. It has some added functionalty. We could imagine collecting all of our instances of `Person` and sorting them by last name, for example.

BUT, now, any other part of our program that was calling `instance_variable_get(:@name)` is broken! Additionally, any part of our program that is calling `instance_variable_set(:@name)` isn't taking advantage of our new first name and last name functionality. Any attempt to change a person's name with `instance_variable_set(:@name)` wouldn't *really* change their name, because it wouldn't touch the `@first_name` and `@last_name`variables set with our `initialize` method. It would just give them a `@name` variable set to a different value than the `@first_name` and `@last_name` variables. That would get confusing, fast.

Allowing our code to rely on an instance variable directly created a program that *is not flexible*. If our program contains multiple occurrences of `instance_variable_get(:@name)` and `instance_variable_set(:@name)`, we would have to hunt down each and every one and change them to accommodate our shift to using both a first and a last name.

Instead of writing code that depends on instance (or any type of) variables, we write *methods* that contain instance variables. This is a form of abstraction, whereas the instance variable `@name` is a literal value. The literal value reference, the variable `@name`, may change as our application grows and we want our application to seamlessly accommodate that change.

Let's create our abstraction: the `#name=` and `#name` setter and getter instance methods:

```ruby
class Person

  def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end

  def name=(full_name)
    first_name, last_name = full_name.split
    @first_name = first_name
    @last_name = last_name
  end

  def name
    "#{@first_name} #{@last_name}".strip
  end

end
```

**Now, even if the content of the `#name` method changes, for example, Kanye changes his mind again and wants to be referred to only as "Yeezy" (using our interface this change would be `kanye.name = "Yeezy"`), the interface, how our application uses that content, remains constant.** In other words, we can change the content of these methods according to our needs, without needing to hunt down every appearance of them in our program and change them as well, like we would need to do with our `instance_method_set` and `instance_method_get` usages.

## Coming Up

In the following lab, you'll be defining your own class and setter and getter methods. Then, we'll discuss yet another level of abstraction dealing with these method types.

<p class='util--hide'>View <a href='https://learn.co/lessons/ruby-object-attrs-readme'>Object Attributes Readme</a> on Learn.co and start learning to code for free.</p>

<p class='util--hide'>View <a href='https://learn.co/lessons/ruby-object-attrs-readme'>Object Attributes Readme</a> on Learn.co and start learning to code for free.</p>

<p class='util--hide'>View <a href='https://learn.co/lessons/ruby-object-attrs-readme'>Object Attributes</a> on Learn.co and start learning to code for free.</p>
