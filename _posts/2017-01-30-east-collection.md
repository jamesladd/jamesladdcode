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

Some have referred to this style as a <a href="{{ https://en.wikipedia.org/wiki/Continuation-passing_style }}">{{ "Continuation" }}</a> or "Functional" style. I'm a Smalltalker so it was going to happen.

#### Collection
Consider the need to see if a given Collection of Permissions contains another Collection of Permissions. This is how it would be written using an East Oriented approach:

_The following code is contained in one file to make it easy for you to run as a working example, however in a real project each Class or Interface would be in its
own file and not static. Additional notes follow the code gist._

{% gist 087c6e6abe03e64d5b51c2c9b97cb2f1 %}

#### Notes
A native Collection is not used because you cannot define only the behaviour you want on a native Collection, and because if the internal collection (line 28) changes you
don't want to have to change all the code that depends on the Collection.

The Collection (the thing with the information) is asked to do the work = The Tell Don't Ask Principle.

As the caller / implementor of ContainedListener you can do whatever you want without modifying the Collection code. In this example I simply print the contained items. Your implementations
of ContainedListener will also serve as a definition of what happens with contained items rather than that logic being spread throughout the codebase.

To fit in with other ecosystems or frameworks there are some methods that don't adhere to an East Oriented approach like "toString" and "equals". These are usually kept to a minimum.

Making this Collection Class generic would not be hard, it is not made generic here so as to be more clear for the reader and immediately runnable.

The "equals" method of Permission is an example and doesn't adhere to Java conventions for an equals method.

I have found through following an East Oriented approach that I write less codei / tests, clearer code / test and my logic finds a Class where it belongs rather than being spread across the codebase.

_Your milage may vary_

