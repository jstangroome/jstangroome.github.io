---
layout: post
permalink: http://blog.stangroome.com/2013/07/25/get-hyper-v-guest-data-without-xml-parsing/
title: Get Hyper-V guest data without XML parsing
description: None
date: 2013-07-24 22:37:42 -0000
last_modified_at: 2013-08-07 22:40:20 -0000
publish: true
pin: false
categories:
- PowerShell
- Virtualization
tags: []
---
I recently needed to query the Hyper-V KVP Exchange data for a guest VM to find the currently configured IPv4 address of the VM's network adapter. A quick search of the Internet reveals that the Msvm_KvpExchangeComponent WMI class is the source of this information and there are at least two blog posts that cover it well:
    * [How to get the IP address of a virtual machine from Hyper-V](http://blogs.technet.com/b/m2/archive/2010/07/29/how-to-get-the-ip-address-of-a-virtual-machine-from-hyper-v.aspx)
    * [Customizing The Key Value Pair (KVP) Integration Component](http://blogs.msdn.com/b/taylorb/archive/2012/12/05/customizing-the-key-value-pair-kvp-integration-component.aspx)
However, in both of these blogs, the actual data comes back as XML which is then parsed using XPath. The original XML looks something like this:
  
        <INSTANCE CLASSNAME="Msvm_KvpExchangeDataItem">
     <PROPERTY NAME="Data" TYPE="string">
      <VALUE>169.254.103.5</VALUE>
     </PROPERTY>
     <PROPERTY NAME="Name" TYPE="string">
      <VALUE>RDPAddressIPv4</VALUE>
     </PROPERTY>
     <PROPERTY NAME="Source" TYPE="uint16">
      <VALUE>2</VALUE>
     </PROPERTY>
    </INSTANCE>

As soon as I saw this XML I recognised it as the DMTF CIM XML format - the same format that the new PowerShell v3 CIM Cmdlets use to transport CIM instances over HTTP (I believe). If this is the format used by PowerShell, it seemed a reasonable assumption that PowerShell or the .NET Framework must already have an implementation for deserializing this XML properly so I don't have to code it myself. With [Reflector](http://www.red-gate.com/products/dotnet-development/reflector/) in hand, I started my investigation at [ManagementBaseObject.GetText](http://msdn.microsoft.com/en-us/library/system.management.managementbaseobject.gettext.aspx) which converts to XML but I couldn't find any complementary methods to go the other direction. I then proceeded to look at ManageBaseObject's implementation of the [ISerializable](http://msdn.microsoft.com/en-us/library/system.runtime.serialization.iserializable.aspx) interface and corresponding constructor but that appears to use binary serialization of COM types. Finally, I turned to the [CimInstance](http://msdn.microsoft.com/en-us/library/microsoft.management.infrastructure.ciminstance\(v=vs.85\).aspx) class and its implementation of ISerializable and discovered the [CimDeserializer](http://msdn.microsoft.com/en-us/library/microsoft.management.infrastructure.serialization.cimdeserializer\(v=vs.85\).aspx). Unfortunately the CimDeserializer methods take a byte array as input and I had strings. So assuming round-tripping should work, I looked to the CimSerializer and tried passing it a CimInstance and inspected the byte array that was returned - every second byte is zero, and the rest fit within 7-bits... smells like Unicode. Taking a small gamble I took the strings from the Msvm_KvpExchangeComponent instance, used System.Text.Encoding.Unicode to convert them to byte arrays and passed them to CimDeserializer.DeserializeInstance. Huzzah! Properly deserialized Msvm_KvpExchangeDataItem instances. And here is the final PowerShell script to return the items for a given VM name: <https://gist.github.com/jstangroome/6068782>
