---
title: PHP To Python
date: 2016-02-24 00:00:00 -07:00
categories:
- Programming
tags:
- Python
- PHP
- programming
- tutorials
layout: post
---

# Lessons learned going from PHP to Python.

### Classes

While Python does have and use classes, it's not always required to use them. You can think of files as classes in most cases. 

File:  cat.py

```python
def meow():
	print "meow"
```

File: speak.py

```python
import cat

cat.meow()
```

Although, if you want to use classes you can. There are some interesting differences to remember.

```python
class Cat: # Class names are StudlyCased
	def purr(self): #functions are typically lowercase, need self  if you aren't passing params
		print "purr"

var kitten = Cat() # To instantiate use parens () "new" is not a thing in python
kitten.purr()
```

What is `self`? Check out this answer [here](http://stackoverflow.com/a/21366809/1227343)

What about `construction`? 

PHP: 

```php
<?
	class Kitten() {
		public function __construct() {}
	}
```

Python:

```python
class Kitten:
	__init__():
		# construction stuff here
```

### Switches

PHP:

```php
<?
switch ($i) {
	case 0:
		echo "i equals 0";
		break;
	case 1:
		echo "i equals 1";
		break;
	default:
		echo "default";
		break;
}
```

Python:

```python
i = 1
if (i == 0):
	print "One"
elif (i == 1):
	print "Two"
else:
	print "default"
```
Notice in Python it's just an if-elseif-else statement. In Python there is no switch statement. A switch is just shorthand for conditionals, so it's not a huge loss if you really think about it.


## Notice
This article is a work in progress. More to come. If you notice any issues or typos, please leave it in the comments and I will happily fix it.
