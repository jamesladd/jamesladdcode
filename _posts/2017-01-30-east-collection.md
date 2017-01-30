---
layout: post
title: East Collections
tags: east design
---
This post is about implementing a Collection using an East Oriented approach.

#### A Recap
There are a two simple rules to greatly improve your code and these are collectively called an East Oriented approach.
These rules are:

1. Methods must be void (no return value) or return this (self). Preferably void (no return value).
2. An exception to rule #1 is where the method is a Factory Method that returns a new instance of a Class.

Some have referred to this style as a "Continuation" or "Functional" style.

#### Collection
Consider the need to see if a given Collection of Permissions contains another Collection of Permissions. This is how it would be written using an East Oriented approach:

_Note: the following code is contained in one file to make it easy for you to run as a working example, however in a real project each Class or Interface would be in its
own file._

{% gist 087c6e6abe03e64d5b51c2c9b97cb2f1 %}

