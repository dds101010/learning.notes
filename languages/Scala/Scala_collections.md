### Scala collections

_Source_: [Scala cookbook](http://scalacookbook.com/) by Alvin Alexander



#### Go-to Collection classes

|                        | Immutable | Mutable       |
| ---------------------- | --------- | ------------- |
| Indexed                | `Vector`  | `ArrayBuffer` |
| Linear  (Linked lists) | `List`    | `ListBuffer`  |

#### Sequence - `Immutable`

|          | IndexedSeq | LinearSeq | Description                                                  |
| -------- | :--------: | :-------: | ------------------------------------------------------------ |
| `List`   |            |     ✓     | A  singly linked list. Suited for recursive algorithms that work by splitting  the head from the remainder of the list. |
| `Queue`  |            |     ✓     | A first-in, first-out  data structure.                       |
| `Range`  |     ✓      |           | A range of integer  values.                                  |
| `Stack`  |            |     ✓     | A last-in, first-out  data structure.                        |
| `Stream` |            |     ✓     | Similar  to  List , but it’s lazy and  persistent. Good for a large or infinite sequence, similar to a Haskell  List. |
| `String` |     ✓      |           | Can be treated as an  immutable, indexed sequence of characters. |
| `Vector` |     ✓      |           | The  “go to” immutable, indexed sequence. The Scaladoc describes it as,  “Implemented as a set of nested arrays that’s efficient at splitting and  joining.” |

#### Sequence - `Mutable`

|                    | IndexedSeq | LinearSeq | Description                                                  |
| ------------------ | :--------: | :-------: | ------------------------------------------------------------ |
| `Array`            |     ✓      |           | Backed  by a Java array, its elements are mutable, but it can’t change in size. |
| `ArrayBuffer`      |     ✓      |           | The  “go to” class for a mutable, sequential collection. The amortized cost for  appending elements is constant. |
| `ArrayStack`       |     ✓      |           | A  last-in, first-out data structure. Prefer over  Stack when performance is important. |
| `DoubleLinkedList` |            |     ✓     | Like  a singly linked list, but with a  prev  method as well. The    documentation states, “The additional links make element removal very  fast.” |
| `LinkedList`       |            |     ✓     | A  mutable, singly linked list.                              |
| `ListBuffer`       |            |     ✓     | Like  an  ArrayBuffer , but backed by a list.  The documentation states,    “If you plan to convert the buffer to a list, use  ListBuffer instead of    ArrayBuffer .” Offers constant-time prepend and append; most other  operations are linear. |
| `MutableList`      |            |     ✓     | A  mutable, singly linked list with constant-time append.    |
| `Queue`            |            |     ✓     | A  first-in, first-out data structure.                       |
| `Stack`            |            |     ✓     | A  last-in, first-out data structure. (The documentation suggests that an  ArrayStack is slightly more efficient.) |
| `StringBuilder`    |     ✓      |           | Used  to build strings, as in a loop. Like the Java   StringBuilder |

#### Map

|                 | Immutable | Mutable | Description                                                  |
| --------------- | :-------: | :-----: | ------------------------------------------------------------ |
| `HashMap`       |     ✓     |    ✓    | The  immutable version “implements maps using a hash trie”; the mutable version  “implements maps using a hashtable.” |
| `LinkedHashMap` |           |    ✓    | “Implements  mutable maps using a hashtable.” Returns elements by the order in which they  were inserted. |
| `ListMap`       |     ✓     |    ✓    | A  map implemented using a list data structure. Returns elements in the opposite  order by which they were inserted, as though each element is inserted at the  head of the map. |
| `Map`           |     ✓     |    ✓    | The  base map, with both mutable and immutable implementations. |
| `SortedMap`     |     ✓     |         | A  base trait that stores its keys in sorted order. (Creating a variable as a  SortedMap currently returns a   TreeMap.) |
| `TreeMap`       |     ✓     |         | An  immutable, sorted map, implemented as a red-black tree.  |
| `WeakHashMap`   |           |    ✓    | A  hash map with weak references, it’s a wrapper around    java.util.WeakHashMap . |

#### Set

|                 | Immutable | Mutable | Description                                                  |
| --------------- | :-------: | :-----: | ------------------------------------------------------------ |
| `BitSet`        |     ✓     |    ✓    | A  set of “non-negative integers represented as variable-size arrays of bits  packed into 64-bit words.” Used to save memory when you have a set of  integers. |
| `HashSet`       |     ✓     |    ✓    | The  immutable version “implements sets using a hash trie”; the mutable version  “implements sets using a hashtable.” |
| `LinkedHashSet` |           |    ✓    | A  mutable set implemented using a hashtable. Returns elements in the order in  which they were inserted. |
| `ListSet`       |     ✓     |         | A  mutable set implemented using a hashtable. Returns elements in the order in  which they were inserted. |
| `TreeSet`       |     ✓     |    ✓    | The  immutable version “implements immutable sets using a tree.” The mutable  version is a mutable  SortedSet with  “an immutable AVL Tree as underlying data structure.” |
| `Set`           |     ✓     |    ✓    | Generic  base traits, with both mutable and immutable implementations. |
| `SortedSet`     |     ✓     |    ✓    | A  base trait. (Creating a variable as a   SortedSet returns a  TreeSet .) |