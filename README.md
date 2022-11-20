# Table Of Content
* [Introduction](#introduction)
* [Bad Code Smells](#bad-code-smells)
   * [Mysterious Name](#mysterious-name)
   * [Duplicated Code](#duplicated-code)
   * [Long Function](#long-function)
   * [Long Parameter List](#long-parameter-list)
   * [Global Data](#global-data)
   * [Mutable Data](#mutable-data)
   * [Divergent Change](#divergent-change)
   * [Shotgun Strategy](#shotgun-strategy)
   * [Feature Envy](#feature-envy)
   * [Data Clumps](#data-clumps)
   * [Primitive Obsession](#primitive-obsession)
   * [Repeated Switches](#repeated-switches)
   * [Loops](#loops)
   * [Lazy Element](#lazy-element)
   * [Speculative Generality](#speculative-generality)
   * [Temporary Field](#temporary-field)
   * [Message Chains](#message-chains)
   * [Middle Man](#middle-man)
   * [Insider Trading](#insider-trading)
   * [Large Class](#large-class)
   * [Alternative Classes with Different Interfaces](#alternative-classes-with-different-interfaces)
   * [Data Class](#data-class)
   * [Refused Bequest](#refused-bequest)
   * [Comments](#comments)
* [My Favourites](#my-favourites)
* [Trouble Grasping](#trouble-grasping)

# Introduction

You did your homework. You know how refactoring works and have learned every single refactoring method there is to know. But when should you use them? Just because you know how to do it, doesn't mean you know when to. Knowing when to start refactoring and when to stop is just as important as refactoring itself. So how would one know when the code needs some refactoring?

<img src="https://i.pinimg.com/originals/87/c5/81/87c5816966db8895b7c6b85e6b2edd41.gif" title="" alt="The wok smelling" data-align="center">

That's right, you smell it. Well, not exactly, that is just a metaphor for traces of problems in the code. Code smells are indication that the code is overdue for refactoring. I am going to touch a few of the code smells.

> If it stinks, change it.
> — Some Random Grandma, discussing when to change diapers

# Bad Code Smells

## Mysterious Name

Can't understand that a variable or a function is for just by it's name? Well that means
you gotta refactor it. Naming something is one of the hardest thing in programming.
Even outside programming, the thought of having the responsibility of naming my child
haunts me. Good thing I will have someone to share that responsibility then.

Change it up using *Rename Variable (137)* or *Rename Field (244)* or in the
case of method, *Change Function Declaration (124)*.

> There are only two hard things in Computer Science: *cache invalidation and naming things*. 
> 
> — Phil Karlton

## Duplicated Code

If you see same code structure at multiple places, you better unify it. If you don't then get ready to find differences carefully whenever you read it or to change a thing at every place if you want to change one thing.

Fix it by *Extract Function (106)*. If its duplicated in subclasses of common base class, *Pull Up Method (350)*. If its similar but not quite identical, use *Slide Statement (223)* to arrange for extraction.

## Long Function

Everybody hates long function except the one writing them. They are difficult to
maintain and understand. Please just break your code into smaller ones instead of
making one behemoth of a function. You might say, *"but but but what about function
calling overhead?"* Shut up okay. We are not living in an era with 16 MBs of ram or 100Mhz of cpu power or writing in assembly. Most modern languages have pretty much eliminated the overhead for in-process calls. A good rule of thumb is, if you feel the need to write a comment, try making a function instead.

The most obvious and common fix is *Extract Function (106)*. There are others too but I am not gonna talk about them, because I need this article to be a 5-10 minutes read.

## Long Parameter List

<img title="" src="https://media.makeameme.org/created/this-parameter-list.jpg" alt="" width="327" data-align="center">

In early days, we were taught to send everything a function needs as parameter. Alternative was global data, which is **really** bad. But long parameter list gets confusing.

Refactor is by using *Replace Parameter with Query (324)* to get second and other parameters using the first one, *Preserve Whole Object (319)* to send the whole data structure (not ideal), or *Introduce Parameter Object (140)*, I mean who doesn't like objects. If multiple functions are working on same parameters, use *Combine Functions into Class (144)*.

## Global Data

Remember when I said global data is bad. Its not just bad, it was invented by the demons of the deepest d̷̮̽e̶̻͗p̶̝̓ẗ̵͉h̶͉̊s̸̰̃ ŏ̵͇ḟ̴̝ h̵̟̏̏ë̵̘̳̺́l̷̗̊̚͠l̴̳̆. And those who use it, are destined to go there too. Global data is just bad, I mean what do you want me to say here. It can be modified from anywhere, no way to know what modified the data, no way to limit access to it. Its just bad. Okay maybe the global data which, you are **100%** sure, will not be modified is okay, like const.

Slap that *Encapsulate Variable (132)* on it.

## Mutable Data

Let me break it up for you, you have a variable. Something somewhere changed the data it contained. Something somewhere else wasn't expecting the change, and your code breaks. Not a big problem if the scope of variable is small, but if it's scope grows, so does it's risk.

*Encapsulate Variable (132)* to make sure all updates are done from specific functions. Use *Split Variable (240)* to keep the updates seperate from original variable. Move updating logic into their own functions by *Slide Statements (223)* and *Extract Function
(106)*. You can use *Combine Functions into Class (144)* or *Combine Functions into Transform (149)* to limit scope of variable. Or you can even use *Change Reference to Value (252)* to avoid updates to variable.

## Divergent Change

When you have to change exact same things when encountered with different scenarios or context but some of the things remains the same, you can seperate the same things into seperate modules.

Use *Split Phase (154)* to seperate the same with different, then employ a combination of  *Move Function (198)*, *Extract Function (106)* or *Extract Class (182)*.

## Shotgun Strategy

If everytime you make one change, you have to make many small little edits in a lot of different classes and places. That, my friends, is one nasty code.

Use *Move Function (198)* and *Move Field (207)* to put changes in single module. Use *Combine Functions into Class (144)* to combine functions working on same data. Use *Split Phase (154)* to seperate same and different phases of logic.

## Feature Envy

This happens when a function in one module spends an alarming amount of time using other module's functions, like invoking half-a-dozen getters to calculate a value. Obviously it wants to be with the data the other module has.

Move it there using *Move Function (198)*. If part of it envies, use *Extract Function (106)* and then *Move Function (198)* 

Some design pattern reeks of this smell, but sometimes you have to compromise.

## Data Clumps

Some data items are like friends. You always find them hanging around together. They are called Data Clumps. 

Sack them up in a class using *Extract Class (182)*. Use *Introduce Parameter Object (140)* or *Preserve Whole Object (319)* for easily passing them to function

## Primitive Obsession

Everybody loves primitives. ints, floats, strings. I mean they are basic building blocks how can you not love them. But sometimes you need to wrap them up in custom fundamental objects/classes for easier calculating, displaying, type checking, basically everything. For example, don't use a string to store a telephone number, but use a TelephoneNumber class.

Use *Replace Primitive with Object (174)*. If there are groups of primitives that commonly appear together, the code smell above this one would help.

## Repeated Switches

Basically, switch bad, polymorphism good. Some even say, ifs also bad but I like ifs, I am not giving them up just yet.

<img title="" src="https://i.redd.it/uu5p198w4z521.jpg" alt="" data-align="center" width="340">

Use *Replace Conditional with Polymorphism (272)*.

## Loops

<img title="" src="https://i.redd.it/zkzpljazr2951.jpg" alt="" width="378" data-align="center">

Don't get me wrong, loops are essential. But on some specific loops we can use *Replace Loop with Pipeline (231)*. For example, filter, map, any, every. It just makes the code more readable.

## Lazy Element

Adding functions and classes is good, but not when they are just added for the sake of it. If you see very simple function or class with only single function, use *Inline Function (115)* or *Inline Class (186)*. With inheritence, use *Collapse Hierarchy (380)*.

## Speculative Generality

Sometimes, people write codes thinking they would need it to do this kind of thing someday. But that day either never comes or is very, very far away. So we should just get rid of the generality.

Use *Collapse Hierarchy (380)* to get rid of abstract classes that are useless. *Inline Function (115)* or *Inline Class (186)* to remove unecessary delegation. *Change Function Declaration (124)* to remove unused or unneeded parameters.

## Temporary Field

If a class has a field which is set only in certain circumstances. Such code is difficult to understand.

Use *Extract Class (182)* to encapsulate orphan variables, and use *Move Function (198)* to put all the concerning code in the new class. You can also use *Introduce Special Case (289)* to make class which are used when variables aren't valid.

## Message Chains

When the client asks for an object, then asks that object for another, then that for another. It is a message chain. Any change in the chain will require client code to change.

Use *Hide Delegate (189)*. But that might just turn the message chain into chain of middleman. Alternative is to use *Extract Function (106)* on the code which uses resulting object and then *Move Function (198)* to push it down the chain.

## Middle Man

One of the pillars of OOP is encapsulation. But with encapsulation, comes delegations. which is fine. But it can go out of control very soon.

If you see a half a class delegating to other class, use *Remove Middle Man (192)* and talk to the object directly.

## Insider Trading

Programmers don't like trading data around between modules. But some trading has to occur. We need to reduce it to a minimum.

Use *Move Function (198)* and *Move Field (207)* if you see two modules talking a lot. If they have common interest, make a third module to keep that commonality between them, or use *Hide Delegate (189)* to make another intermediary module.

## Large Class

If your class is too big, and trying to do too much, just chop it down.

You can *Extract Class (182)* or *Extract Superclass (375)* or *Replace Type Code with Subclasses (362)*

## Alternative Classes with Different Interfaces

Classes are good for polymorphism. Changing classes in times of need. But that won't work if alternative classes have different interfaces.

Use *Change Function Declaration (124)* to make functions match up. Keep using *Move Function (198)* to move behaviour into class until protocol matches. If duplication arises, use *Extract Superclass (375)*.

## Data Class

Class only has data, getter, setters and no behaviour? That is a data class.

Apply *Encapsulate Record (162)* on it. And *Remove Setting Method (331)* on fields that shouldn't change. Try to *Extract Function (106)* and *Move Function (198)* to the piece of codes using those getters and setters and try to get them in the data class.

## Refused Bequest

Subclasses are getting methods and data from the superclass it doesn't need or what? Well, that means most probably the hierarchy is wrong.

Use *Push Down Method (359)* and *Push Down Field (361)* to push unused code to the sibling. That way the parent hold only whats common.

## Comments

Comments aren't bad. Infact, I call them good smell. But it is mentioned here because comments are generally used to put deodrant over the smelly code. If you need to explain in a thick comment what a piece of code does. Its probably not a good piece of code.

Just inpect the code, find the smell, employ the corresponding refactoring.



# My Favourites

My favourite smells from these gotta be Repeated Switches, since I myself, before reading the book, never thought of switch statements that way. Like how they can be substituted with polymorphism. It does require making classes and interfaces, but it is an elegant method of avoiding switch statements.

Another one of my favourites is Comments, cause hey, who doesn't like comments. I always thought of commented code as good code, but now I know how comments, not being a smell itself, tries to hide other smells.

# Trouble Grasping

Some smell's solutions were difficult to understand, like Message Chaining.
