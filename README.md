# Eclipse Collections Cookbook

## Contents

- [Notes about the cookbook](#notes-about-the-cookbook)
- [Eclipse Collections dependencies](#eclipse-collections-dependencies)
  * [Maven](#maven)
- [What are the Eclipse Collections types?](#what-are-the-eclipse-collections-types)
- [Creating](#creating)
- [Converting from Java streams to Eclipse Collections](#converting-from-java-streams-to-eclipse-collections)
- [Converting from Eclipse Collections to Java Collections](#converting-from-eclipse-collections-to-java-collections)
- [Transformations](#transformations)
- [Immutables](#immutables)

## Notes about the cookbook

- Every Eclipse Collections type has both a mutable and immutable equivalent. For example:
```java
Lists.mutable.empty();
Lists.immutable.empty();
```
- For brevity, only one will be referenced in each section
- I like to use `List` types in examples, but they usually apply to _all_ Eclipse Collections types unless otherwise noted
- I abbreviate `Eclipse Collections` as `EC` in places
- These two methods are equivalent for all EC types:
```java
Lists.mutable.of("x", "y");
Lists.mutable.with("x", "y");
```
- In the cookbook I always use `.of()` because it is most similar to Java 9 syntax

## Eclipse Collections dependencies

### Maven

```
<!-- Eclipse Collections - advanced data structures -->
<!-- https://mvnrepository.com/artifact/org.eclipse.collections/eclipse-collections -->
<dependency>
    <groupId>org.eclipse.collections</groupId>
    <artifactId>eclipse-collections</artifactId>
    <version>11.0.0.M1</version>
</dependency>

<!-- "Verify" class of asserts for Eclipse Collections -->
<!-- https://mvnrepository.com/artifact/org.eclipse.collections/eclipse-collections-testutils -->
<dependency>
    <groupId>org.eclipse.collections</groupId>
    <artifactId>eclipse-collections-testutils</artifactId>
    <version>11.0.0.M1</version>
    <scope>test</scope>
</dependency>
```

## What are the Eclipse Collections types?

| EC Type | Java Equivalent | Useful for |
| ------- | --------------- | ---------- |
| `List<>` | `List<>` | Non-unique ordered collection |
| `Set<>` | `Set<>` | Unique non-ordered collection |
| `SortedSet<>` | `SortedSet<>` | Unique, sorted, ordered collection |
| `Bag<>` | `Map<K, Integer>` | Storing occurrences of items |
| `SortedBag<>` | `SortedMap<K, Integer>` | Storing and sorting by occurrence |
| `Stack<>` | `Stack<>` | LIFO queue |
| `Map<>` | `Map<>` | Collection of key-value pairs |
| `SortedMap<>` | `SortedMap<>` | Sorted, ordered collection of key-value pairs |
| `BiMap<>` | Use two maps | Map that can lookup in both directions with unique keys and values |
| `Multimap<>` | `Map<K, Collection<V>>` | Map with multiple values |
| `Partition<>` | `Map<Boolean, Collection<V>>` | Collection of selected items and rejected items |
| `Pair<>` | `Map.Entry<>` | Grouping of two items |
| `PrimitiveList<>` | Boxed | Extremely memory-efficient collection |
| `PrimitiveSet<>` | Boxed | Extremely memory-efficient collection |
| `PrimitiveStack<>` | Boxed | Extremely memory-efficient collection |
| `PrimitiveBag<>` | Boxed | Extremely memory-efficient collection |
| `PrimitiveMap<>` | Boxed | Extremely memory-efficient collection |
| `PrimitiveStream<>` | Boxed* | Extremely memory-efficient collection |

- *Java does have IntStream, LongStream, and DoubleStream.
- Primitive types are: `Int`, `Long`, `Float`, `Char`, `Byte`, `Boolean`, `Short`, `Double`

## Creating

| What I want | How to make | Type hierarchy |
| ----------- | ----------- | -------------- |
| Create a List | `Lists.mutable.empty()` | `MutableList`, `ListIterable` |
| Create a Set | `Sets.mutable.empty()` | `MutableSet`, `UnsortedSetIterable` |
| Create a SortedSet | `SortedSets.mutable.empty()` | `MutableSortedSet`, `SortedSetIterable` |
| Create a Bag | `Bags.mutable.empty()` | `MutableBag`, `UnsortedBag` |
| Create a SortedBag | `SortedBags.mutable.empty()` | `MutableSortedBag`, `SortedBag` |
| Create a Stack | `Stacks.mutable.empty()` | `MutableStack`, `StackIterable` |
| Create a Map | `Maps.mutable.empty()` | `MutableMap`, `UnsortedMapIterable` |
| Create a SortedMap | `SortedMaps.mutable.empty()` | `MutableSortedMap`, `SortedMapIterable` |
| Create a BiMap | `BiMaps.mutable.empty()` | `MutableBiMap`, `BiMap` |
| Create a Multimap | `Multimaps.mutable.empty()` | `MutableListMultimap`, `ListMultimap` |
| Create a Pair | `Tuples.pair("one", 2)` | `Pair` |
| Create a Triple | `Tuples.triple(1, "two", 3)` | `Triple` |
| Create an Interval (range) | `IntInterval.from(0).to(9).by(2)` | `IntInterval`, `IntIterable` |

## Converting from Java streams to Eclipse Collections

```java
// Use ofAll() or withAll():

List<String> jlist1 = List.of("x", "y");
ImmutableList<String> ecList1 = Lists.immutable.ofAll(jlist1);

Map<String, String> jmap1 = Map.of("x", "y");
ImmutableMap<String, String> ecMap1 = Maps.immutable.ofAll(jmap1);

// With streams, use Collectors2:

List<String> jlist2 = List.of("x", "y");

ImmutableList<String> ecList2 = jlist2.stream()
    .collect(Collectors2.toImmutableList());
```

## Converting from Eclipse Collections to Java Collections

```java
// Mutable types can be cast directly to Java types:
List<String> jlist3 = Lists.mutable.of("x", "y");
Map<String, String> jmap2 = Maps.mutable.of("x", "y");

// Immutable types have a helper method:
List<String> jlist4 = Lists.immutable.of("x", "y").castToList();
Collection<String> jcoll1 = Lists.immutable.of("x", "y").castToCollection();
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
