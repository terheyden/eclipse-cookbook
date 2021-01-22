# eclipse-cookbook
Eclipse Collections Cookbook

## Converting Types

| Java Collection | Eclipse Collections Equivalent |
| --------------- | ------------------------------ |
| `ArrayList` | `MutableList` (`FastList`) |
| `HashSet` | `MutableSet` (`UnifiedSet`) |
| `HashMap` | `MutableMap` (`UnifiedMap`) |
| `List.of("x", "y")` | `Lists.mutable.of("x", "y")` |

## New Types With No Direct Java Equivalent

| New Eclipse Collections Type | Java Equivalent | Example Use |
| ---------------------------- | --------------- | ----------- |
| `Bag` | `Map<K, Integer>` | Track the count of each UserType |
| `Multimap` | `Map<K, Collection<V>>` | Group users by last name |
| `IntList` | `List<Integer>` | List of user IDs |

## Creating

| What I Want | Mutable Version | Immutable Version |
| ----------- | --------------- | ----------------- |
| Create a List | `Lists.mutable.empty()` | `Lists.immutable.empty()` |
| Create a Set | `Sets.mutable.empty()` | `Sets.immutable.empty()` |
| Create a SortedSet | `SortedSets.mutable.empty()` | `SortedSets.immutable.empty()` |
| Create a Bag | `Bags.mutable.empty()` | `Bags.immutable.empty()` |
| Create a SortedBag | `SortedBags.mutable.empty()` | `SortedBags.immutable.empty()` |
| Create a Stack | `Stacks.mutable.empty()` | `Stacks.immutable.empty()` |
| Create a Map | `Maps.mutable.empty()` | `Maps.immutable.empty()` |
| Create a SortedMap | `SortedMaps.mutable.empty()` | `SortedMaps.immutable.empty()` |
| Create a BiMap | `BiMaps.mutable.empty()` | `BiMaps.immutable.empty()` |
| Create a Multimap | `Multimaps.mutable.empty()` | `Multimaps.immutable.empty()` |
| Create a collection with items | e.g. `Lists.mutable.of("x", "y")` | e.g. `Lists.immutable.of("x", "y")` |
| Create a Pair | `Tuples.pair("key", "val")` | n/a |

## Converting from Java streams to Eclipse Collections

```java
ImmutableList<String> list = List.of("x", "y")
    .stream()
    .collect(Collectors2.toImmutableList());
```

## Transformations

| What I Want | How to Get It |
| ----------- | ------------- |
| Count occurrences of items in a list | `list.toBag()` |
| Group items in a list | `list.groupBy(groupFunc)` |
| Collect from a list but put results in a set | `list.collect(function, Sets.mutable.empty())` |
| Count the number of users in California | `users.countBy(User::getState)` |

## Immutables

| What I Want | How to Get It |
| ----------- | ------------- |
| Create an immutable copy from a mutable | `list.toImmutable()` |
| Copy an immutable and add one item | e.g. `myImmutableList.newWith("x");` |
| Copy an immutable and add many items | e.g. `myImmutableList.newWithAll(items);` |
| Copy an immutable and remove one item | e.g. `myImmutableList.newWithout("x");` |
| Copy an immutable and remove many items | e.g. `myImmutableList.newWithoutAll(items);` |

## Maven

```
<!-- https://mvnrepository.com/artifact/org.eclipse.collections/eclipse-collections -->
<dependency>
    <groupId>org.eclipse.collections</groupId>
    <artifactId>eclipse-collections</artifactId>
    <version>11.0.0.M1</version>
</dependency>
```

## Notes

- In examples I default to using `List`, but the content usually applies to _all_ Eclipse Collections types, e.g. `Set`, `Bag`, `Multimap`, etc.
- During object creation, I default to using `.of()`, e.g. `Lists.mutable.of("x", "y")` and not `.with()`, e.g. `Lists.mutable.with("x", "y")`, because it is most similar to Java 9 syntax
