# all

## 시그니처

```
inline fun <T> Array<out T>.all(predicate: (T) -> Boolean): Boolean
```

```
inline fun ByteArray.all(predicate: (Byte) -> Boolean): Boolean
```

```
inline fun ShortArray.all(predicate: (Short) -> Boolean): Boolean
```

```
inline fun IntArray.all(predicate: (Int) -> Boolean): Boolean
```

```
inline fun LongArray.all(predicate: (Long) -> Boolean): Boolean
```

```
inline fun FloatArray.all(predicate: (Float) -> Boolean): Boolean
```

```
inline fun DoubleArray.all(predicate: (Double) -> Boolean): Boolean
```

```
inline fun BooleanArray.all(predicate: (Boolean) -> Boolean): Boolean
```

```
inline fun CharArray.all(predicate: (Char) -> Boolean): Boolean
```

> Returns true if all elements match the given predicate.
- 모든 요소가 주어진 술어(함수)를 만족하면 true를 반환한다.

> Note that if the array contains no elements, the function returns true because there are no elements in it that do not match the predicate. See a more detailed explanation of this logic concept in "Vacuous truth" article.
- 만약 배열이 원소를 포함하고 있지 않다면, 함수는 true를 반환한다. 이는 술어(함수)를 만족하지 않는 원소가 없기 때문이다. 이 논리 컨셉에 대한 좀 더 자세한 설명은 ["Vacuous truth"](https://en.wikipedia.org/wiki/Vacuous_truth)라는 글을 참고하라.

## 설명

`Array<T>`의 방식이 아니라, 각각의 원시 타입을 요소로 하는 고유한 배열 타입이 존재한다. 코틀린에서 배열이란 자료구조의 요소는 각각 고정된 사이즈를 가지고 있고, 배열의 크기(길이)도 고정되어 있다. 동적 언어의 `Array<T>`와 같이 타입을 지정하기 위해서는 배열의 원소가 교체 가능한 참조 방식 또는 가변 사이즈를 지원하는 방식으로 만들어져야 하는데, 코틀린의 배열은 고정된 사이즈를 가지고 있기 때문에 동일한 자료구조에 여러 타입을 지정하는 것은 불가능하다. 자바의 제네릭이 오브젝트에 대해서 성립하는 이유도 참조 주소라는 고정된 사이즈의 길이를 갖기 때문에 가능한 것이다.

위의 ***Array의 경우 원시 타입의 값을 원소로 하는 배열이다. 원시 타입의 값의 최대 사이즈는 고정되어 있고, 각각 타입에 따라서 사이즈가 다를 수 있기 때문에, 요소의 원시 타입의 종류에 따라 배열의 종류도 여럿 나뉘게 된다.

## 예제

#### `Array<out T>.all`

```kt
val words = arrayOf("Kotlin", "Java", "Scala")
val allStartWithCapital = words.all { it[0].isUpperCase() }
println(allStartWithCapital) // true
```
- `val words = arrayOf("Kotlin", "Java", "Scala")`: [`arrayOf`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin/array-of.html)는 제네릭을 사용하여 제네릭으로 지정한 타입의 요소를 갖는 배열을 생성한다. 제네릭을 지정하지 않더라도 코틀린의 컴파일러는 요소의 타입을 통해서 제네릭에서 사용할 타입을 추론하여 제네릭을 자동으로 적용한다. 원시 타입을 갖는 배열 자료 구조인 `IntArray`, `DoubleArray`, `BooleanArray` 등과 달리 `arrayOf`는 원시 타입을 요소로 갖지 않고, 자바에서 제네릭이 오브젝트에 대해 성립하는 것과 같이 코틀린에서도 제네릭은 오브젝트에 대해 성립한다. `arrayOf`는 제네릭을 사용하므로 요소로 오브젝트를 가지며, 코틀린의 문자열은 자바와의 호환성을 위해 오브젝트이므로 배열의 요소를 문자열로 하기 위해서는 `arrayOf`를 사용한다.
- `val allStartWithCapital = words.all { it[0].isUpperCase() }`: `words`가 문자열로 이뤄진 배열이므로 람다식의 `it`은 문자열 요소를 전달 받는다. 문자열의 첫 번째 글자 `it[0]`를 대문자인지 판단하는 술어 함수인 람다식이고 리턴 키워드가 없고 마지막 식이 반환되기 때문에 `it[0].isUpperCase()` 그 자체로 반환이 된다.

#### `ByteArray.all`

```kt
val bytes = byteArrayOf(1, 2, 3)
val allPositive = bytes.all { it > 0 }
println(allPositive) // true

```

#### `ShortArray.all`

```kt
val shorts = shortArrayOf(100, 200, 300)
val allLessThanThousand = shorts.all { it < 1000 }
println(allLessThanThousand) // true

```

#### `IntArray.all`

```kt
val numbers = intArrayOf(2, 4, 6, 8)
val allEven = numbers.all { it % 2 == 0 }
println(allEven) // true

```

#### `LongArray.all`

```kt
val bigNumbers = longArrayOf(1L, 2L, 3L)
val allPositive = bigNumbers.all { it > 0 }
println(allPositive) // true

```

#### `FloatArray.all`

```kt
val floats = floatArrayOf(1.2f, 3.4f, 5.6f)
val allGreaterThanZero = floats.all { it > 0f }
println(allGreaterThanZero) // true

```

#### `DoubleArray.all`

```kt
val doubles = doubleArrayOf(0.1, 0.2, 0.3)
val allLessThanOne = doubles.all { it < 1.0 }
println(allLessThanOne) // true

```

#### `BooleanArray.all`

```kt
val bools = booleanArrayOf(true, true, true)
val allTrue = bools.all { it }
println(allTrue) // true

```

#### `CharArray.all`

```kt
val chars = charArrayOf('a', 'b', 'c')
val allLowerCase = chars.all { it.isLowerCase() }
println(allLowerCase) // true

```

---

## 시그니처

```
inline fun <T> Iterable<T>.all(predicate: (T) -> Boolean): Boolean(source)
```

### 설명

for문 등의 문법에 의해 순회 할 수 있고, 다양한 컬렉션 메소드를 갖는 인터페이스인 [`Iterable`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/-iterable/)의 all 메소드이다.

## 예제

```kt
val isEven: (Int) -> Boolean = { it % 2 == 0 }
val zeroToTen = 0..10
println("zeroToTen.all { isEven(it) } is ${zeroToTen.all { isEven(it) }}") // false
println("zeroToTen.all(isEven) is ${zeroToTen.all(isEven)}") // false

val evens = zeroToTen.map { it * 2 }
println("evens.all { isEven(it) } is ${evens.all { isEven(it) }}") // true

val emptyList = emptyList<Int>()
println("emptyList.all { false } is ${emptyList.all { false }}") // true 
```
- `val isEven: (Int) -> Boolean = { it % 2 == 0 }`: 짝수인지 판별하는 술어함수이다.
- `val zeroToTen = 0..10`: [`IntRange`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/-int-range/) 타입을 반환한다. 이는 배열과 달리 모든 요소를 메모리에 저장하는 방식이 아니라, 시작점과 끝점만 저장하고 요소를 순회할 때 요소의 값이 생성되는 레이지한 개념에 가까운 표현으로 연속된 대량의 범위의 값의 집합을 생성할 때도 사용할 수 있다.
- [`IntRange`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/-int-range/)의 시그니처를 보면, `IntProgression`의 상속을 받는데, `IntProgression`는 [`Iterable`](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/-iterable/) 인터페이스의 구현체로 인터페이스에는 함수의 시그니처 이외의 코드의 동작을 정의할 수 없기 때문에 Iterable 인터페이스는 all 메소드를 확장함수로 사용한다.
- `zeroToTen.all { isEven(it) }}`: `zeroToTen`는 1~10까지의 수를 가진다. 2,4,6,8,10은 짝수인지 판별하는 술어함수를 만족하지만, 1,3,5,7,9는 만족하지 않는다. `all` 확장 함수는 모든 요소들이 술어함수를 만족해야 하지만 그렇지 않으므로 최종적으로 false를 반환한다.
- `zeroToTen.all(isEven)`: `zeroToTen.all { isEven(it) }}`가 람다 함수 표기인 반면, `all(isEven)`은 인자로 받는 함수의 시그니처가 `all` 파라메터로 받는 함수의 시그니처 `Int -> Boolean`와 동일한 경우 함수 참조 방식으로 함수 파라메터의 인자 전달을 생략할 수 있다.
- `val evens = zeroToTen.map { it * 2 }`: 모든 요소에 2를 곱하여 짝수로 만든다.
- `evens.all { isEven(it) }`: 모든 요소가 짝수이므로 `all` 확장 함수는 true를 반환한다.
- `val emptyList = emptyList<Int>()`: 요소의 타입이 `Int`으로 제한되어 있지만, 요소가 없어 빈 요소가 되었다.
- `emptyList.all { false }`: 빈 요소에 대한 `all` 메소드는 Vacuous truth의 개념이 적용되어 true가 된다.

## References
- https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/all.html
