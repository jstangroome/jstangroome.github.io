---
permalink: http://blog.stangroome.com/2014/02/10/powershell-select-xml-versus-get-content/
title: PowerShell Select-Xml versus Get-Content
description: None
date: 2014-02-10 12:29:32 -0000
last_modified_at: 2014-02-10 12:29:32 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
In PowerShell, one of the most common examples you will see for parsing an XML file into a variable uses the [Get-Content cmdlet](http://technet.microsoft.com/en-us/library/hh849787.aspx) and the [cast operator](http://technet.microsoft.com/en-us/library/hh847732.aspx), like this:
  
        $Document = [xml](Get-Content -Path myfile.xml)

The resulting type of the $Document variable is an instance of [System.Xml.XmlDocument](http://msdn.microsoft.com/en-us/library/system.xml.xmldocument). However, there is another approach to get the same, or better, result using the [Select-Xml cmdlet](http://technet.microsoft.com/en-us/library/hh849968.aspx):
  
        $Document = ( Select-Xml -Path myfile.xml -XPath / ).Node

Sure, using the second variant is slightly longer, but with an important benefit over the first, and it's not performance related. In the first example, the file is first read into an array of strings and then cast. The casting operation (implemented by System.Management.Automation.LanguagePrimitives.ConvertToXml) is using an [XmlReaderSettings](http://msdn.microsoft.com/en-us/library/system.xml.xmlreadersettings) instance with the [IgnoreWhitespace](http://msdn.microsoft.com/en-us/library/system.xml.xmlreadersettings.ignorewhitespace)property set to true and an XmlDocument instance with the [PreserveWhitespace](http://msdn.microsoft.com/en-us/library/system.xml.xmldocument.preservewhitespace) property set to false. In the second example, the file is read directly into an XmlDocument (implemented by System.Management.Automation.InternalDeserializer.LoadUnsafeXmlDocument) using an XmlReaderSettings instance with the IgnoreWhitespace property set to false and an XmlDocument instance with the PreserveWhitespace property set to true - the opposite values of the first example. The Select-Xml approach won't completely preserve all the original formatting from the source file but it preserves much more than the Get-Content approach will and I've found this extremely useful when bulk updating version controlled XML files with a PowerShell script and wanting the resulting file diff to show the intended change and not be obscured by formatting changes. You could construct the XmlDocument and XmlReaderSettings directly in PowerShell but not in so few characters. You can also load the System.Xml.Linq assembly and use the XDocument class which appears to give slightly better formatting consistency again but it's still not perfect and PowerShell doesn't provide the same quick access to elements and attributes as properties on the object.
