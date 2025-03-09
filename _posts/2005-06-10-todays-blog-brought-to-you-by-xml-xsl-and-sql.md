---
layout: post
permalink: http://blog.stangroome.com/?p=146
title: Today's blog brought to you by XML, XSL, and SQL
description: None
date: 2005-06-10 04:34:54 -0000
last_modified_at: 2005-06-10 04:34:54 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

The major project I am currently working on requires many tables to be created in several copies of an SQL Server database in multiple offices. All of these tables need very similar views and stored procedures. There usually are two views per table, one to show all the records, and one to show the records 'WHERE deleted = 0'. I never delete any information in this database, it just gets hidden. There also at least three stored procedures per table, two for synchronisation with other databases, and the other for inserting new records. Quite obviously this becomes tedious quickly. I couldn't find much on the web that suited my exact requirements and I was feeling too lazy to develop my own software. The answer is a very nice combo of XML, XSL, and a command line transformation tool kindly provided by Microsoft in the form of MSXSL.EXE.

Firstly I define any new tables I want to create in a fairly straight forward XML layout. For example:

> <table name="locations">  
>  <field name="deleted" type="bit" nullable="false" id="deleted" />  
>  <field name="lastRevision" type="lastRevisionType" nullable="false" />  
>  <field name="number" type="int" nullable="false" id="pk">  
>   <constraint type="primaryKey" />  
>  </field>  
>  <field name="suburb" type="varchar" size="64" nullable="false" />  
>  <field name="state" type="varchar" size="4" nullable="false" />  
>  <field name="postCode" type="varchar" size="4" nullable="false" />  
>  <field name="freightZone" type="int" nullable="false">  
>   <constraint type="foreignKey" foreignTableName="freightZones" foreignFieldName="number" />  
>  </field>  
> </table>

Secondly I use some rather fine tuned XSL scripts and a neat batch file to call MSXSL with the appropriate parameters to output the CREATE TABLE SQL, and the SQL for all the views and stored procedures into a file ready to be pasted into Query Analyzer. As you can see, the XML layout is very easy to understand and it would be equally easy to export this layout for existing tables in almost any database or to use this layout to generate any SQL or perhaps even documentation, diagrams, .NET classes or .NET Windows Forms with all the needed controls.

The tricky part in using XSL to generate SQL is getting commas between all fields except the last and, even more so, getting the whitespace output neatly. Fortunately I was cunning enough to ask a markup-wizard colleague of mine to do most of the XSL work for me. It was greatly appreciated as I only had to add the finishing touches and didn't need to read the W3C's XSL specs again. Admittedly, I kept the transformations simple. If I need to define a table with a multi-field primary key then I will need to adjust the resulting SQL before I use it. This happens rarely and is still easier than doing it all manually. There are probably other situations that cause the XSL to fall over but I haven't encountered them yet.

For anyone who has used similar techniques before or decides to try it now, I would like to hear some of your experiences. Here is an example of one of the XSL scripts to get you started:

> <?xml version="1.0" encoding="iso-8859-1" ?>  
> <xsl:stylesheet xmlns:xsl="<http://www.w3.org/1999/XSL/Transform>" version="1.0">  
>  <xsl:output method="text" encoding="iso-8859-1" indent="no" />  
>  <xsl:strip-space elements="*" />  
>    
>  <xsl:template match="table">  
> CREATE VIEW dbo.vw_<xsl:value-of select="@name" /> AS  
> SELECT  
> <xsl:for-each select="field[not(@id='deleted')]">  
>  <xsl:text> </xsl:text>  
>  <xsl:value-of select="@name"/>  
>  <xsl:if test="position() &lt; last()"><xsl:text>, </xsl:text></xsl:if>  
> </xsl:for-each>  
> FROM dbo.tb_<xsl:value-of select="@name" />  
> WHERE  
> <xsl:for-each select="field[@id='deleted']">  
>  <xsl:text> </xsl:text>  
>  <xsl:if test="position() &gt; 1"><xsl:text>AND </xsl:text></xsl:if>  
>  <xsl:value-of select="@name"/>  
>  <xsl:text> = 0</xsl:text>  
> </xsl:for-each>  
> GO
>
> GRANT SELECT ON dbo.vw_<xsl:value-of select="@name" /> TO sales, visitors  
> GO  
>  </xsl:template>  
>    
> </xsl:stylesheet>  
>

]]>
