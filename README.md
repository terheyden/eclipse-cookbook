# Eclipse Collections Cookbook

## Contents

- [About the cookbook](#about-the-cookbook)
- [Eclipse Collections dependencies](#eclipse-collections-dependencies)
  * [Maven](#maven)
- [Eclipse Collections types](#eclipse-collections-types)
- [Getting started](#getting-started)
- [Creating Eclipse Collections](#creating-eclipse-collections)
- [Converting from Java collections (JCF) to Eclipse Collections](#converting-from-java-collections-jcf-to-eclipse-collections)
- [Converting from Eclipse Collections to Java Collections](#converting-from-eclipse-collections-to-java-collections)
- [Basic transformations](#basic-transformations)
  * [Other transformations](#other-transformations)
- [Immutable collections](#immutable-collections)

## About the cookbook

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
<!-- You really only need this dependency -->
<!-- https://mvnrepository.com/artifact/org.eclipse.collections/eclipse-collections -->
<dependency>
    <groupId>org.eclipse.collections</groupId>
    <artifactId>eclipse-collections</artifactId>
    <version>${eclipse.version}</version>
</dependency>

<!-- If you use Jackson, this adds support for Eclipse Collections -->
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.datatype/jackson-datatype-eclipse-collections -->
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-eclipse-collections</artifactId>
    <version>${jackson.version}</version>
</dependency>

<!-- Testing dependency - adds "Verify" class of asserts for collections -->
<!-- https://mvnrepository.com/artifact/org.eclipse.collections/eclipse-collections-testutils -->
<dependency>
    <groupId>org.eclipse.collections</groupId>
    <artifactId>eclipse-collections-testutils</artifactId>
    <version>${eclipse.version}</version>
    <scope>test</scope>
</dependency>
```

## Eclipse Collections types

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

## Getting started

- [Chapter 15](https://eclipse.github.io/eclipse-collections-kata/company-kata/#/15) of the [Company Kata](https://github.com/eclipse/eclipse-collections-kata/tree/master/company-kata) has a good overview of adapting from Java to Eclipse Collections
- I suggest going through the [Eclipse Collections Katas](https://github.com/eclipse/eclipse-collections-kata) in order
  - For each Kata, click on its name
  - Scroll down and click on "Presentation Format"
- The [Eclipse Collections guide](https://github.com/eclipse/eclipse-collections/blob/master/docs/guide.md) is a comprehensive read

## Creating Eclipse Collections

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

So for example:
```java
// Java (JCF):
List<String> javaList = new ArrayList<>();

// Eclipse Collections (EC):
MutableList<String> mutableEcList = Lists.mutable.empty();
// Mutation-agnostic version (for example, as a method parameter):
ListIterable<String> ecList = mutableEcList;
```

## Converting from Java collections (JCF) to Eclipse Collections

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

// Immutable types have helper methods:
List<String> jlist4 = Lists.immutable.of("x", "y").castToList();
Collection<String> jcoll1 = Lists.immutable.of("x", "y").castToCollection();
```

## Basic transformations

```java
// select() performs a filter.
ImmutableList<Pet> oldCats = cats.select(cat -> cat.age() > 10);

// reject() performs a negative filter.
ImmutableList<Pet> oldCats2 = cats.reject(cat -> cat.age() <= 10);

// collect() performs a mapping.
ImmutableList<String> catNames = cats.collect(Pet::name);
ImmutableIntList catAges = cats.collectInt(Pet::age);

// flatCollect() of course flattens iterables.
ImmutableList<Pet> allPets = users.flatCollect(User::pets);

// detect() selects one and returns it.
Pet mika = cats.detect(cat -> cat.name().equalsIgnoreCase("Mika"));
int mikaIndex = cats.detectIndex(cat -> cat.name().equalsIgnoreCase("Mika"));
```

### Other transformations

| What I Want | How to Get It |
| ----------- | ------------- |
| Count occurrences of items in a list | `list.toBag()` |
| Group items in a list | `list.groupBy(groupFunc)` |
| Collect from a list but put results in a set | `list.collect(function, Sets.mutable.empty())` |
| Count the number of users in California | `users.countBy(User::getState)` |

## Immutable collections

| What I Want | How to Get It |
| ----------- | ------------- |
| Create an immutable copy from a mutable | `list.toImmutable()` |
| Copy an immutable and add one item | e.g. `myImmutableList.newWith("x");` |
| Copy an immutable and add many items | e.g. `myImmutableList.newWithAll(items);` |
| Copy an immutable and remove one item | e.g. `myImmutableList.newWithout("x");` |
| Copy an immutable and remove many items | e.g. `myImmutableList.newWithoutAll(items);` |
