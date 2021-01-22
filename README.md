# eclipse-cookbook
Eclipse Collections Cookbook

## Converting Types

| Java Collection | Eclipse Collections Equivalent |
| --------------- | ------------------------------ |
| `ArrayList` | `MutableList` (`FastList`) |
| `HashSet` | `MutableSet` (`UnifiedSet`) |
| `HashMap` | `MutableMap` (`UnifiedMap`) |
| `List.of("x", "y");` | `Lists.mutable.of("x", "y");` |

## New Types

| New Eclipse Collections Type | Java Equivalent |
| ---------------------------- | --------------- |
| `MutableBag` | `Map<K, Integer>` |

## Creating

| What I Want | How to Get It |
| -------- | --- |
| Create a mutable List | `Lists.mutable.empty()` |
| Create an immutable List | `Lists.immutable.empty()` |
| Create a mutable Set | `Sets.mutable.empty()` |
| Create an immutable Set | `Sets.immutable.empty()` |
| Create a mutable Bag | `Bags.mutable.empty()` |
| Create an immutable Bag | `Bags.immutable.empty()` |
| Create a mutable Stack | `Stacks.mutable.empty()` |
| Create an immutable Stack | `Stacks.immutable.empty()` |
| Create a mutable SortedBag | `SortedBags.mutable.empty()` |
| Create an immutable SortedBag | `SortedBags.immutable.empty()` |
| Create a mutable SortedSet | `SortedSets.mutable.empty()` |
| Create an immutable SortedSet | `SortedSets.immutable.empty()` |
| Create a mutable Map | `Maps.mutable.empty()` |
| Create an immutable Map | `Maps.immutable.empty()` |
| Create a mutable SortedMap | `SortedMaps.mutable.empty()` |
| Create an immutable SortedMap | `SortedMaps.immutable.empty()` |
| Create a mutable BiMap | `BiMaps.mutable.empty()` |
| Create an immutable BiMap | `BiMaps.immutable.empty()` |
| Create a mutable Multimap | `Multimaps.mutable.empty()` |
| Create an immutable Multimap | `Multimaps.immutable.empty()` |
| Create a collection with items | e.g. `Lists.mutable.of("x", "y");` |
| Create an immutable from a mutable | e.g. `Lists.mutable.of("x", "y").toImmutable();` |

## Immutables

| What I Want | How to Get It |
| ----------- | ------------- |
| Copy an immutable collection and add one item | e.g. `myImmutableList.newWith("x");` |
| Copy an immutable collection and add many items | e.g. `myImmutableList.newWithAll(items);` |
| Copy an immutable collection and remove one item | e.g. `myImmutableList.newWithout("x");` |
| Copy an immutable collection and remove many items | e.g. `myImmutableList.newWithoutAll(items);` |

## Maven

```
<!-- https://mvnrepository.com/artifact/org.eclipse.collections/eclipse-collections -->
<dependency>
    <groupId>org.eclipse.collections</groupId>
    <artifactId>eclipse-collections</artifactId>
    <version>11.0.0.M1</version>
</dependency>
```
