# Note: Scala
Scala is a <strong> pure object-oriented language </strong>. In Scala, <strong> every value is an object </strong>. This is a learning note of Scala.

### Agenda
* [Features](#features)
* [Basic Syntax](#basicsyntax)
* [Function](#function)
* [Trait](#trait)
* [Extrator](#extractor)
* [FileIO](#fileio)

### Features
- Object-oriented: Every Value is an object
- Nexted Functions; Support high-order funcitons
- Domain specific language (DSL) support;
- Consistent with Java:
  - Run on JVM
  - Be able to execute Java code
  - Concurrent& Synchronized processing
- Web Frameworks:
  - Lift
  - Play
  - Bowler

### Basic
#### Syntax
- Object
- Class
- Methods
- Fields
- Closure
- Traits

#### Data Types
| Type | Bits | Range |
| -------- | ------------ | -------------- |
| `Byte` | 8bits | -128 ~ 127 |
| `Short` | 16bits | -2^15 ~ 2^15 |
| `Int` | 32bits | -2^31 ~ 2^31 |
| `Long` | 64bits |-2^63 ~ 2^63 |
| `Float` | 32bits |
| `Double` | 64bits |
| `Char` | 16bits | U+0000 ~ U+FFFF |
| `String` |
| `Boolean` | | True or False |
| `Unit` | | No Value |
| `Null` | Null or empty reference |
| `Nothing` | Subtype of every other type, including no values |
| `Any` | Supertype of any type; any object is `Any` |
| `AnyRef` | Supertype of any reference type |

#### Variable Declaration
| Type | Feature | Scope |
| ----- | ----- | ------ |
| `var` | Mutable variable | `Fields`, `Local Variables` |
| `val` | Immutable variable | `Fields`, `Local Variables`, `Methods` |

#### Access Modifier
Access Modifiers in Scala are similar to ones in Java except for `default`. `default` is `public` in Scala.

| Type | Scope |
| ----- | ----- |
| `private` | Only visible inside the class |
| `protected` | Visible inside the class and subclasses |
| `public` | Could be accessed anywhere |

Scala also has scope of protection. See the following code.
```scala
package society {
   package professional {
      class Executive {
         private[professional] var workDetails = null
         private[society] var friends = null
         private[this] var secrets = null

         def help(another : Executive) {
            println(another.workDetails)
            println(another.secrets) //ERROR
         }
      }
   }
}
```

### Function
A Scala function declaration has the following form. Methods are implicitly declared abstract if no equal sign and method body is used.
```scala
def functionName ([list of parameters]) :[return type]
```

#### Call-by-name
```scala
object Demo {
   def main(args: Array[String]) {
        delayed(time());
   }

   def time() = {
      println("Getting time in nano seconds")
      System.nanoTime
   }
   def delayed( t: => Long ) = {
      println("In delayed method")
      println("Param: " + t)
   }
}
```

#### Redundant Parameters
```scala
object Demo {
   def main(args: Array[String]) {
      printStrings("Hello", "Scala", "Python");
   }
   
   def printStrings( args:String* ) = {
      var i : Int = 0;
      
      for( arg <- args ){
         println("Arg value[" + i + "] = " + arg );
         i = i + 1;
      }
   }
}
```

#### High-order Functions
```scala
object Demo {
   def main(args: Array[String]) {
      println( apply( layout, 10) )
   }

   def apply(f: Int => String, v: Int) = f(v)

   def layout[A](x: A) = "[" + x.toString() + "]"
}
```

#### Anonymous Functions
```scala
var inc = (x:Int) => x + 1
```

#### Partially Applied Function
```scala
import java.util.Date

object Demo {
   def main(args: Array[String]) {
      val date = new Date
      val logWithDateBound = log(date, _ : String)

      logWithDateBound("message1" )
      Thread.sleep(1000)
      
      logWithDateBound("message2" )
      Thread.sleep(1000)
      
      logWithDateBound("message3" )
   }

   def log(date: Date, message: String) = {
      println(date + "----" + message)
   }
}
```

#### Currying Functions
```scala
object Demo {
   def main(args: Array[String]) {
      val str1:String = "Hello, "
      val str2:String = "Scala!"
      
      println( "str1 + str2 = " +  strcat(str1)(str2) )
   }

   def strcat(s1: String)(s2: String) = {
      s1 + s2
   }
}
```

### Trait
A trait encapsulates methods and field definitions, which can then be reused by mising them into classes. Unlike class ineritance, in which each class must inherit from just one superclass, a class can mix in any number of traits.
```scala
trait Equal {
    def isEqual(x: Any): Boolean
    def isNotEqual(x: Any): Boolean = !isEqual(x)
}

class Point(xc: Int, yc: Int) extends Equal {
   var x: Int = xc
   var y: Int = yc
   
   def isEqual(obj: Any) = obj.isInstanceOf[Point] && obj.asInstanceOf[Point].x == y
}

object Demo {
   def main(args: Array[String]) {
      val p1 = new Point(2, 3)
      val p2 = new Point(2, 4)
      val p3 = new Point(3, 3)

      println(p1.isNotEqual(p2))
      println(p1.isNotEqual(p3))
      println(p1.isNotEqual(2))
   }
}
```

### Extrator
An extractor in Scala is an object that has a method called unapply as one of its members. See the following code for a better understanding.
```scala
object Demo {
   def main(args: Array[String]) {
      val x = Test(5)
      println(x)

      x match {
         case Test(num) => println(x+" is bigger two times than "+num)
         
         //unapply is invoked
         case _ => println("i cannot calculate")
      }
   }
   def apply(x: Int) = x*2
   def unapply(z: Int): Option[Int] = if (z%2==0) Some(z/2) else None
}
```

### FileIO
#### Read File
```scala
import scala.io.Source

object Demo {
   def main(args: Array[String]) {
      println("Following is the content read:" )

      Source.fromFile("Demo.txt" ).foreach { 
         print 
      }
   }
}
```

#### Read Command Line
```scala
object Demo {
   def main(args: Array[String]) {
      print("Please enter your input : " )
      val line = Console.readLine
      
      println("Thanks, you just typed: " + line)
   }
}
```

#### Write File
```scala
import java.io._

object Demo {
   def main(args: Array[String]) {
      val writer = new PrintWriter(new File("test.txt" ))

      writer.write("Hello Scala")
      writer.close()
   }
```
