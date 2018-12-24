---
published: true
title: Effective Java 3rd Edition Notes
---
## Effective Java 3rd Edition - Notes

###### Item 55 - Return `Optional` Judiciously
Many a times we require a method to indicate that it cannot return result (which is a valid usecase). We end up returning `null` or throwing exceptions from method for this usecase. Returning `null` value or throwing exception is not perfect way to handle this usecase. Clients need to have a check for `null` before doing anything on returned result from method in case if it can return `null` otherwise the client code may get `NullPointerException`. Throwing an exception is not ideal and defeats the purpose of having exception reserved for exceptional circumstances. Also throwing a new instace of exception is expensive as it requires the whole stacktrace to be captured.

Java 8's `Optional<T>` comes to help in these kind of usecases.