title: The Evolution of Testing 
output: basic.html
controls: true

--

# testing!
## let's talk about it

--

### math trick

For any multiple of three, the sum of its digits is also a multiple of three.

--
### math trick

For any multiple of three, the sum of its digits is also a multiple of three.

####	12: 1+2 = 3

####	810: 8+1+0 = 9

--
### let's try it

```scala
package math
import scala.annotation.tailrec

object FunMath {
  
  @tailrec
  def divisibleByThree(x: Int): Boolean = {
    val sum = sumOfDigits(x)
    if (sum < 10) {
      sum match {
        case 0 | 3 | 6 | 9 => true
        case _ => false
      }
    } else divisibleByThree(sum)
  }
  
  private def sumOfDigits(x: Int): Int = 
    Math.abs(x).toString.toList.map(c => c.asDigit).foldLeft(0)(_ + _)
}
```

--
### let's test it

```scala
package example

import org.junit.runner.RunWith
import org.scalatest.junit.JUnitRunner
import org.scalatest.FunSuite

import math.FunMath
import math.FunMath._

class FunMathSuite extends FunSuite {
  // basic numbers
  test("1 should fail") {
    assert(FunMath.divisibleByThree(1) == false)
  }
  test("2 should fail") {
    assert(FunMath.divisibleByThree(2) == false)
  }
  test("3 should pass") {
    assert(FunMath.divisibleByThree(3) == true)
  }
}
```

-- 
### let's catch some corner cases

```scala
  // tiny numbers
  test("-17077161 should pass") {
    assert(FunMath.divisibleByThree(-17077161) == true)
  }
  test("-2147483648 should fail") {
    assert(FunMath.divisibleByThree(Int.MinValue) == false)
  }
  
  // huge numbers
  test("6442449 should pass") {
    assert(FunMath.divisibleByThree(6442449) == true)
  }
  test("2147483647 should fail") {
    assert(FunMath.divisibleByThree(Int.MaxValue) == false)
  }  
  
  // weird numbers
  test("0 should pass") {
    assert(FunMath.divisibleByThree(0) == true)    
  }
```

--
### let's encapsulate our data

```scala
package example

import org.junit.runner.RunWith
import org.scalatest.junit.JUnitRunner
import org.scalatest.FunSuite

import math.FunMath
import math.FunMath._

class FunMathSuite3 extends FunSuite {
  val multiplesOfThree = List(-17077161, 0, 3, 6442449)
  val nonMultiplesOfThree = List(Int.MinValue, 1, 2, Int.MaxValue)
  
  // passing numbers
  test("multiples of three should pass") {
    for (num <- multiplesOfThree) {
      assert(FunMath.divisibleByThree(num) == true, num + " did not pass")   
    }
  }
  
  // failing numbers
  test("non multiples of three should fail") {
    for (num <- nonMultiplesOfThree) {
      assert(FunMath.divisibleByThree(num) == false, num + " did not fail")   
    }
  }
}
```

--
### let's add more data points
```scala
package example

import org.junit.runner.RunWith
import org.scalatest.junit.JUnitRunner
import org.scalatest.FunSuite

import math.FunMath
import math.FunMath._

class FunMathSuite4 extends FunSuite {
  val multiplesOfThree = -17077161 :: 6442449 :: (-102 to 100 by 3).toList  
  val nonMultiplesOfThree = Int.MinValue :: Int.MaxValue :: (-101 to 100 by 3).toList ::: (-100 to 100 by 3).toList
  
  // passing numbers
  test("multiples of three should pass") {
    for (num <- multiplesOfThree) {
      assert(FunMath.divisibleByThree(num) == true, num + " did not pass")   
    }
  }
  
  // failing numbers
  test("non multiples of three should fail") {
    for (num <- nonMultiplesOfThree) {
      assert(FunMath.divisibleByThree(num) == false, num + " did not fail")   
    }
  }
}
```

-- 
### too much work

![Lazy Dog](/assets/dog.JPG)

--
### scalacheck!

define behavior, get data

--
### let's let scalacheck come up with data for us

```scala
package example

import org.scalatest.prop.Checkers
import org.scalacheck.Prop.forAll
import org.junit.runner.RunWith
import org.scalatest.junit.JUnitRunner
import org.scalatest.FunSuite

import math.FunMath
import math.FunMath._

class FunMathSuite5 extends FunSuite with Checkers {
  val propIsMultipleOfThree = forAll{
  	(n: Int) => FunMath.divisibleByThree(n) == ((n % 3) == 0)
  }
  
  test("results agree with mod 3") {
    check(propIsMultipleOfThree)
  }
}
```

--
### look out for

```scala
assert(myBuggyFunction1 == myBuggyFunction2)
```

--
### thanks!
