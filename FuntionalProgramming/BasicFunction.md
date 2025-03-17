# 코틀린의 함수

```kotlin
fun sum(a: Int, b: Int): Int {
    var sum = 0;
    for (i in a..b) {
        sum += i
    }
    return sum
}

fun main() {
    val sumResult = sum(5, 10)
    println(sumResult)
}
```

- sum 함수는 첫 번째 인자로 받은 값부터 두 번째 인자로 받은 값까지 모두 더한 결과를 반환한다. (add 함수가 아님에 주의)

```kotlin
fun sum(a: Int, b: Int): Int {
    var sum = 0;
    for (i in a..b) {
        sum += i
    }
    return sum
}

fun product(a: Int, b: Int): Int {
    var product = 1
    for(i in a..b) {
        product *= i
    }
    return product;
}

fun main() {
    val sumResult = sum(5, 10)
    println(sumResult)
    val productResult = product(5, 10)
    println(productResult)
}
```

## 반환 타입의 중요성

반환 타입을 적으면 함수에서 잘못된 타입의 결과가 리턴이 되는 경우 타입이 잘못 된다는 것을 알려 준다.

```kotlin
  fun sum(a: Int, b: Int) {
      var sum = 0;
      for (i in a..b) {
          sum += i
      }
  }
  
  fun main() {
      val sumResult = sum(5, 10)
      println(sumResult) // kotlin.Unit
  }
```

위의 예에서 sum 함수의 반환 타입을 적지 않았다면 return 값을 적는 것을 적지 않았을 때 타입 불일치로 지적하지 않는다. 함수의 결과값은 아무것도 반환하지 않기 때문에 결과값이 kotlin.Unit으로 나온다. 반환 타입을 적어 주었다면 IDE에 의해 return이 빠진 것을 쉽게 알 수 있을 것이다.

## 코틀린 VS 자바

자바로 함수형 프로그래밍을 하려면 굉장한 보일러 플레이트를 요구하는 것에 반해 코틀린에서 함수를 작성하는 것은 아주 간단하다. 물론 자바도 버전업을 하면서 함수형 프로그래밍을 위한 여러 문법들을 도입했지만 코틀린은 자바에 비해 간단한 방식으로 함수를 다룰 수 있도록 한다.
