---
published: true
title: Effective Java 3rd Edition Notes
---
## Effective Java 3rd Edition - Notes

###### Item 55 - Return `Optional` Judiciously
Many a times we require a method to indicate that it cannot return result (which is a valid usecase). We end up returning `null` or throwing exceptions from method for this usecase. Returning `null` value or throwing exception is not perfect way to handle this usecase. Clients need to have a check for `null` before doing anything on returned result from method in case if it can return `null` otherwise the client code may get `NullPointerException`. Throwing an exception is not ideal and defeats the purpose of having exception reserved for exceptional circumstances. Also throwing a new instace of exception is expensive as it requires the whole stacktrace to be captured.

Java 8's `Optional<T>` comes to rescue in these kind of usecases.

- Optional is a immutable container and can hold single non-null `T` reference
- Adds one more level of abstraction and indirection over `T`
- Does not implement `Collection<T>`
- More flexible and easier to use than throwing exception or returning `null`

```Java
public static <E extends Comparable<E>> Optional<E> max(Collection<E> c) {

  if (c.isEmpty()) {
    return Optional.empty();
  }

  E result = null;
  for (E e : c) {
    if (result == null || e.compareTo(result) > 0) {
      result = Objects.requireNonNull(e);
    }
  }
	
  return Optional.of(result);
}
```

- `Optional.empty()` returns an empty optional
- `Optional.of(value)` return optional containing given non-null value
 - If you pass `value` as `null`, it will throw `NullPointerException`
 - `Optional.of()` should be used only with non-null values
- `Optional.ofNullable(value)` accepts a possibly `null` value
 - returns empty optional if value passed is `null`

**_Never return a `null` from an `Optional`-returning method_**

- Optionals are similar in spirit to checked exception in the sense that they force client to confront the fact that the method can return no value

```Java
public static <E extends Comparable<E>> Optional<E> max(Collection<E> c) {
  return c.stream().max(Comparator.naturalOrder());
}
```

- Use `Optional.orElse()` to provide default value
```Java
String lastWordInLexicon = max(words).orElse("No words...");
```
- Or throw a chosen exception with `Optional.orElseThrow()`
```Java
Toy myToy = max(toys).orElseThrow(TemperTantrumException::new);
```
- Get value from Optional using `Optional.get()`
 - can throw `NoSuchElementException` if it is empty
 ```Java
  Element lastNobleGas = max(Elements.NOBLE_GASES).get();
  ```


