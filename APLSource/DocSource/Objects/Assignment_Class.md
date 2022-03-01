# Assignment Object

## Purpose

Represents a deferred insert, update, or delete.

## Description

The assignment object provides a holding place for accumulating
inserts, updates, and deletes to a database, which may then be executed
in one atomic transaction.

An assignment object is obtained as the result of the [DeferredInsert](),
[DeferredUpdate](), and [DeferredDelete]() methods of the Table object.
In addition, an array of assignment objects is returned by the 
[GatherAssignments]() method.
Once one or more assignment objects are accumulated, they may be
passed as an argument to the [Assign]() method which executes them.
