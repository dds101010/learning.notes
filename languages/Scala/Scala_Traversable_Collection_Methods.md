### _Traversable_ Collection Methods

**Predicate**: A predicate is simply a method, function, or anonymous function that
takes one or more parameters and returns a Boolean value.



```scala
val c1 = IndexedSeq.range(1, 24, 3)
//> c1  : IndexedSeq[Int] = Vector(1, 4, 7, 10, 13, 16, 19, 22)

val c2 = IndexedSeq.range(10, 42, 3)
//> c2  : IndexedSeq[Int] = Vector(10, 13, 16, 19, 22, 25, 28, 31, 34, 37, 40)

val c3 = Vector(c1, c2)
//> c3  : scala.collection.immutable.Vector[IndexedSeq[Int]] = Vector(Vector(1, 4, 7, 10, 13, 16, 19, 22), Vector(10, 13, 16, 19, 22, 25, 28, 31, 34, 37, 40))

val c4 = Vector(Some(4), None, Some(2), None)
//> c4  : scala.collection.immutable.Vector[Option[Int]] = Vector(Some(4), None, Some(2), None)

val c5 = new scala.collection.mutable.ArrayBuffer[Int]()
//> c5  : scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer()

val c6 = Vector(2, 4, 5, 6, 7)
//> c6  : scala.collection.immutable.Vector[Int] = Vector(2, 4, 5, 6, 7)
```



### Predicate based functions

- `count()`: count of elements returning true for condition

  ```scala
  c1 count (_ % 2 == 0)
  //> res1: Int = 4
  ```

- `exists()`: return true if **ANY** element results in true for condition. (Similar to `any()` of python)

  ```scala
  c2 exists (_ % 5 == 0)
  //> res4: Boolean = true
  ```

- `forall()`: returns true if **ALL** elements result in true for condition (Similar to `all()` of python)

  ```scala
  c2 forall (_ % 2 == 0)
  //> res5: Boolean = false
  ```

- `filter()`: returns elements from the collection for which condition results in true. empty collection if no element passes the conditions.

  ```scala
  c1 filter (_ % 2 == 0)
  //> res6: IndexedSeq[Int] = Vector(4, 10, 16, 22)
  ```

- `filterNot()`: opposite of `filter()`

  ```scala
  c1 filterNot (_ % 2 == 0)
  //> res7: IndexedSeq[Int] = Vector(1, 7, 13, 19)
  ```

- `find()`: returns the first element that matches the predicate as `Some[A]` . Returns `None` if no match is found

  ```scala
  c1 find (_ % 5 == 0)
  //> res8: Option[Int] = Some(10)
  ```

- `parition()`: returns a tuple `(a, b)` where `a` is collection of all the elements that results in `true` and `b` is collection of elements that results in `false`

  ```scala
  Vector(1, 2, 3 4).partition(_ % 2 == 0)
  //> res12: (scala.collection.immutable.Vector[Int], scala.collection.immutable.Vector[Int]) = (Vector(2, 4),Vector(1, 3))

  val (even, odd) = Vector.range(1, 10).partition(_ % 2 == 0)
  ```

  ​

  // tbc

