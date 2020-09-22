+++
title = "Javascript - \"this\" is awesome"
date = "2016-04-19T18:00:00+07:30"
tags = ["javascript", "this"]
draft = false
author = "Amitabh Das"
+++

For those of you coming from the Java, C++ side of programming, this keyword is bound to trip you sometime. In programming languages such as C++, you use it in context of classes and objects.

# Does that mean JavaScript has classes?

Yes and No. You can emulate classes in JavaScript but calling them classes is as good as calling tomato a vegetable. Although according ECMAScript 2015 (ES6) standards, classes have been introduced to JavaScript. But that’s just syntactic sugar over the prototype based inheritance.

# What is this?
this is a special keyword in JavaScript used in functions(or global scope) which is set to refer to the object calling that function. this keyword changes in every call and its behavior is different when strict mode is on. In strict mode, when global functions are called, this is undefined. If strict mode is off, this refers to the window object.

In short, this refers to the owner of the function in which it is used.

# Where to use this?
## Function in global namespace

Refers to the window variable in non-strict mode and undefined in strict mode.

```
function() {
    console.log(this); // refers to window
}
```

## Function in an object 

Refers to the object which owns the function.

```
var x = {
    testing: function() {
        console.log(this); // refers to variable x
    }
};
```

## As a constructor

When a function is called with the new keyword, and the function happens to use this keyword, a new object is returned with the modifications made to the object by the function using this keyword.

```
var Person = function(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
};

var john = new Person(“John”, “Locke”);
var ben = new Person(“Benjamin”, “Linus”);
```

# Where to be careful using this?

## Inside Closures

```
var example = {
    data: 42
    getData: function() {
        function mixer() {
            this.data += Math.random() * 22;
        }

        // Here function mixer is not called through any object. 
        // Therefore by default, this points to window
        mixer(); 
    }
};

example.getData();
```

In the function mixer, this refers to the window object and not the example object. Note that the function mixer is not called via the example object. A solution to this would be to save this variable outside the function mixer, in the getData function and then use it inside the mixer function.

## Passing object methods as callbacks

```
var example = {
    data: [],
    url: “https://www.google.com”,
    getData: function() {
        $.ajax({
            url: this.url,
            success: function(data) {
                this.data = data; 
            }
        });
    }
};

example.getData();
```

In the function passed as a success callback to jQuery ajax, this keyword will not refer to the  example object. this keyword is assigned as the object which calls the function. So when jQuery ajax calls the success handler, this keyword points to jQuery ajax. Again, solution is the same as in the previous.

# Conclusion:

Remeber **this**: `this` only points to the object calling the function. If you do, then you can easily wrap your head around the complications you create for yourself. This can be a powerful tool if you use it right.
