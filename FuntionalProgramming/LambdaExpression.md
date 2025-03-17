# 람다 표현식

람다 표현식을 통해서 함수를 하나의 값으로 취급할 수 있다. 함수가 값으로 취급되면 함수의 파라메터로 전달될 수 있다.

```kotlin
fun sum(a: Int, b: Int) = 
    fold(a, b, 0, { acc, i -> acc + i })

fun product(a: Int, b: Int) = 
    fold(a, b, 1, { acc, i -> acc * i })

fun main() {
    val sumResult = sum(1,5)
    println(sumResult)
    val productResult = product(1,5)
    println(productResult)
}

fun fold(
    a: Int,
    b: Int,
    initial: Int,
    operation: (Int, Int) -> Int
): Int {
    var acc = initial
    for (i in a..b) {
        acc = operation(acc, i)
    }
    return acc
}
```

위 예제에서 람다 표현 `{ acc, i -> acc + i }`, `{ acc, i -> acc * i }`은 fold 함수의 4번째 파라메터 `operation: (Int, Int) -> Int`에 전달되었다.

화살표를 사용할 때 `=>`가 아니라 `->`임에 주의하자. 자바스크립트의 `() => {}` 표현과 다르므로 헷갈리지 않도록 한다.

## fold 함수 이해하기

```kotlin
fun fold(
    a: Int,
    b: Int,
    initial: Int,
    operation: (Int, Int) -> Int
): Int {
    var acc = initial
    for (i in a..b) {
        acc = operation(acc, i)
    }
    return acc
}
```

fold 함수는 범위를 지정하고 해당 범위 안의 순차적으로 나열되는 모든 값에 대해 reduce 연산을 한다.

코틀린의 표준라이브러리는 범위 문법 `(a..b)`에 대해 `fold` 메소드를 내장하여 제공한다. 위의 `fold` 함수와 달리 대상 문법 `(a..b)`에 이미 범위가 존재하므로 범위를 지정하기 위해 받는 두 파라메터 a, b는 필요없으므로 초기값과 순회 함수를 받기만 하면 된다.

## 표준라이브러리 사용하기

표준라이브러리는 언어에 내장된 것으로 특별히 import로 추가하지 않아도 사용가능하다. 표준 라이브러리에 내장된 fold 함수는 수의 범위를 나타내는 `(a..b)`의 메소드로 정의되어 있다.

```kotlin
fun sum(a: Int, b: Int) = (a..b).fold(0) { acc, i -> acc + i }
fun product(a: Int, b: Int) = (a..b).fold(1) { acc, i -> acc + i}

fun main() {
    val sumResult = sum(1,5)
    println(sumResult)
    val productResult = product(1,5)
    println(productResult)
}
```

위의 예제를 보면 람다 표현식이 fold 함수의 인자로 들어가 있지 않다. 코틀린에서 함수의 파라메터 마지막이 함수를 받을 때, 마지막 파라메터를 받지 않은 상태에서 닫은 후에 람다 함수를 받을 수 있는데 위와 같은 문법을 후행 람다(`trailing lambda`) 문법을 제공한다. `a..b).fold(0)`에서 fold는 2개의 인자를 받는데, 마지막 인자를 받는 파라메터가 함수를 받는 파라메터이므로 후행 람다로 사용될 수 있다. 따라서 인자를 하나만 받고 (`fold(0`) 함수를 닫은 후 (`fold(0)`) 후행 람다를 `{ acc, i -> acc + i }`를 받도록 된다.

## 함수 참조 사용하기

```kotlin
fun sum(a: Int, b: Int) = (a..b).fold(0, Int::plus)
fun product(a: Int, b: Int) = (a..b).fold(1, Int::times)

fun main() {
    val sumResult = sum(1,5)
    println(sumResult)
    val productResult = product(1,5)
    println(productResult)
}
```

코틀린에서 람다 표현식 `{ acc, i -> acc + i }`와 같은 코드는 값으로 다뤄진다. 그에 반해 일반 함수 `Int::plus`는 이미 생성된 함수를 참조한다. 코틀린은 자바가 메인인 JVM에서 구동되므로 (익명 클래스의 인스턴스나 Java 8부터 도입된 함수형 인터페이스로 변환) 람다 표현식과 같은 값으로 다뤄지는 함수가 아니라면 모두 컴파일 후에 객체로 변환되는 함수가 된다. 따라서 미리 내장된 함수는 JVM에서 객체로 구현되어 있으므로 `Int::plus`는 함수로 사용되는 객체를 참조한다.

## 컬렉션 사용하기

```kotlin
fun sum(a: Int, b: Int) = (a..b).sum()
fun product(a: Int, b: Int) = (a..b).fold(1, Int::times)

fun main() {
    val sumResult = sum(1,5)
    println(sumResult)
    val productResult = product(1,5)
    println(productResult)
}
```

범위 내의 모든 수의 합을 구하는 것은 이미 `(a..b)` 구문의 컬렉션 메소드 sum으로 제공하기 때문에 sum으로 표현을 좀 더 단순화 할 수 있다.
