---
layout: post
permalink: http://blog.stangroome.com/?p=26
title: Using IDisposable and Lambdas for temporary values
description: None
date: 2008-05-24 05:09:39 -0000
last_modified_at: 2008-05-24 05:09:39 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

What follows is some code I wrote last week that I couldn't find anywhere else and I thought was useful. First, is a test method demonstrating my use case, followed by the implementation of the TemporaryValue classes to fulfil the requirement.

The property get lambda expression can be an instance or static property as long as it is read-write. In this implementation there is no exception handling and no multithreading considerations.

[TestMethod]  
public void Original_property_value_should_be_restored_after_temporary_value_block()  
{  
    const string OriginalValue = "_original_value_";  
    const string NewValue = "_new_value_";  
    var f = new Foo {Bar = OriginalValue};  
  
    Assert.AreSame(OriginalValue, f.Bar);  
  
    using (TemporaryValue.For(() => f.Bar, NewValue))  
    {  
        Assert.AreSame(NewValue, f.Bar);  
    }  
  
    Assert.AreSame(OriginalValue, f.Bar);  
}

public static class TemporaryValue   
{   
    public static TemporaryValue For(Expression<Func> propertyGet, TValue newValue)   
    {   
        return new TemporaryValue(propertyGet, newValue);   
    }   
}   
   
public class TemporaryValue : IDisposable   
{   
    private PropertyInfo _propertyInfo;   
    private object _target;   
    private TValue _original;   
   
    public TemporaryValue(Expression<Func> propertyGet, TValue newValue)   
    {   
        var memberExpression = (MemberExpression)propertyGet.Body;   
        _propertyInfo = (PropertyInfo)memberExpression.Member;   
   
        var targetExpression = memberExpression.Expression;   
        if (null != targetExpression)   
        {   
            _target = Expression.Lambda<Func<object>>(targetExpression).Compile().Invoke();   
        }   
   
        _original = propertyGet.Compile().Invoke();   
        Set(newValue);   
    }   
   
    private void Set(TValue value)   
    {   
        _propertyInfo.SetValue(_target, value, null);   
    }   
   
    public void Dispose()   
    {   
        if (null != _propertyInfo)   
        {   
            Set(_original);   
            _propertyInfo = null;   
            _target = null;   
            _original = default(TValue);   
        }   
    }   
}

After learning to manipulate lambda expressions in this way I decided it would be a good idea to use a similar approach to remove the need for string literals in an INotifyPropertyChanged implementation. Unfortunately, while I was offline for a few days, [Paul got a head start](http://www.paulstovell.com/blog/strongly-typed-property-names).

]]>
