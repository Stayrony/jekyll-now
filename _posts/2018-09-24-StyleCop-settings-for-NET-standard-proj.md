---
layout: post
title: StyleCop Analyzers template for .NET Standard
tags: [StyleCop, c#, .net, .NET Standard]
comments: true
---

One of the most demanding aspects of a developer's life is working with legacy code created by others who may not share your philosophy on code creation. Code consistency is beneficial to both the team and the organization, with the added benefit of improving performance through rules such as closing database connections. Microsoft StyleCop provides the vehicle to put such rules in place, and it seemlessly integrates with Visual Studio.

StyleCop is an open source static code analysis tool from Microsoft that checks C# code for conformance to StyleCop's recommended coding styles and a subset of Microsoft's .NET Framework Design Guidelines. 

The rules are classified into the following categories:

- Documentation
- Layout
- Maintainability
- Naming
- Ordering
- Readability
- Spacing

# Getting it

Install Stylecop via [NuGet StyleCop.Analyzers](https://www.nuget.org/packages/StyleCop.Analyzers/).
![screenshot](/images/StyleCopNETStandard/StyleCop_Install.png "Install StyleCop.Analyzers")

# Choosing rules

For choosing rules right click on C# project in Solution Explorer and select Properties form the context menu (only for Windows):

![screenshot](/images/StyleCopNETStandard/Properties.png "Properties")

From the tabs alone the left of the configuration window select Code Analysis and open StyleCop rules file:

![screenshot](/images/StyleCopNETStandard/Code_analysis.png "Code Analysis")

You can edit the rule settings by opening this file:
![screenshot](/images/StyleCopNETStandard/Rules.png "Rules")

You can choose to ignore rules (Action=”None”) and get warnings (Action=”Warning”) or errors (Action=”Error”) in the case your code is not compliant.

![screenshot](/images/StyleCopNETStandard/Rules_1.png "Rules")
![screenshot](/images/StyleCopNETStandard/Rules_2.png "Rules")

You will find the settings file in your project folder here:

![screenshot](/images/StyleCopNETStandard/Settings_file.png "Settings File")

Now you can copy this file to "stylecop" folder in .NET Standard proj on VS for Mac.

# Add CodeAnalysisRuleSet property to the project

To enable a ruleset that we previously had in “stylecop” folder in the solution folder, you need to manually edit project file and add following.

```
<PropertyGroup>
  <CodeAnalysisRuleSet>$(SolutionDir)\stylecop\StyleCopRuleSet.ruleset</CodeAnalysisRuleSet>
</PropertyGroup>
```
Folders and files structure is displayed in the screenshot below.

![screenshot](/images/StyleCopNETStandard/Files_structure.png "Files structure")

# StyleCopRuleSet.ruleset file

Example of setting rules file:

![screenshot](/images/StyleCopNETStandard/Rules_3.png "Rules for proj")

# Just do it!

Now that the warnings are set to be treated as errors, the solution can no longer build until they are resolved.

For example, by default in StyleCop a closing curly bracket must not be preceded by a blank line. If one is set, an error will be received like the following:

![screenshot](/images/StyleCop/StyleCop_4.png "error")

Code readability and manageability are not big issues when working solo, but as the development team grows so does the issues related to managing a code base. A consistent coding standard simplifies the chore of code maintenance, and StyleCop provides this ability, as well as allows you to add your own rules to further enforce standards.

# References
1. Patton, Tony (June 24, 2011). ["Maintain code consistency with Microsoft StyleCop"](https://www.techrepublic.com/blog/software-engineer/maintain-code-consistency-with-microsoft-stylecop/). Blogs / Software Engineer. TechRepublic. Retrieved December 10, 2011.
2. [StyleCop: Setting to use it for a Solution with multiple Project Types (.NET Framework, .NET Core and .NET Standard Library)](http://www.softwarepronto.com/2018/05/stylecop-setting-to-use-it-for-solution.html)
3. [Visual Studio 2017 StyleCop Analyzers template for .NET Core](https://ignas.me/tech/visual-studio-2017-stylecop-analyzers-template-net-core/)
4. [.NET Core, Code Analysis and StyleCop](https://carlos.mendible.com/2017/08/24/net-core-code-analysis-and-stylecop/)
5. [StyleCop для .NET Core и .NET Standard](http://andrey.moveax.ru/post/net-standard-using-style-cop)