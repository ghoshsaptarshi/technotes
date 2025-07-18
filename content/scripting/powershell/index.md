---
title: PowerShell
description: 
permalink: 
aliases: 
tags:
  - PowerShell
draft: false
date: 2025-07-07
cssclasses:
---
PowerShell is a combination of shell, scripting language and automation platform primarily built for Windows Platform [^1]. It offers multiple enhancements/features over regular shells like -
- Integrated IDE with debugger support
	- The IDE is called "PowerShell ISE"
- It can accept/return .NET objects
	- This sets it apart from regular shells which typically accept/return text only
- It's extensible using modules, classes, etc

### The command syntax
PowerShell scripts are saved with `.ps1` extensions.
Compiled commands in PowerShell are known as cmdlets and follows a **Verb-Noun** naming convention.
Some of the core cmdlets -
- `Get-Help`
- `Get-Command`
- `Get-Member`

### Get-Help

```powershell
Get-Help -Name Get-Help
```

## Resources
- [PowerShell 101](https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/00-introduction?view=powershell-7.5)
- [Microsoft Learning Portal - PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.5)
- [PowerShell Gallery](https://www.powershellgallery.com/)
- [PowerShell Module Browser](https://learn.microsoft.com/en-us/powershell/module/)

[^1]: It has been extended to other platforms as well.