---
layout: post
permalink: http://blog.stangroome.com/?p=20
title: Strongly Typed Windows Forms Data Binding
description: None
date: 2008-10-14 11:12:36 -0000
last_modified_at: 2008-10-14 11:12:36 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

I was recently inspired by a [question about property names in strings](http://stackoverflow.com/questions/152250/get-class-property-name) on [Stack Overflow](http://stackoverflow.com/). The problem is that Windows Forms data binding takes the names of properties to bind as string parameters and if these properties change or are removed, errors won't surface until the bound control is exercised at run-time. So, using [Paul's](http://www.paulstovell.com/blog/) [strongly typed property name ideas](http://www.paulstovell.com/blog/strongly-typed-property-names), I propose this alternative syntax for programmatic data binding in Windows Forms:
  
    nameTextBox.Bind(t => t.Text, aBindingSource, (Customer c) => c.FirstName);
    // Binds the Text property on nameTextBox to the FirstName property
    // of the current Customer in aBindingSource, no string literals required.

And the following code to implement support for it:
  
    public static class ControlExtensions
    {
        public static Binding Bind<TControl, TDataSourceItem>(this TControl control, Expression<Func<TControl, object>> controlProperty, object dataSource, Expression<Func<TDataSourceItem, object>> dataSourceProperty) where TControl: Control
        {
            return control.DataBindings.Add(PropertyName.For(controlProperty), dataSource, PropertyName.For(dataSourceProperty));
        }
    }
  
    public static class PropertyName
    {
        public static string For<T>(Expression<Func<T, object>> property)
        {
            var member = property.Body as MemberExpression;
            if (null == member)
            {
                var unary = property.Body as UnaryExpression;
                if (null != unary) member = unary.Operand as MemberExpression;
            }
            return null != member ? member.Member.Name : string.Empty;
        }
    }

Thoughts? Overkill?

]]>
