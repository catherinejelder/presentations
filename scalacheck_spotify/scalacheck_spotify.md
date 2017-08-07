title: Property Based Testing for Scio Pipelines 
output: basic.html
controls: true
theme: jdan/cleaver-retro

--

### property based testing for scio pipelines

what alternatives to unit testing are there?

--

### let's talk about testing!

##### what should you get out of this talk?

one or two new ways to think about testing scio pipelines

##### what do I want out of this talk?

feedback

--

### agenda

intro to property based testing (scalacheck)

simple use case: multiples of three

spotify use case: abba quality pipeline

pros / cons / thoughts

--

### unit testing vs property based testing
#### what are we testing?

method

property (usually of a method)

#### how are we testing it?

input: data, output: data

input: generator (of data), output: property

--

### definitions
#### generator

produces data of a specified type

#### property

specifies characteristics of a dataset

--

### simple use case

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
    Math.abs(x).toString.toList.map(_.asDigit).sum
}
```
--

### let's test it

```scala
package example

import org.scalatest.FunSuite
import math.FunMath

class FunMathSuite4 extends FunSuite {
  val multiplesOfThree = -17077161 :: 6442449 :: (-102 to 100 by 3).toList  
  val nonMultiplesOfThree = (Int.MinValue :: Int.MaxValue :: (-101 to 100 by 3).toList 
    ::: (-100 to 100 by 3).toList)
  
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
### scalacheck!

I'm tired of coming up with data points.

I want to specify behavior and get data for free.

--
### let's let scalacheck come up with data for us

```scala
package example

import org.scalatest.prop.Checkers
import org.scalacheck.Prop.forAll
import org.scalatest.FunSuite
import math.FunMath

class FunMathSuite5 extends FunSuite with Checkers {
  val propIsMultipleOfThree = forAll{(n: Int) => FunMath.divisibleByThree(n) == ((n % 3) == 0)}

  test("results agree with mod 3") {
    check(propIsMultipleOfThree)
  }
}
```
--
### spotify use case

abba quality pipeline

--
### scalacheck pros

easily generate lots of data to test with

--
### scalacheck cons

tests get harder to reason about 

(my\_function\_1 == my\_function\_1)
or 
(my\_buggy\_function\_1 == my\_buggy\_function\_2)

non deterministic

-- 
### scalacheck at spotify

flatmap provides generators for "spotify data"

https://ghe.spotify.net/scala/scalacheck-extra

--
### thanks!
