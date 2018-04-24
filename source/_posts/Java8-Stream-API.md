---
title: Java8 Stream API
date: 2018-04-25 00:31:05
tags: [java]
---

### Stream

**Stream分为中间操作（Intermediate operations）和终止操作（Terminal operations）**

- `Intermediate`：一个流可以后面跟随零个或多个 intermediate 操作。其目的主要是打开流，做出某种程度的数据映射/过滤，然后返回一个新的流，交给下一个操作使用。这类操作都是惰性化的（lazy），就是说，仅仅调用到这类方法，并没有真正开始流的遍历。常见操作有 map (mapToInt, flatMap 等)、 filter、 distinct、 sorted、 peek、 limit、 skip、 parallel、 sequential、 unordered。

- `Terminal`：一个流只能有一个 terminal 操作，当这个操作执行后，流就被使用“光”了，无法再被操作。所以这必定是流的最后一个操作。Terminal 操作的执行，才会真正开始流的遍历，并且会生成一个结果，或者一个 side effect。常见操作有 forEach、 forEachOrdered、 toArray、 reduce、 collect、 min、 max、 count、 anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 iterator。

**关于`Short-circuiting`**
- 对于一个 intermediate 操作，如果它接受的是一个无限大（infinite/unbounded）的 Stream，但返回一个有限的新 Stream。
- 对于一个 terminal 操作，如果它接受的是一个无限大的 Stream，但能在有限的时间计算出结果。
- 常见操作有 anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 limit

以下摘自 [这里](https://github.com/CarpenterLee/JavaLambdaInternals/blob/master/6-Stream%20Pipelines.md)

<table width="600"><tr><td colspan="3" align="center"  border="0">Stream操作分类</td></tr><tr><td rowspan="2"  border="1">中间操作(Intermediate operations)</td><td>无状态(Stateless)</td><td>unordered() filter() map() mapToInt() mapToLong() mapToDouble() flatMap() flatMapToInt() flatMapToLong() flatMapToDouble() peek()</td></tr><tr><td>有状态(Stateful)</td><td>distinct() sorted() sorted() limit() skip() </td></tr><tr><td rowspan="2"  border="1">结束操作(Terminal operations)</td><td>非短路操作</td><td>forEach() forEachOrdered() toArray() reduce() collect() max() min() count()</td></tr><tr><td>短路操作(short-circuiting)</td><td>anyMatch() allMatch() noneMatch() findFirst() findAny()</td></tr></table>

Stream上的所有操作分为两类：中间操作和结束操作，中间操作只是一种标记，只有结束操作才会触发实际计算。中间操作又可以分为无状态的(Stateless)和有状态的(Stateful)，无状态中间操作是指元素的处理不受前面元素的影响，而有状态的中间操作必须等到所有元素处理之后才知道最终结果，比如排序是有状态操作，在读取所有元素之前并不能确定排序结果；结束操作又可以分为短路操作和非短路操作，短路操作是指不用处理全部元素就可以返回结果，比如找到第一个满足条件的元素。之所以要进行如此精细的划分，是因为底层对每一种情况的处理方式不同。


**Stream API**

**`Stream<T> filter(Predicate<? super T> predicate);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .filter(str -> str.length() > 3);
```
```            
Stream<String>                Stream<String> 
  +-------+                                  
  | one   |                                  
  +-------+                                  
  | two   |                                  
  +-------+                     +-------+    
  | three |     filter()        | three |    
  +-------+  --------------->   +-------+    
  | four  |  str.length() > 3   | four  |    
  +-------+                     +-------+    
  | five  |                     | five  |    
  +-------+                     +-------+          
```

**`<R> Stream<R> map(Function<? super T, ? extends R> mapper);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .map(String::toUpperCase);
```
```
Stream<String>                Stream<String>
  +-------+                     +-------+   
  | one   |                     | ONE   |   
  +-------+                     +-------+   
  | two   |                     | TWO   |   
  +-------+                     +-------+   
  | three |       map()         | THREE |   
  +-------+  --------------->   +-------+   
  | four  |    toUpperCase      | FOUR  |   
  +-------+                     +-------+   
  | five  |                     | FIVE  |   
  +-------+                     +-------+   
```

**`IntStream mapToInt(ToIntFunction<? super T> mapper);`**

```java
Stream.of("1", "2", "3", "4", "5")
            .mapToInt(Integer::valueOf);
```
```
Stream<String>                  IntStream
  +-------+                     +-------+   
  |   1   |                     |   1   |   
  +-------+                     +-------+   
  |   2   |                     |   2   |   
  +-------+                     +-------+   
  |   3   |     mapToInt()      |   3   |   
  +-------+  --------------->   +-------+   
  |   4   |  Integer.valueOf    |   4   | 
  +-------+                     +-------+   
  |   5   |                     |   5   |   
  +-------+                     +-------+   
```

**`LongStream mapToLong(ToLongFunction<? super T> mapper);`**

```java
Stream.of("1", "2", "3", "4", "5")
            .mapToLong(Long::valueOf);
```
```
Stream<String>                 LongStream
  +-------+                     +-------+   
  |   1   |                     |   1   |   
  +-------+                     +-------+   
  |   2   |                     |   2   |   
  +-------+                     +-------+   
  |   3   |     mapToLong()     |   3   |   
  +-------+  --------------->   +-------+   
  |   4   |    Long.valueOf     |   4   | 
  +-------+                     +-------+   
  |   5   |                     |   5   |   
  +-------+                     +-------+   
```

**`DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper);`**

```java
Stream.of("1.1", "2.2", "3.3", "4.4", "5.5")
            .mapToDouble(Double::valueOf);
```
```
Stream<String>                 DoubleStream
  +-------+                     +-------+   
  |  1.1  |                     |  1.1  |   
  +-------+                     +-------+   
  |  2.2  |                     |  2.2  |   
  +-------+                     +-------+   
  |  3.3  |    mapToDouble()    |  3.3  |   
  +-------+  --------------->   +-------+   
  |  4.4  |   Double.valueOf    |  4.4  | 
  +-------+                     +-------+   
  |  5.5  |                     |  5.5  |   
  +-------+                     +-------+   
```

**`<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);`**

```java
Stream.of(Arrays.asList("one", "two"), Arrays.asList("three", "four", "five"))
            .flatMap(list -> list.stream());
```
```
Stream<List<String>                      Stream<String>        
  +-----------+                                                
  | +-------+ |                                                
  | | one   | |                                                
  | +-------+ |                                                
  | | two   | |                            +-------+           
  | +-------+ |                            | one   |           
  |           |         flatMap()          +-------+           
  | +-------+ |      --------------->      | two   |           
  | | three | |                            +-------+           
  | +-------+ |                            | three |           
  | | four  | |                            +-------+           
  | +-------+ |                            | four  |           
  | | five  | |                            +-------+           
  | +-------+ |                            | five  |           
  +-----------+                            +-------+   
```

**`IntStream flatMapToInt(Function<? super T, ? extends IntStream> mapper);`**

*Similar to `flatMap`*

**`LongStream flatMapToLong(Function<? super T, ? extends LongStream> mapper);`**

*Similar to `flatMap`*

**`DoubleStream flatMapToDouble(Function<? super T, ? extends DoubleStream> mapper);`**

*Similar to `flatMap`*

**`Stream<T> distinct();`**

```java
Stream.of("one", "two", "three", "four", "four")
            .distinct();
```
```
Stream<String>                Stream<String>
  +-------+                                
  | one   |                                
  +-------+                     +-------+  
  | two   |                     | one   |  
  +-------+                     +-------+  
  | three |     distinct()      | two   |  
  +-------+  --------------->   +-------+  
  | four  |                     | three |  
  +-------+                     +-------+  
  | four  |                     | four  |  
  +-------+                     +-------+  
```

**`Stream<T> sorted();`**

*默认自然顺序等价于`sorted(Comparator.naturalOrder())`* 

```java
Stream.of("one", "two", "three", "four", "five")
            .sorted();
```
```
Stream<String>                Stream<String>
  +-------+                     +-------+   
  | one   |                     | five  |   
  +-------+                     +-------+   
  | two   |                     | four  |   
  +-------+                     +-------+   
  | three |       sorted()      | one   |   
  +-------+  --------------->   +-------+   
  | four  |                     | three |   
  +-------+                     +-------+   
  | five  |                     | two   |   
  +-------+                     +-------+   
```

**`Stream<T> sorted(Comparator<? super T> comparator);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .sorted(Comparator.naturalOrder());
```
```
Stream<String>                Stream<String>
  +-------+                     +-------+   
  | one   |                     | five  |   
  +-------+                     +-------+   
  | two   |                     | four  |   
  +-------+                     +-------+   
  | three |       sorted()      | one   |   
  +-------+  --------------->   +-------+   
  | four  |     naturalOrder    | three |   
  +-------+                     +-------+   
  | five  |                     | two   |   
  +-------+                     +-------+   
```

**`Stream<T> peek(Consumer<? super T> action);`**

```java
       Stream.of("one", "two", "three", "four", "five")
            .peek(e -> System.out.println("Peek value: " + e))
            .count();
```

**`Stream<T> limit(long maxSize);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .limit(4);
```

```
Stream<String>                Stream<String>
  +-------+                                
  | one   |                                
  +-------+                     +-------+  
  | two   |                     | one   |  
  +-------+                     +-------+  
  | three |       limit()       | two   |  
  +-------+  --------------->   +-------+  
  | four  |                     | three |  
  +-------+                     +-------+  
  | five  |                     | four  |  
  +-------+                     +-------+  
```

**`Stream<T> skip(long n);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .skip(2);
```
```
Stream<String>                Stream<String>
  +-------+                                
  | one   |                                
  +-------+                      
  | two   |                       
  +-------+                     +-------+  
  | three |       skip()        | three |  
  +-------+  --------------->   +-------+  
  | four  |                     | four  |  
  +-------+                     +-------+  
  | five  |                     | five  |  
  +-------+                     +-------+  
```

**`void forEach(Consumer<? super T> action);`**

```java
Stream.of("one", "two", "three", "four", "five")
            .forEach(str -> {
                System.out.println(str.toUpperCase());
            });
```

**`void forEachOrdered(Consumer<? super T> action);`**

*非并行化情况下等价于`forEach`，并行化情况下`forEachOrdered`始终保持顺序， 但`forEach`不是*

```java
Stream.of("one", "two", "three", "four", "five")
            .parallel()
            .forEach(System.out::println);

Stream.of("one", "two", "three", "four", "five")
            .parallel()
            .forEachOrdered(System.out::println);
```

**`Object[] toArray();`**

```java
Stream.of("one", "two", "three", "four", "five")
            .toArray();
```

```
Stream<String>                  String[] 
  +-------+                     +-------+   
  | one   |                     | five  |   
  +-------+                     +-------+   
  | two   |                     | four  |   
  +-------+                     +-------+   
  | three |      toArray()      | one   |   
  +-------+  --------------->   +-------+   
  | four  |                     | three |   
  +-------+                     +-------+   
  | five  |                     | two   |   
  +-------+                     +-------+   
```

**`<A> A[] toArray(IntFunction<A[]> generator);`**

*建议采用该方法*

```
Stream.of("one", "two", "three", "four", "five")
            .toArray(String[]::new);
```

**`T reduce(T identity, BinaryOperator<T> accumulator);`**

```java
Stream.of(1, 2, 3, 4)
            .reduce(0, (acc, element) -> acc + element);
```

**`Optional<T> reduce(BinaryOperator<T> accumulator);`**

```java
Stream.of(1, 2, 3, 4)
            .reduce((acc, element) -> acc + element)
            .ifPresent(System.out::println);
```

**`<U> U reduce(U identity,
                BiFunction<U, ? super T, U> accumulator,
                BinaryOperator<U> combiner);`**

*`并行化` 的时候 `combiner` 参数才有效。*

```java
Arrays.asList(1, 2, 3, 4, 5, 6).parallelStream()
            .reduce(0,
                (sum, p) -> {
                    System.out.format("accumulator: sum=%s; person=%s\n", sum, p);
                    return sum += p;
                },
                (sum1, sum2) -> {
                    System.out.format("combiner: sum1=%s; sum2=%s\n", sum1, sum2);
                    return sum1 + sum2;
                });
```

**`<R> R collect(Supplier<R> supplier,
                BiConsumer<R, ? super T> accumulator,
                BiConsumer<R, R> combiner);`**

*`supplier`创造目标类型实例， `accumulator` 将当元素添加到目标中， `combiner` 将中间状态的多个结果整合到一起*
*`并行化` 的时候 `combiner` 参数才有效。*

```java
Stream.of("one", "two", "three", "four", "five")
            .parallel()
            .collect(
                () -> new ArrayList<>(),
                (list, item) -> list.add(item),
                (one, two) -> one.addAll(two)
            ).forEach(System.out::println);
```
**`<R, A> R collect(Collector<? super T, A, R> collector);`**

*流转换为其它数据结构， 具体参考`Collector`，`Collectors`*

```java
Stream.of("one", "two", "three", "four", "five")
            .collect(Collectors.toList());

Stream.of("one", "two", "three", "four", "five")
            .collect(Collectors.toSet());
```

```
Stream<String>                 List<String>
  +-------+                     +-------+   
  | one   |                     | five  |   
  +-------+                     +-------+   
  | two   |                     | four  |   
  +-------+                     +-------+   
  | three |      collect()      | one   |   
  +-------+  --------------->   +-------+   
  | four  | Collectors.toList() | three |   
  +-------+                     +-------+   
  | five  |                     | two   |   
  +-------+                     +-------+   
```

**`Optional<T> min(Comparator<? super T> comparator);`**

```java
Stream.of(1, 2, 3, 4)
            .min(Comparator.naturalOrder());
```
```
Stream<Integer>           Optional<Integer>  
  +-------+                                  
  |   1   |                                  
  +-------+                                  
  |   2   |                                  
  +-------+                                  
  |   3   |     min()       +-------+        
  +-------+  ----------->   |   1   |        
  |   4   |                 +-------+        
  +-------+                                  
```

**`Optional<T> max(Comparator<? super T> comparator);`**

```java
Stream.of(1, 2, 3, 4)
            .max(Comparator.naturalOrder());
```
```
Stream<Integer>           Optional<Integer>  
  +-------+                                  
  |   1   |                                  
  +-------+                                  
  |   2   |                                  
  +-------+                                  
  |   3   |     max()       +-------+        
  +-------+  ----------->   |   4   |        
  |   4   |                 +-------+        
  +-------+                                  
```

**`long count();`**
```java
Stream.of(1, 2, 3, 4)
            .count();
```
```
Stream<Integer>               long  
  +-------+                                  
  |   1   |                                  
  +-------+                                  
  |   2   |                                  
  +-------+                                  
  |   3   |     count()     +-------+        
  +-------+  ----------->   |   4   |        
  |   4   |                 +-------+        
  +-------+                                  
```

**`boolean anyMatch(Predicate<? super T> predicate);`**

*流中存在元素匹配，即为`true`*

```java
Stream.of("one", "two", "three", "four", "five")
            .anyMatch(str -> str.contains("tw"));
```

**`boolean allMatch(Predicate<? super T> predicate);`**

*流中全部元素匹配，才为`true`*

```java
Stream.of("one", "two", "four")
            .allMatch(str -> str.contains("o"));
```

**`boolean noneMatch(Predicate<? super T> predicate);`**

*流中全部元素均不匹配，才为`true`*

```java
Stream.of("one", "two", "three", "four", "five")
            .noneMatch(str -> str.contains("six"));
```

**`Optional<T> findFirst();`**

*返回第一个元素*

```java
Stream.of("one", "two", "three", "four", "five")
            .findFirst().ifPresent(System.out::println);
```

**`Optional<T> findAny();`**

*`并行化`时“随机”选择一个元素返回，非并行化和`findFirst`类似*
```java
Stream.of("one", "two", "three", "four", "five", "six")
            .parallel()
            .findAny().ifPresent(System.out::println);
```
