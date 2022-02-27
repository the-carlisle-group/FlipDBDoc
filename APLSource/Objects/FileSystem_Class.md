# FileSystem Object

## Purpose

Working with the file system.

## Description

The FileSystem class is a repository of methods for working with the file system,
including creating, deleting, copying and moving files and folders.
All of methods are static.

The FileSystem object is accessed under the Scripting object. For example:

~~~
     Scripting.FileSystem.FileExists 'c:NoSuchFile.txt'
┌───────┐
│0      │
└Boolean┘
~~~

When making many calls to the FileSystem object, it is convenient to assign it to a
shorter name first. For example:

~~~
      FSO=Scripting.FileSystem
      FSO.FileExists 'c:NoSuchFile1.txt'
┌───────┐'
│0      │'
└Boolean┘'
      FSO.FileExists 'c:NoSuchFile2.txt'
┌───────┐
│0      │
└Boolean┘
~~~

