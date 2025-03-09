---
layout: post
permalink: http://blog.stangroome.com/?p=92
title: Beware The Evil Shadow
description: None
date: 2007-06-20 13:26:23 -0000
last_modified_at: 2007-06-20 13:26:23 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

[![Evil Shadow](http://www.codeassassin.com/blog/content/binary/WindowsLiveWriter/BewareTheEvilShadow_13887/evilshadow_1.jpg)](http://www.flickr.com/photos/herby_fr/51491971/) Object-oriented programming principles have been an excellent addition to software development but one component that inevitably appears in OO languages has confounded me by it's uselessness. In VB.NET it is known as the "Shadows" modifier, in C# it is found in class member declarations sharing the name "new".

The purpose of this modifier keyword is to allow a subclass to redefine a property, field, or method of the base class where the base class has not been declared "Overridable" or "virtual" in VB.NET or C# respectively. Unfortunately it suffers from a major drawback, as demonstrated by the following code:

class Base  
{  
   virtual public string Extendable() { return "Extend"; }  
   public string Locked() { return "Lock"; }  
}  
  
class Foo : Base  
{  
   override public string Extendable() { return "Food"; }  
   new public string Locked() { return "Picked"; }  
}  
  
Base b = new Base();  
b.Extendable(); // returns "Extend"  
b.Locked(); // returns "Lock"  

Foo f = new Foo();  
f.Extendable(); // returns "Food"  
f.Locked(); // returns "Picked"  

Base bf = new Foo();  
bf.Extendable(); // returns "Food"  
bf.Locked(); // returns "Lock" !!!  

As you can see, if the Foo methods are called via a Foo-typed variable, there is no difference between shadowing and overriding. However, as soon as you call a Foo method via a Base-typed variable, or pass a Foo instance to another class expecting a Base, you lose the functionality of Foo.

If "new" is omitted from Foo's implementation of Locked, Visual Studio reports an error: _The keyword 'new' is required on 'Locked' because it hides method 'Base.Locked()'_. It is very easy for someone to simply add the keyword as required with realising the consequences.

Occasionally, all you might want to do is utilise some existing behaviour in a base class and add some more features and shadowing or member hiding is fine. The fact that you have used shadowing is not obvious in the calling code though.

Most of the time, I want to alter the behaviour of a base class and pass my new subclass to existing code, perhaps in the Framework, that is only expecting the base type. Beware.

]]>
