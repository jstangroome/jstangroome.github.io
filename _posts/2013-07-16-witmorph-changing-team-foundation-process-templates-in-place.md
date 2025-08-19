---
permalink: /2013/07/16/witmorph-changing-team-foundation-process-templates-in-place/
title: WitMorph - changing Team Foundation process templates in-place
date: 2013-07-16 06:02:52 -0000
last_modified_at: 2014-02-11 20:15:20 -0000
publish: true
pin: false
categories:
- TFS
tags:
- WitMorph
---
## Background

Historically Team Foundation Server process templates have not had a good upgrade story. When you create a new Team Project you are required to select the process template (typically CMMI, Agile, or recently Scrum) and that is the process template used by your project for the rest of its life. From the first TFS version, customizing the project has always been possible - adding and removing fields to work items, or adding entirely new work item types, and the TFS Power Tools certainly improve that experience by providing a GUI instead of a wall of XML and a command-line. But when a new version of TFS ships with new versions of the process templates you’ve either had to create a new Team Project with the new template and try to copy most* of the data across, or hope someone publishes a way to upgrade between specific process template versions - typically another exercise in tedious manual XML manipulation. If you've chosen the wrong process template and want to change from Agile to Scrum then I really don’t envy the work ahead of you. Team Foundation Server 2012 included a wizard in Web Access for the first time to help add to an existing Team Project the process template components needed to enable the Task Board and Code Review functionality but it doesn’t upgrade the template completely, it only works with TFS 2010 templates or newer, and will be limited by any customizations that may have been made. Having performed many TFS upgrades for equally many clients over the years, I've often had to perform the manual analysis of the differences between various versions of a TFS process template or a client’s custom template, and find a way to migrate from one to the other. I've always felt that this process could be approached much like a database migration would and could be automated. This year I finally had the opportunity to try it.

## A Series Of Incremental Changes

Take a simple scenario of one of the differences between Microsoft’s Agile and Scrum templates – the former has a User Story work item with a Stack Rank field and the latter has a Product Backlog Item work item with a Backlog Priority field. In each template, this field is used to provide a number to specify the relative ordering of the work to be done. To convert a Team Project from the Agile template to the Scrum template I would need to rename the “User Story” work item to “Product Backlog Item” instead, typically via the “witadmin renamewitd” command line tool, and I would also need to rename the “Microsoft.VSTS.Common.StackRank” field to “Microsoft.VSTS.Common.BacklogPriority” but TFS doesn't have a mechanism for renaming a field’s reference name (only the Display Name). To rename the field, I need to perform three discrete steps instead:
    1. Add the Backlog Priority field to the User Story work item type definition in addition to the Stack Rank field and import the new definition into the existing Team Project.
    2. Copy the value of the Stack Rank field on every existing User Story work item in the Team Project into the Backlog Priority field.
    3. Remove the Stack Rank field from the User Story work item type definition and import it again.
There is also a similar process required when the work item states are different between two work item definitions. As you can imagine, the process for determining, and then executing each of these steps for each individual field and state on each work item type is time consuming and error prone but it also very algorithmic.

## A Comparison, a Map, and an Action List

The approach I have used for automatically calculating and applying the incremental changes begins with finding the differences between the templates. Using the Agile-to-Scrum conversion scenario again, some key differences that would be detected are:
    * The User Story and Issue work item types are removed in the Scrum template.
    * The Product Backlog Item and Impediment work item types are added in the Scrum template.
    * The Task work item type still exists but:
      * The New, Active, and Closed states are removed.
      * The To Do, In Progress, and Done states are added.
As you can see from this list, a pure set of differences is insufficient. I don’t want to delete all the User Story work items, I want to convert them to Product Backlog Item work items. So I need to define a map like this:
    * User Story work item => Product Backlog Item work item.
    * Issue work item => Impediment work item.
    * Task work item => Task work item.
      * New state => To Do state.
      * Active state => In Progress state.
      * Closed state => Done state.
This map can then be applied to the simple differences and produce something more useful:
    * The User Story work item type is renamed to Product Backlog Item
      * The Stack Rank field is renamed to Backlog Priority
    * The Issue work item type is renamed to Impediment
    * The Task work item’s New state is renamed to To Do.
It is important to note that maps are directional – a User Story has Active, Resolved, and Closed states but a PBI has Approved, Committed, and Done states which don’t perfectly align 1-to-1 and will be mapped slightly differently depending on the direction of the conversion. Finally, from a mapped difference list, an ordered Action list can be determined:
    1. Add Backlog Priority field to the User Story work item.
    2. Add To Do state to Task work item.
    3. Copy Stack Rank field value on all User Story work items to the Backlog Priority field.
    4. Change all Task work items in the New state to be in the To Do state instead.
    5. Remove the Stack Rank field from the User Story work item.
    6. Remove the New state from the Task work item.
    7. Rename the User Story work item type to “Product Backlog Item”.
While it is an arbitrary decision to rename the work item types at the end instead of the beginning, the order of the other actions is more critical – I cannot change Tasks to the To Do state until that state has been defined. Also, while the order implies some dependencies between actions, these dependencies have varying importance. For example, the Backlog Priority field *must* be added before values are copied to it, but it is reasonable to decide to remove the Stack Rank field without copying the data.

## The Code

WitMorph (Work Item Type Morph) started as a proof-of-concept code base to see if the idea of automating this process would even work. Once I had successfully tested a few basic scenarios I started to refactor the code toward something more maintainable and testable. At the moment the structure of WitMorph can be broken down as follows:
    * The WitMorph class library. The core functionality for the approach described above. It handles:
      * Reading the description of a process template from an active Team Project, a Collection’s template store, or a process template folder on disk.
      * Defining a map between two process templates.
      * Comparing two process templates and generating a list of differences.
      * Converting a list of differences into an ordered list of actions.
      * Applying the list of actions to an active Team Project.
    * A very basic WitMorph.Console command line program to drive a Scrum 2.0 to Agile 6.0 conversion for the given TFS Collection URI and Team Project names provided as parameters.
    * A work-in-progress WitMorph.UI Windows Forms** program to walk a user though the process of selecting the Team Project, the Process Template, and the template map, finding the differences, generating an action list, and choosing which actions to apply.
    * A small set of automated tests which restore a TFS collection, perform a conversion, and repeat – rather infrastructure heavy by nature. Mocking TFS in these tests are of little value because TFS’ idiosyncrasies are key to successfully testing process template conversions.
The code for WitMorph is completely open source and hosted on GitHub here: <https://github.com/codeassassin/WitMorph> In my next post I [walkthrough converting a process template](http://blog.stangroome.com/2013/07/16/witmorph-walkthrough/ "WitMorph – Walkthrough a Conversion").

### Footnotes

*Most processes for copying work item data between Team Projects will lose attachments, links, and history. The more comprehensive processes (ie TFS Integration Platform) for copying data will keep these items but the history will contain the time stamps and user details of the person performing the migration, not the original audit data. **I’d rather have a WPF GUI but my personal WPF skills would only have impeded my ability to deliver something useful, quickly. If you would like to build the WPF GUI, just send a pull request.
