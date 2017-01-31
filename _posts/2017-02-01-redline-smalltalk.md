---
layout: post
title: Redline Smalltalk
tags: smalltalk redline
---
![Redline Smalltalk Mascot](/public/img/redline-mascot.png)

As some of you will know I have been implementing a Smalltalk compiler for the Java Virtual Machine. You can find more details [here](http://redline.st) and the source [here](https://github.com/redline-smalltalk/redline-smalltalk).

After some time away doing other things I have come back to Redline Smalltalk with gusto. The latest code is a total re-write as I'm now using a PEG based grammar via ANTLR4 where previously I had used ANTLR3 which was not as good at handling Smalltalk syntax. The result is a smaller and faster grammar. 

Another major change is that everything is more message based, that is instead of generating a Java Class in memory that represents the Smalltalk class and instantiating it the Java Class that is generated simply sends messages to Smalltalk Objects to achieve the same result. The way Smalltalk was intended.

The compiler is almost complete and you run Smalltalk today. Here is an example 'Hello World'.

{% gist 30469af041fa0a6685e63eaf2162fbb5 %}

And here is a snippet from the source for Object.st the base Object in the Smalltalk Runtime Hierarchy:

{% gist 1ca37bf18116af59babc9e371dcf24b9 %}

All Smalltalk sources are loaded through the SmalltalkClassLoader which generates an in memory Java Class and instanitated which causes the messages contain within the class to be sent. Given a Java Class is generated it is possible to integrate with Java Libraries and for Java to call code in Smalltalk.

You will notice in the snippet above that some messages are being sent to the Object 'JVM'. This is a special Object that allows Redline to generate any JVM bytecode instruction thereby allowing any code you can create with the JVM to be implemented. You can think of messages to JVM as the equivalent to inline assembler in C/C++.

You can also package up Smalltalk into a STAR (A Java Archive but with a snappier name) and deploy to Amazon AWS Lambda.

Work continues on Redline Smalltalk and I welcome people to get involved. You don't need to know Java although there is also work to be done for people who do.
If you would like to be involved please email me at object at redline.st

