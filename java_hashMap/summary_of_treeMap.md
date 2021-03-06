# TreeMap用法总结

```java
public class TreeMap<K,V>
extends AbstractMap<K,V>
implements NavigableMap<K,V>, Cloneable, Serializable
```

TreeMap中的元素默认按照keys的自然排序排列。

（对Integer来说，其自然排序就是数字的升序；对String来说，其自然排序就是按照字母表排序）

## 构造函数

- `TreeMap()`：创建一个空TreeMap，keys按照自然排序

  ```java
  TreeMap<Integer, String> treeMap = new TreeMap<>();
  ```

- `TreeMap(Comparator comparator)`：创建一个空TreeMap，按照指定的comparator排序

  ```
  TreeMap<Integer, String> map = new TreeMap<>(Comparator.reverseOrder());
  map.put(3, "val");
  map.put(2, "val");
  map.put(1, "val");
  map.put(5, "val");
  map.put(4, "val");
  System.out.println(map); // {5=val, 4=val, 3=val, 2=val, 1=val}
  ```

- `TreeMap(Map m)`：由给定的map创建一个TreeMap，keys按照自然排序

  ```java
  Map<Integer, String> map = new HashMap<>();
  map.put(1, "val");
  ...
  TreeMap<Integer, String> treeMap = new TreeMap<>(map);
  ```

- `TreeMap(SortedMap m)`：由给定的有序map创建TreeMap，keys按照原顺序排序

## 常用方法

### 增添元素

- `V put(K key, V value)`：将指定映射放入该TreeMap中
- `V putAll(Map map)`：将指定map放入该TreeMap中

### 删除元素

- `void clear()`：清空TreeMap中的所有元素
- `V remove(Object key)`：从TreeMap中移除指定key对应的映射

### 修改元素

- `V replace(K key, V value)`：替换指定key对应的value值
- `boolean replace(K key, V oldValue, V newValue)`：当指定key的对应的value为指定值时，替换该值为新值

### 查找元素

- `boolean containsKey(Object key)`：判断该TreeMap中是否包含指定key的映射
- `boolean containsValue(Object value)`：判断该TreeMap中是否包含有关指定value的映射
- `Map.Entry<K, V> firstEntry()`：返回该TreeMap的第一个（最小的）映射
- `K firstKey()`：返回该TreeMap的第一个（最小的）映射的key
- `Map.Entry<K, V> lastEntry()`：返回该TreeMap的最后一个（最大的）映射
- `K lastKey()`：返回该TreeMap的最后一个（最大的）映射的key
- `v get(K key)`：返回指定key对应的value
- `SortedMap<K, V> headMap(K toKey)`：返回该TreeMap中严格小于指定key的映射集合
- `SortedMap<K, V> subMap(K fromKey, K toKey)`：返回该TreeMap中指定范围的映射集合（大于等于fromKey，小于toKey）

### 遍历接口

- `Set<Map<K, V>> entrySet()`：返回由该TreeMap中的所有映射组成的Set对象
- `void forEach(BiConsumer<? super K,? super V> action)`：对该TreeMap中的每一个映射执行指定操作
- `Collection<V> values()`：返回由该TreeMap中所有的values构成的集合

### 其他方法

- `Object clone()`：返回TreeMap实例的浅拷贝
- `Comparator<? super K> comparator() `：返回给该TreeMap的keys排序的comparator，若为自然排序则返回null
- `int size()`：返回该TreepMap中包含的映射的数量

```java
TreeMap<Integer, String> treeMap = new TreeMap<>();
treeMap.put(1, "a");
treeMap.put(2, "b");
treeMap.put(3, "c");
treeMap.put(4, "d"); // treeMap: {1=a, 2=b, 3=c, 4=d}

treeMap.remove(4); // treeMap: {1=a, 2=b, 3=c}
int sizeOfTreeMap = treeMap.size(); // sizeOfTreeMap: 3

treeMap.replace(2, "e"); // treeMap: {1=a, 2=e, 3=c}

Map.Entry entry = treeMap.firstEntry(); // entry: 1 -> a
Integer key = treeMap.firstKey(); // key: 1
entry = treeMap.lastEntry(); // entry: 3 -> c
key = treeMap.lastKey(); // key: 3
String value = treeMap.get(3); // value: c
SortedMap sortedMap = treeMap.headMap(2); // sortedMap: {1=a}
sortedMap = treeMap.subMap(1, 3); // sortedMap: {1=a, 2=e}

Set setOfEntry = treeMap.entrySet(); // setOfEntry: [1=a, 2=e, 3=c]
Collection<String> values = treeMap.values(); // values: [a, e, c]
treeMap.forEach((integer, s) -> System.out.println(integer + "->" + s)); 
// output：
// 1 -> a
// 2 -> e
// 3 -> c
```

## 遍历方式

- for循环

  ```java
  for (Map.Entry entry : treeMap.entrySet()) {
  		System.out.println(entry);
  }
  ```

- 迭代器循环

  ```java
  Iterator iterator = treeMap.entrySet().iterator();
  while (iterator.hasNext()) {
  		System.out.println(iterator.next());
  }
  ```

---

## 补充：如何选择合适的Map

- HashMap可实现快速存储和检索，但其缺点是其包含的元素是无序的，这导致它在存在大量迭代的情况下表现不佳。
- LinkedHashMap保留了HashMap的优势，且其包含的元素是有序的。它在有大量迭代的情况下表现更好。
- TreeMap能便捷的实现对其内部元素的各种排序，但其一般性能比前两种map差。

**LinkedHashMap映射减少了HashMap排序中的混乱，且不会导致TreeMap的性能损失。**

---

#### 参考

- [Class TreeMap<K,V>](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)
- [A Guide to TreeMap in Java](https://www.baeldung.com/java-treemap)

