
thaiWitter Class System
=======================

The class system made for #thaiWitter. Features:

- Creating classes that inherits from other class (only instance methods / variables are inherited).
- Override any function in any class (monkey-patching).
- Allows calling of overridden method (both with inheritance and monkey-patching).


Usage
=====

Creating a new class
--------------------

Inherits from the base class:

    var ClassA = new Class();


Adding class methods
--------------------

    ClassA.implement({
        hello: function() {
            alert("Hello!");
        }
    });

    ClassA.hello(); // alerts Hello!


Adding instance methods
-----------------------

    ClassA.prototype.implement({
        init: function(name) {
            this.name = name; // function!
        },
        getName: function() {
            return this.name + '';
        },
        hello: function() {
            alert("Hello, " + this.getName());
        }
    });

    var a = new ClassA('World');
    a.hello(); // alerts Hello, World

__Note:__ `.implement` can be chained, so you can do like:

    ClassA.implement({ ... }).prototype.implement({ ... });

__Also Note:__ `.init` is the name of the constructor function.


Creating a subclass
-------------------

    var ClassB = new Class(ClassA);

__Note:__ Class methods and static variables are not inherited. Only members in the prototype will be inherited.


Calling the overridden function
-------------------------------

When you want to call the overriden function, use `arguments.callee._super.call(this, [your arguments here])`

    ClassB.prototype.implement({
        getName: function() {
            return arguments.callee._super.call(this) + '!!';
        },
        hello: function() {
            alert("Hi, " + this.getName());
        }
    });

    var b = new ClassB('undefined');
    b.hello(); // alerts Hi, undefined!!


Monkey-patching
---------------

Just do the same, and you can also call the overridden method the same way. This also affect all the subclasses when they call the overridden function. 

    ClassA.prototype.implement({
        getName: function() {
            return arguments.callee._super.call(this).toUpperCase();
        }
    });

    a.hello(); // alerts Hello, WORLD
    b.hello(); // alerts Hi, UNDEFINED!!



