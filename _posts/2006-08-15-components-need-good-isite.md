---
layout: post
permalink: http://blog.stangroome.com/?p=124
title: Components need good ISite
description: None
date: 2006-08-15 02:12:21 -0000
last_modified_at: 2006-08-15 02:12:21 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

Accessing a Form from inside a Component is surprisingly convoluted in the .NET Framework. I created a new class that inherits from Component that would provide extra functionality to any Form I dropped it on. Ideally, it would search it's parent Form for various types of controls and add handlers to certain control events to provide special behaviour. Unfortunately, neither the Component.Site or .Container properties provide access to the parent Form or ContainerControl.

Some of the components included with the Framework are interacting with their parents so I know it's possible. I dropped a ErrorProvider onto a new Form and checked the designer code to see how it was instantiated. I then opened up Reflector, headed directly for the ErrorProvider component and followed this rather bizarre code path:

  1. The designer code creates a private member "components" initialised as a new ComponentModel.Container.  
  2. It then creates the ErrorProvider passing the new Container to its constructor.  
  3. The ErrorProvider constructor simply calls the Container.Add method passing itself.  
  4. The Container.Add method (the trick bit), after extensive validation and array sizing, creates a new ComponentModel.Site and assigns it to the (ErrorProvider) component's Site property.  
  5. The ErrorProvider.Site property stores a reference to the incoming Site then proceeds to request a ComponentModel.Design.IDesignerHost via a call to Site.GetService.  
  6. The ErrorProvider then retrieves the IDesignerHost.RootComponent property, casts it to a ContainerControl and assigns the value to it's own public ContainerControl property.  
  7. The designer then serializes the value of this property to the designer code of the parent Form as "Me.ErrorProvider1.ContainerControl = Me".

The ErrorProvider, and probably many other components included with the Framework, use the behaviour of the designer's code generator to ensure it will have access to it's container at run-time. This feels tightly-coupled yet fragile all at the same time with these odd interdependencies.

Sadly, a Component cannot find trace its roots any other way, so my custom component was forced to borrow this ugly technique. Like the ErrorProvider, my component too must be instantiated differently if created programmatically instead of in the Visual Studio designer.

At least now I, and hopefully you, understand how the Component, Site, and Container classes interact with each other and if you want to access the Form from your own Component, know you can.

]]>
