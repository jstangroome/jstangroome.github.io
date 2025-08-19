---
permalink: /2012/01/16/create-a-powershell-v3-ise-add-on-tool/
title: Create a PowerShell v3 ISE Add-on Tool
date: 2012-01-15 23:38:27 -0000
last_modified_at: 2012-01-16 04:14:15 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
_Note: This process is based on PowerShell v3 CTP 2 and is subject change._ When you open PowerShell v3's ISE (Integrated Scripting Environment) you should see a new Commands pane that wasn't present in version 2. [![](http://blog.stangroome.com/wp-content/uploads/2012/01/commands-pane.png)](http://blog.stangroome.com/wp-content/uploads/2012/01/commands-pane.png) This is a built-in example of an ISE Add-on Tool but you can also create your own quite easily. At its simplest an ISE Add-on Tool is a WPF Control that implements the [IAddOnToolHostObject](http://msdn.microsoft.com/en-us/library/microsoft.powershell.host.ise.iaddontoolhostobject\(v=vs.85\).aspx)interface. To get started writing your own add-on follow these simple steps:
    1. Open Visual Studio and create a new WPF User Control Library project. The new project should contain a new UserControl1.
    2. Add a new Project Reference to the Microsoft.PowerShell.GPowerShell (version 3.0) assembly located in the GAC.
    3. Open the UserControl1.xaml.cs code-behind file and change the UserControl1 class to implement the IAddOnToolHostObject interface.
    4. The only member of this interface is the HostObject property which can be declared a a simple auto-property for now.
    5. Build the project and find the path of the DLL it produces.
    6. Open the PowerShell v3 ISE and load the DLL you have just built, eg: Add-Type -Path 'c:\users\me\documents\project1\bin\debug\project1.dll'
    7. Add the UserControl to the current PowerShell tab's VerticalAddOnTools collection and make it visible (replace "Project1.UserControl1" below with the full namespace of your control): $psISE.CurrentPowerShellTab.VerticalAddOnTools.Add('MyAddOnTool', [Project1.UserControl1], $true)
After following these steps you should see a new, blank pane as an extra tab where the Commands pane normally appears and now you can return to Visual Studio and start adding to the appearance and behaviour of your new add-on tool. There are some other things worth knowing about developing ISE Add-On Tools:
    * When your UserControl is added to the ISE, the HostObject property is set to an object almost identical to the [$psISE](http://technet.microsoft.com/en-us/library/dd819500.aspx) variable and your control uses this to interact with the ISE itself. For example you can manipulate text in the editor, or you can register for events that tell your control when a new file is opened.
    * While your UserControl is loaded in the ISE, you cannot build in Visual Studio because the DLL is locked and you need to close the ISE to unlock the file. I recommend adding a small PowerShell script to your project that performs steps 6 and 7 above and change your project's Debug options in Visual Studio so that the ISE is started and your script is opened when you press F5.
    * In step 7 above, your control is added to the VerticalAddOnTools collection but there is also a HorizontalAddOnTools collection and the user can move your control between these two at any time via the ISE menus so make sure you design the appearance of your Add-On Tool to work in both orientations.
Alternatively the [Show-UI module](http://showui.codeplex.com/) for PowerShell includes a ConvertTo-ISEAddOn cmdlet to create the necessary WPF objects natively from PowerShell and avoid the need for a Visual Studio project.
