# Health Groups SCOM Management Pack

# MP and Visual Studio Files

* [Sealed MP](MPs/Health.Groups.mp)
* [Unsealed MP](MPs/Health.Groups.xml)
* [Visual Studio Solution Files](<Health Groups>)

# Introduction
Health Group MP is a simple MP to address an annoyance with the groups that the authoring workspace creates and a limitation of dependency monitors. 

* Groups are created without any dependency monitors to roll up the health of members. You workaround this for already created group using the [OpsMgr Group Health Rollup Configuration Task Management Pack](https://ds.squaredup.com/blog/opsmgr-group-health-rollup-configuration-task-management-pack/), but I'd prefer a one step process.

>No monitors targeting the group so no health roll up
![Standard Group](<Screencaps/Standard Group.png>)

* There isn't a way to roll up critical health states as warning if only a set percentage are critical. Dependency monitor roll up methods available out of the:

  * Best of – Fairly obvious how this one works, as long as one of the member object was healthy the monitor would be healthy 
  * Worst of – Again pretty straight forward, the monitor would match the worst state of any of the members 
  * Percentage – The monitor will match a percentage of the member objects, almost what we want

The MP adds two authoring templates. 

![Authoring Templates](<Screencaps/Authoring Templates.png>)

* Group with Health - Creates a group with dependency monitors already targeted.

> Group member states roll up to group. 
![Group with Health](<Screencaps/Group with Health.png>)

* Group with Smart Health - Creates a group with a set of dependency monitors that roll up critical health states as warning if only a set percentage (50% by default) are critical. 

>Only 50% of members are critical so group is warning.
![Rolling up 50% or less critical as warning](<Screencaps/Group with Smart Health - Warning.png>)

>More than 50% of members are critical so group is critical.
![Rolling up more than 50% critical as Critical](<Screencaps/Group with Smart Health - Critical.png>)

## Smart Group Health Roll Up - Under the hood

By combining two of the dependency monitor roll up algorithms and creating a recovery task. We can roll up critical states as warning until the percentage threshold is reached. Targeted at the group we have

* A dependency monitor with the worst of algorithm. 
* A recovery that targets the worst of monitor and changes critical state to warning state. 
* Another dependency monitor with the percentage algorithm - we need this to roll up a critical state once more than the defined percentage are in a critical state. 

>Health Explorer shows the two monitors combining
![Health Explorer](<Screencaps/Group with Smart Health - Health Explorer.png>)




