---
title: tuple
description: 
permalink: 
aliases: 
tags: 
draft: true
date: 
cssclasses:
---
# Tuple


```python
SimpleTuple1 = ("Arghya", "Banik", "Saptarshi", "Ghosh")
SimpleTuple2 = ("1", "2", "3", "4")`
print(SimpleTuple1)
```

## Length of a Tuple
```python
print(len(SimpleTuple1))
```

## Extraction

```python
print(SimpleTuple1[2])
print(SimpleTuple1[-2])
print(SimpleTuple1[1:2])
print(SimpleTuple1[1:3])
print(SimpleTuple1[2:])
print(SimpleTuple1[:3])
print(SimpleTuple1[:])
```

## Immutability
```python
SimpleTuple1[0] = "Test"
print(SimpleTuple1)
```

## Membership
```python
print("Arghya" in SimpleTuple1)
print("Arghya" not in SimpleTuple1)
print("Blabla" in SimpleTuple1)
print("Blabla" not in SimpleTuple1)
```

## Merging 2 tuples
```python
SimpleTuple3 = SimpleTuple1 + SimpleTuple2
print(SimpleTuple3)
```

## count()
```python
print(SimpleTuple1.count("Arghya"))
print(SimpleTuple1.count("Blabla"))
```

## index()
```python
print(SimpleTuple1.index("Arghya"))
print(SimpleTuple1.index("Blabla"))
```

## Test
```python
test1 = ("Arghya")
print(type(test1))
test2 = ("Arghya",)
print(type(test2))
```