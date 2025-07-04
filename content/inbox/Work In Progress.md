---
title: Untitled
description: 
permalink: 
aliases: 
tags: 
draft: false
date: 
cssclasses:
---
## Published Notes without date
```dataview
TABLE file.mtime as "Last Modified"
WHERE draft = false
AND date = ""
SORT file.mtime asc
```

## Draft Notes
```dataview
TABLE file.mtime as "Last Modified"
WHERE draft = true
SORT file.mtime asc
```

