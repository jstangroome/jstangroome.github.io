---
layout: post
permalink: http://blog.stangroome.com/2012/04/23/tfs-lab-build-needs-to-start-the-environment/
title: TFS Lab Build needs to start the environment
description: None
date: 2012-04-23 03:12:49 -0000
last_modified_at: 2012-04-23 03:12:49 -0000
publish: true
pin: false
categories:
- SCVMM
- TFS
tags: []
---
Most of the Team Foundation Server virtual lab environments I work with use snapshots that are taken while the lab virtual machines are running. When configuring a Build Definition with the default lab build process template (LabDefaultTemplate.xaml) and choosing to restore the environment to one of these snapshots everything just works. However, [the guidance from Microsoft](http://blogs.msdn.com/b/bharry/archive/2010/02/10/a-tfs-2010-upgrade-success-story.aspx) around taking snapshots of certain virtual machines (primarily Active Directory domain controllers and occasionally SQL Servers) is to shutdown the VMs first before taking the snapshot. In recent work with a lab containing a domain controller, we’ve followed the recommendation of performing the snapshot while the VM is off. As a result of this I’ve discovered that the default lab build process template will restore to the selected snapshot and wait 20 minutes for the workflow capability of the environment to become ready and then fail. It never checks if the environment is started and never attempts to start it. I’ve since added a call to the GetLabEnvironmentStatus and StartLabEnvironment activities into the lab build process template just after the “Restore Snapshot” sequence and before the “Do deployment” sequence so the environment gets started if it isn’t already running. The [full customised build process template is available as a Gist](https://gist.github.com/2468512). The changes are small though - first a new variable definition is inserted at line 30:
  
        <Variable x:TypeArguments="mtlc:LabEnvironmentState" Name="LabEnvironmentState" />

And then 6 lines are inserted at line 81 (was line 80 before the above line was added):
  
        <mtlwa:GetLabEnvironmentStatus LabEnvironmentUri="[LabEnvironmentUri]" Result="[LabEnvironmentState]" DisplayName="Get Lab Environment State" />
    <If Condition="[LabEnvironmentState &lt;&gt; Microsoft.TeamFoundation.Lab.Client.LabEnvironmentState.Running]" DisplayName="If Lab Environment Not Running">
      <If.Then>
        <mtlwa:StartLabEnvironment LabEnvironmentUri="[LabEnvironmentUri]" DisplayName="Start Lab Environment"/>
      </If.Then>
    </If>

A very similar change will be required in TFS 11 because its lab build process still won't start an environment. Apparently this is to avoid making assumptions about the order in which each VM in a lab should be started.
