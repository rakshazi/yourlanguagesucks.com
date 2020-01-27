---
title: .NET
layout: language
date: 2017
author: theory.org
---
* SortedList uses key-value pairs. There is no standard .NET collection for a list that keeps its items in sort order.
* Changing a collection invalidates **all** iteration operations in **every** thread, even if the current item is unaffected.
This means every foreach can and will throw an exception when you least expect it.
Unless you use locks which in turn can cause deadlocks.
Can be circumvented by using official concurrent or immutable collections or by iterating by using "for" instead of "foreach".
* The MSDN documentation of the (hypothetical) GetFrobnicationInterval would explain that it returns the interval at which the object frobnicates.
It will also state that the method will throw an InvalidOperationException if the frobnication interval cannot be retrieved.
You will also find two "community comments", one of which will contain line noise and the other will inquire about what frobnication is at room temperature in broken English.

