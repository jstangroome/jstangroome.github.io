---
layout: post
permalink: http://blog.stangroome.com/?p=69
title: Serialization Engine Wish List
description: None
date: 2007-07-30 03:32:27 -0000
last_modified_at: 2007-07-30 03:32:27 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

[![Cereal](http://www.codeassassin.com/blog/content/binary/WindowsLiveWriter/SerializationEngineWishList_AF65/cereal_1.jpg)](http://www.flickr.com/photos/stevenwilke/375214502/) On several occasions I have need to serialize an object graph in memory out to a stream and also needed to read XML data back into objects. Each time I'm confronted by such a task I find myself hunting through the various MSDN serialization topics and becoming increasingly depressed.

Neither the System.Runtime.Serialization namespace nor the System.Xml.Serialization namespace seem to fit my requirements without needing to write large chunks of custom serialization code. It's getting to the point that I'm tempted to write my own serialization engine.

I know that if I do write my own engine, it will consume most of my time, so for now I'm simply going to document what I'd like to see in said engine.

  * Classes should not require a parameter-less constructor to be deserialized. As a simple alternative a class could define a static method which takes a collection of key value pairs from the engine, creates an instance via an appropriate constructor and returns it back to the engine. Ideally the engine would support declarative attributes on an existing constructor defining how to call it.
  * Classes should not need to override their entire serialization process because the engine does not understand one of it's field types. If my class has four integer fields, two string fields, and a complex unserializable Foo field, the engine should handle the integers and strings itself then hand the final stage to my custom serialization function to handle only the Foo field.
  * Classes should be able to override part of the serialization process then return control to the engine to finish the task. Extending my Foo example, I might have custom code to step in and serialize the Foo data but if Foo has a simple serializable member of type Bar, I want to hand serialization of that member back to the engine.
  * Classes should not need public modifiers on members to enable serialization. The engine should make efficient use of reflection to access private and protected members. Ideally the CLR would have a serialization-access level to allow an engine to work with private members in a low trust environment. I think serialization is such a fundamental function that it warrants some cooperation and special treatment by the runtime environment.
  * Classes shouldn't care what format the engine is serializing to or from. If it's binary, SOAP, XML, JSON, or any other new format, the attributes on my class and my custom serialization code should not need to change.

I'm sure there are some other requirements that I've forgotten and I'm sure this is somewhat too late for Visual Studio 2008 but hopefully I'm not the only one that feels this could be improved. What challenges have you faced with the .NET serialization system?

]]>
