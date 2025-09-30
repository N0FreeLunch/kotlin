# any

## 시그니처

```
fun <T> Array<out T>.any(): Boolean
```

```
fun ByteArray.any(): Boolean
```

```
fun ShortArray.any(): Boolean
```

```
fun IntArray.any(): Boolean
```

```
fun LongArray.any(): Boolean
```

```
fun FloatArray.any(): Boolean
```

```
fun DoubleArray.any(): Boolean
```

```
fun BooleanArray.any(): Boolean
```

```
fun CharArray.any(): Boolean
```

## 설명

> Returns true if array has at least one element.
- 배열이 적어도 하나의 요소를 가졌다면 true를 반환한다.

## 예제

```kt
val emptyList = emptyList<Int>()
println("emptyList.any() is ${emptyList.any()}") // false

val nonEmptyList = listOf(1, 2, 3)
println("nonEmptyList.any() is ${nonEmptyList.any()}") // true 

```
- `val emptyList = emptyList<Int>()`: 코틀린에서 List, Set, Map는 같은 읽기 전용 읽기 전용 자료 구조이기 때문에 빈 자료유형을 선언할 때 새로운 객체를 만들지 않고, 미리 준비된 각 읽기 전용 유형의 싱글턴 객체를 사용하여 메모리 소모를 절약할 수 있는 방법을 제공한다.
- `emptyList.any()`: 어떠한 요소도 없기 때문에, any 메소드는 false를 반환한다.
- `val nonEmptyList = listOf(1, 2, 3)`: `listOf`는 해당 요소를 가진 읽기전용 배열인 List 타입의 자료 유형을 만든다.
- `nonEmptyList.any()`: `nonEmptyList`는 요소가 존재하기 때문에 any 메소드는 true를 반환한다.

---

## 시그니처

```
inline fun <T> Array<out T>.any(predicate: (T) -> Boolean): Boolean
```

```
inline fun ByteArray.any(predicate: (Byte) -> Boolean): Boolean
```

```
inline fun ShortArray.any(predicate: (Short) -> Boolean): Boolean
```

```
inline fun IntArray.any(predicate: (Int) -> Boolean): Boolean
```

```
inline fun LongArray.any(predicate: (Long) -> Boolean): Boolean
```

```
inline fun FloatArray.any(predicate: (Float) -> Boolean): Boolean
```

```
inline fun DoubleArray.any(predicate: (Double) -> Boolean): Boolean
```

```
inline fun BooleanArray.any(predicate: (Boolean) -> Boolean): Boolean
```

```
inline fun CharArray.any(predicate: (Char) -> Boolean): Boolean
```

```
inline fun <T> Iterable<T>.any(predicate: (T) -> Boolean): Boolean
```

## 설명

> Returns true if at least one element matches the given predicate.

- 적어도 하나의 요소가 술어함수를 만족하면 true를 반환한다.

## 예제

```kt
val isEven: (Int) -> Boolean = { it % 2 == 0 }
val zeroToTen = 0..10
println("zeroToTen.any { isEven(it) } is ${zeroToTen.any { isEven(it) }}") // true
println("zeroToTen.any(isEven) is ${zeroToTen.any(isEven)}") // true

val odds = zeroToTen.map { it * 2 + 1 }
println("odds.any { isEven(it) } is ${odds.any { isEven(it) }}") // false

val emptyList = emptyList<Int>()
println("emptyList.any { true } is ${emptyList.any { true }}") // false 

```

- `val isEven: (Int) -> Boolean = { it % 2 == 0 }`: 짝수인지 판별하는 술어함수
- `val zeroToTen = 0..10`: [IntRange](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/-int-range/) 타입을 반환한다. 이는 배열과 달리 모든 요소를 메모리에 저장하는 방식이 아니라, 시작점과 끝점만 저장하고 요소를 순회할 때 요소의 값이 생성되는 레이지한 개념에 가까운 표현으로 연속된 대량의 범위의 값의 집합을 생성할 때도 사용할 수 있다.
- `zeroToTen.any { isEven(it) }`: IntRange의 시그니처를 보면, [IntProgression](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/-int-progression/)의 상속을 받는데, `Iterable<Int>` 인터페이스의 구현체이다. 이는 위의 `inline fun <T> Iterable<T>.any(predicate: (T) -> Boolean): Boolean` 시그니처에 해당하는 any 확장함수이다. 이 확장함수는 `(T) -> Boolean` 시그니처의 함수를 받는데, `isEven`는 이 시그니처에 부합하는 함수이므로 `.any { n -> isEven(n) }`와 동일한 람다함수 표기이다.
- `zeroToTen.any(isEven)}`: 람다 표기법이 아닌 그냥 함수로 전달하는 방식의 코드이다.


- `val odds = zeroToTen.map { it * 2 + 1 }`: 마찬가지로 [IntRange](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/-int-range/)의 인터페이스인 `Iterable<Int>`의 [확장 함수인 map](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/map.html)으로 `inline fun <T, R> Iterable<T>.map(transform: (T) -> R): List<R>` 시그니처이다. `odds`는 `map` 함수의 반환 값이므로 `List<R>`의 타입이다.
- `odds.any { isEven(it) }`: ` List<R>`는 타입 인터페이스이므로, 이 인터페이스 사양을 만족하는 `ArrayList<R>`, `LinkedList<R>` 등의 구현체가 런타임에는 반환된다. 여기서는 `ArrayList<Int>`가 반환된 것이고, `inline fun <T> Iterable<T>.any(predicate: (T) -> Boolean): Booleann` 시그니처의 확장 함수가 사용되었다. 제네릭을 사용하는 경우는 보통 이 시그니처가 사용되며, 요소가 원시 타입을 사용하는 경우에는 `**Array.any()` 쪽이 사용된다.

---

## 시그니처

```
fun <T> Iterable<T>.any(): Boolean
```

## 설명

> Returns true if collection has at least one element.

- 컬렉션이 적어도 하나의 요소를 가지면 true를 반환한다.

## 예제

```kt
val emptyList = emptyList<Int>()
println("emptyList.any() is ${emptyList.any()}") // false

val nonEmptyList = listOf(1, 2, 3)
println("nonEmptyList.any() is ${nonEmptyList.any()}") // true

```

---

## 시그니처

```
fun <K, V> Map<out K, V>.any(): Boolean
```

## 설명

> Returns true if map has at least one entry.

## 예제

```kt
val emptyList = emptyList<Int>()
println("emptyList.any() is ${emptyList.any()}") // false

val nonEmptyList = listOf(1, 2, 3)
println("nonEmptyList.any() is ${nonEmptyList.any()}") // true

```

---

## 시그니처

```
inline fun <K, V> Map<out K, V>.any(predicate: (Map.Entry<K, V>) -> Boolean): Boolean
```

## 설명

> Returns true if at least one entry matches the given predicate.

## 예제

```kt
val isEven: (Int) -> Boolean = { it % 2 == 0 }
val zeroToTen = 0..10
println("zeroToTen.any { isEven(it) } is ${zeroToTen.any { isEven(it) }}") // true
println("zeroToTen.any(isEven) is ${zeroToTen.any(isEven)}") // true

val odds = zeroToTen.map { it * 2 + 1 }
println("odds.any { isEven(it) } is ${odds.any { isEven(it) }}") // false

val emptyList = emptyList<Int>()
println("emptyList.any { true } is ${emptyList.any { true }}") // false 

```

---

## 시그니처

```
@ExperimentalUnsignedTypes
inline fun UIntArray.any(): Boolean
```

```
@ExperimentalUnsignedTypes
inline fun ULongArray.any(): Boolean
```

```
@ExperimentalUnsignedTypes
inline fun UByteArray.any(): Boolean
```

```
@ExperimentalUnsignedTypes
inline fun UShortArray.any(): Boolean
```

## 설명

> Returns true if array has at least one element.

## 예제

```kt
val emptyList = emptyList<Int>()
println("emptyList.any() is ${emptyList.any()}") // false

val nonEmptyList = listOf(1, 2, 3)
println("nonEmptyList.any() is ${nonEmptyList.any()}") // true 
```

---

## 시그니처

```
@ExperimentalUnsignedTypes
inline fun UIntArray.any(predicate: (UInt) -> Boolean): Boolean
```

```
@ExperimentalUnsignedTypes
inline fun ULongArray.any(predicate: (ULong) -> Boolean): Boolean
```

```
@ExperimentalUnsignedTypes
inline fun UByteArray.any(predicate: (UByte) -> Boolean): Boolean
```

```
@ExperimentalUnsignedTypes
inline fun UShortArray.any(predicate: (UShort) -> Boolean): Boolean
```

## 설명

> Returns true if at least one element matches the given predicate.

## 예제

```kt
val isEven: (Int) -> Boolean = { it % 2 == 0 }
val zeroToTen = 0..10
println("zeroToTen.any { isEven(it) } is ${zeroToTen.any { isEven(it) }}") // true
println("zeroToTen.any(isEven) is ${zeroToTen.any(isEven)}") // true

val odds = zeroToTen.map { it * 2 + 1 }
println("odds.any { isEven(it) } is ${odds.any { isEven(it) }}") // false

val emptyList = emptyList<Int>()
println("emptyList.any { true } is ${emptyList.any { true }}") // false 

```

## References

- https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/any.html
