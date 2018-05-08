_Reference_: [Programming in Scala](https://www.artima.com/pins1ed/basic-types-and-operations.html)

## Chapter 5: Basic Types & Operations
There is no primitive data type in Scala.

| Value type   | Range                                                        |
| ------------ | ------------------------------------------------------------ |
| `scala.Byte` | 8-bit signed two's complement integer (-2<sup>7</sup> to 2<sup>7</sup> - 1, inclusive) |
|`scala.Short`|16-bit signed two's complement integer (-2<sup>15</sup> to 2<sup>15</sup> - 1, inclusive)|
|`scala.Int`|32-bit signed two's complement integer (-2<sup>31</sup> to 2<sup>31</sup> - 1, inclusive)|
|`scala.Long`|64-bit signed two's complement integer (-2<sup>63</sup> to 2<sup>63</sup> - 1, inclusive)|
|`scala.Char`|16-bit unsigned Unicode character (0 to 2<sup>16</sup> - 1, inclusive)|
|`java.lang.String`|a sequence of Chars|
|`scala.Float`|32-bit IEEE 754 single-precision float|
|`scala.Double`|64-bit IEEE 754 double-precision float|
|`scala.Boolean`|`true` or `false`|

> Bite short char into floating long double (_Byte Short Char Int Float Long Double_)

**Literal**: A way to write constant value directly into code.
**Integral Types**:
- `Byte`, `Short`, `Int`, `Long`, `Char`

### Integral Literals
- Can be represented as **Decimal(10 base)**, **Octal(8 base)** and **Hexadecimal(16 base)**
- Hexadecimal:
  - `^[+-]?0[Xx][0-9a-fA-F]+$`
- Octal:
  - `^[+-]?0[0-7]+$`
- Decimal:
  - `^[+-]?[1-9]\d*$`
- Scala shell always prints integer values in base 10, no matter what literal form you may have used to initialize it
- If number ends with non-zero digit, it is considered `Int`
- If number ends with `L` or `l`, it is considered `Long`

**Numeric Types**:
- `Float`, `Double`

### Floating point Literals
- `[+-]?\d+?(\.\d+)?([eE]\d+)?`
- If number ends with `F` or `f`, it is considered `Float`
- If number ends with `D`, `d` or any digit, it is considered `Double`

### Character Literals
- In addition to providing an explicit character between the single quotes, you can provide an octal or hex number for the character code point preceded by a backslash
- The octal number must be between `'\0'` and `'\377'`
- A character literal can also be given as a general Unicode character consisting of four hex digits and preceded by a `\u`
- **Special character literal escape sequence**:
    | Literal | Meaning                    |
    | ------- | -------------------------- |
    | `\n`    | line feed (`\u000A`)       |
    | `\b`    | backspace (`\u0008`)       |
    | `\t`    | tab (`\u0009`)             |
    | `\f`    | form feed (`\u000C`)       |
    | `\r`    | carriage return (`\u000D`) |
    | `\"`    | double quote (`\u0022`)    |
    | `\'`    | single quote (`\u0027`)    |
    | `\\`    | backslash (`\u005C`)       |

### Symbol Literals
- `^'[a-zA-Z_]\w+$`
- This is also valid: `'~!@#%&*+|\:/?><`. Not sure why. ???
- literals are mapped to instances of the predefined class `scala.Symbol`
- `val a = 'test` will be expanded to `val a = Symbol("test")` by Scala compiler
- symbols are interned. If you write the same symbol literal twice, both expressions will refer to the exact same Symbol object.
  ```scala
  val a = 'this_is_1_test
  //> a  : Symbol = 'this_is_1_test
  val b = 'ok
  //> b  : Symbol = 'ok
  a == b
  //> res0: Boolean = false
  a.toString
  //> res1: String = 'this_is_1_test
  a.name
  //> res2: String = this_is_1_test
  def printName(arg: Symbol): Unit = {
  	println(arg.name)
  } //> printName: (arg: Symbol)Unit
  ```

## Operators and methods
- All operators in Scala are methods. i.e. `1 / 20` is same as `1./(20)`
- Scala also supports prefix and post fix operators
- **Infix operators**:
  - Operators: `+ - / * % ^`
      ```scala
        // User defined
        class A(value: Int) {
        def + (that: A): A = {
      	  new A(this.value + that.value)
        }
        }
        val a = new A(10)
        val b = new A(20)
        a + b // results in A(30)
        a.+(b) // also results in A(30)
      ```
      ```
      - `>>` adds sign bit to the right most position. Hence, it is **signed** shift right.
      - `>>>` adds `0` to the right most position. Hence, it is **unsigned** shift right
      ```

- **Prefix operators**:
  - Operators: `+ - ! ~`
  - Method name syntax: `unary_<Operator>`
      ```scala
      !true == true.unary_!
      -20 == 20.unary_-
      // User defined
      class B(var value: Int) {
        def unary_! {
            println("bang!")
        }
      }
      val a = new B(0)
      !a // bang!
      ```
    - See `Bool.scala`

## Object Equality

```scala
scala> List(1, 2, 3) == List(1, 2, 3)
res34: Boolean = true

scala> List(1, 2, 3) == List(4, 5, 6)
res35: Boolean = false

scala> 1 == 1.0
res36: Boolean = true
    
scala> List(1, 2, 3) == "hello"
res37: Boolean = false

scala> List(1, 2, 3) == null
res38: Boolean = false
    
scala> null == List(1, 2, 3)
res39: Boolean = false
```
### Rule for equality:
```scala
left == right // left.==(right)
```
- is the left side `null`?
  - NO
    - is right side `null`?
      - YES -> `false`
      - NO -> call `left.equals(right)`
  - YES
    - is right side `null`?
      - YES -> `true`
      - NO -> `false`

> Java's `==` compares **reference equality** on reference types, which means the two variables point to the same object on the JVM's heap. Scala provides a facility for comparing reference equality, as well, under the name `eq`. However, `eq` and its opposite, `ne`, only apply to objects that directly map to Java objects.



### Call-by-value & Call-by-name

- default parameters passed in scala are all **Call-by-Value**, meaning, all parameters are evaluated before executing method code.

- **Call-by-Name** parameters are evaluated only if they come in scope of execution in function body.

  ```scala
  def loop: Int = loop

  def test(a: Int, b: Int): Int = if (a == 0) 0 else b

  test(1, loop)
  // this will never terminate, because compiler will first try to reduce function loop to value, which is immpossible because of it being infinite loop.

  def test_new(a: => Int, b: => Int): Int = if (a == 0) 0 else b

  test_new(0, loop)
  /*
  this will return 0. '=>' in parameter indicates that these parameters are Call By Name, meaning they are evaluated only if they are encountered/used inside function body.
  Here, as first parameter is 0, there is no need for compiler to execute else block and infinite loop is avoided.
  */
  ```



### Operator Precedence

- Operator precedence determines which parts of an expression are evaluated before the other parts

- You can use parentheses in expressions to clarify evaluation order or to override precedence

- *Scala decides precedence based on the first character of the methods used in operator notation*

- Operator precedence table

  | Precedence (decreasing order)    |
  | -------------------------------- |
  | `(all other special characters)` |
  | `* / %`                          |
  | `+ -`                            |
  | `:`  (right-associative methods) |
  | `< >`                            |
  | `= !`                            |
  | `&`                              |
  | `^`                              |
  | `|`                              |
  | `(all letters)`                  |

- If an operator **ends** in an equals character (`=`), and the operator is not one of the comparison operators `<=`, `>=`, `==`, or `!=`, then the precedence of the operator is the same as that of simple assignment (`=`)


#### Right Associativity:

- Any method name that ends with colon(`:`) is invoked on right operand, passing in the left operand.

  ```scala
  implicit class IntEnh(arg0: Int) {
      def ~: (arg1: Int): Int = {
          println(s"Called on $arg0")
          println(s"Passed $arg1 as parameter")
          arg0 + arg1
      }
  }

  1 ~: 2 // 2.~:(1)
  /*
  Called on 2
  Passed 1 as parameter
  */

  1 ~: 2 ~: 3 // 3.~:(2).~:(1)
  /*
  Called on 3
  Passed 2 as parameter
  Called on 5
  Passed 1 as parameter
  */
  ```

  ​

### Rich Wrappers

- methods made available via **implicit conversions**

  | Basic type | Rich wrapper                           |
  | ---------- | -------------------------------------- |
  | `Byte`     | `scala.runtime.RichByte`               |
  | `Short`    | `scala.runtime.RichShort`              |
  | `Int`      | `scala.runtime.RichInt`                |
  | `Long`     | `scala.runtime.RichLong`               |
  | `Char`     | `scala.runtime.RichChar`               |
  | `Float`    | `scala.runtime.RichFloat`              |
  | `Double`   | `scala.runtime.RichDouble`             |
  | `Boolean`  | `scala.runtime.RichBoolean`            |
  | `String`   | `scala.collection.immutable.StringOps` |



