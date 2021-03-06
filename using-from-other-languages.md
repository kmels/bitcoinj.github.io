---
layout: base
title: "How to access bitcoinj from other languages"
---

<div markdown="1" id="toc" class="toc"><div markdown="1">

* Table of contents
{:toc}

</div></div>

<div markdown="1" class="toccontent">

#How to access bitcoinj from other languages

##Introduction

Java is a fine choice for something that needs to be widely understood, secure against buffer overflows and usable from Android phones. But it's also old, verbose and just generally is not for everyone.

This page details how you can go about using bitcoinj from other languages.

###C++

At the moment, the easiest way to use bitcoinj from C++ is to embed the JVM and use auto-generated JNI wrappers. Here's an example of what such a program looks like:

[https://github.com/mikehearn/cppjvm/blob/master/mytest/bcj-hello-world.cpp](https://github.com/mikehearn/cppjvm/blob/master/mytest/bcj-hello-world.cpp)

It relies on a fork of a tool called CPPJVM, which reflects Java class files and then generates C++ wrapper objects that invoke the Java methods/reads the fields.

The tool generates all the classes needed based on a recursive exploration of some root classes, listed here:

[https://github.com/mikehearn/cppjvm/blob/master/java/cppjvm/morkfile](https://github.com/mikehearn/cppjvm/blob/master/java/cppjvm/morkfile)

Because `WalletAppKit` is there and you can reach most of the bitcoinj API via this class, most of the API has wrappers generated. If you're missing a piece, you can add the class you want to that list, delete the "gen" directory and rerun make.

Callback interfaces (`WalletListener`, `PeerEventListener` etc) have to be bound manually and that isn't complete. If you want to use bitcoinj from C++, you'll need to finish off the thunks. Fortunately it's easy and there aren't many of them:

[https://github.com/mikehearn/cppjvm/blob/master/mytest/native-listeners.h](https://github.com/mikehearn/cppjvm/blob/master/mytest/native-listeners.h)

[https://github.com/mikehearn/cppjvm/blob/master/mytest/native-listeners.cpp](https://github.com/mikehearn/cppjvm/blob/master/mytest/native-listeners.cpp)

We are also exploring complete automated conversion of the bitcoinj sources into C++, so it could be used without any JVM at all. However this work is at an earlier, less usable stage.

###Objective-C

The Hive team maintain a library called [BitcoinKit](https://github.com/hivewallet/BitcoinKit), which is designed for building wallet apps on MacOS X. It does not expose the raw bitcoinj API, rather providing its own Objective-C API on top and using JNI under the covers.

###Python

Python has many fans and can be fun to prototype in. The original Python implementation cannot interop with Java code, but we can gain access via another way using [Jython](http://www.jython.org/index.html). Jython is an implementation of Python (2.x) that is compatible with the original language/class libraries and provides an interactive interpreter that works just like regular Python's for exploration and learning. As long as you don't rely on any custom CPython extensions, you can just run your regular Python application out of the box, including UNIX style executable scripts.

Why would you do that? Primarily because you can import Java class libraries as you would Python class libraries (from com.google.bitcoin.core import Wallet). You also get a better garbage collector and real multi-threading support.

###Ruby

In a similar vein to Jython, there is also [JRuby](http://jruby.org/).  Like Jython it runs Ruby on the JVM, with the same advantages - an excellent garbage collector, interop with Java code, real multi-threading support and so on. Like Jython the only thing it has issues with is modules that rely on C extension functions, but anything pure Ruby works.

###Lisp

The block explorer at biteasy.com is written in Clojure, a dialect of Lisp that runs on the JVM. It uses bitcoinj directly.

</div>
