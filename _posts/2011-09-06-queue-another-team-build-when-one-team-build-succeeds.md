---
layout: post
permalink: http://blog.stangroome.com/2011/09/06/queue-another-team-build-when-one-team-build-succeeds/
title: Queue another Team Build when one Team Build succeeds
description: None
date: 2011-09-06 06:26:46 -0000
last_modified_at: 2014-02-19 10:07:53 -0000
publish: true
pin: false
categories:
- TFS
tags: []
---
**Update:[with Team Build 2013 you can even pass parameters to queued builds](http://blog.stangroome.com/2014/02/19/queue-a-team-build-from-another-and-pass-parameters/ "Queue a Team Build from another and pass parameters").** I have seen several Team Foundation Server environments where multiple build definitions exist in a single project and need to executed in a particular order. Two common techniques to achieve this are:

* Queue all the builds immediately and rely upon using a single build agent to serialize the builds. This approach prevents parallelization for improved build times and continues to build subsequent builds even when one fails.
* Check build artifact(s) into source control at the end of the build and let this trigger subsequent builds configured for Continuous Integration. This approach can complicate build definition workspaces and committing build artifacts to the same repository as code is not generally recommended.

As an alternative I have developed a simple customization to TFS 2010's default build process template (the DefaultTemplate.xaml file in the BuildProcessTemplates source control folder) that allows a build definition to specify the names of subsequent builds to queue upon success. It only requires two minor changes to the Xaml file. The first is a line inserted immediately before the closing </x:Members> element near the top of the file:
  
        <x:Property Name="BuildChain" Type="InArgument(s:String[])" />

The second is a block inserted immediately before the final closing </Sequence> at the end of the file
  
        <If Condition="[BuildChain IsNot Nothing AndAlso BuildChain.Length &gt; 0]" DisplayName="If BuildChain defined">
          <If.Then>
            <If Condition="[BuildDetail.CompilationStatus = Microsoft.TeamFoundation.Build.Client.BuildPhaseStatus.Succeeded]" DisplayName="If this build succeeded">
              <If.Then>
                <Sequence>
                  <Sequence.Variables>
                    <Variable x:TypeArguments="mtbc:IBuildServer" Default="[BuildDetail.BuildDefinition.BuildServer]" Name="BuildServer" />
                  </Sequence.Variables>
                  <ForEach x:TypeArguments="x:String" DisplayName="For Each build in BuildChain" Values="[BuildChain]">
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="buildChainItem" />
                      </ActivityAction.Argument>
                      <Sequence DisplayName="Queue chained build">
                        <Sequence.Variables>
                          <Variable Name="ChainedBuildDefinition" x:TypeArguments="mtbc:IBuildDefinition" Default="[BuildServer.GetBuildDefinition(BuildDetail.TeamProject, buildChainItem)]"/>
                          <Variable  Name="QueuedChainedBuild" x:TypeArguments="mtbc:IQueuedBuild" Default="[BuildServer.QueueBuild(ChainedBuildDefinition)]"/>
                        </Sequence.Variables>
                        <mtbwa:WriteBuildMessage Message="[String.Format(&quot;Queued chained build '{0}'&quot;, buildChainItem)]" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" />
                      </Sequence>
                    </ActivityAction>
                  </ForEach>
                </Sequence>
              </If.Then>
            </If>
          </If.Then>
        </If>

You can see the resulting [DefaultTemplate.xaml file here](https://gist.github.com/1196590/). After applying these changes and checking-in the build process template file you can specify which builds to queue upon success via the Edit Build Definition windows in Visual Studio: [![The new BuildChain build process parameter](http://blog.stangroome.com/wp-content/uploads/2011/09/capture.png)](http://blog.stangroome.com/wp-content/uploads/2011/09/capture.png) You can specify the names of multiple Build Definitions from the same Team Project each on a separate line. When the first build completes successfully, all builds listed in the BuildChain property will then be queued in parallel and processed by the available Build Agents. No checking is currently done for circular dependencies so be careful not to chain a build to itself directly or indirectly and create an endless loop.
