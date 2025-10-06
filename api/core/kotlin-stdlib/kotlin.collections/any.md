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

```

- `val isEven: (Int) -> Boolean = { it % 2 == 0 }`: 짝수인지 판별하는 술어함수
- `val zeroToTen = 0..10`: [IntRange](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/-int-range/) 타입을 반환한다. 이는 배열과 달리 모든 요소를 메모리에 저장하는 방식이 아니라, 시작점과 끝점만 저장하고 요소를 순회할 때 요소의 값이 생성되는 레이지한 개념에 가까운 표현으로 연속된 대량의 범위의 값의 집합을 생성할 때도 사용할 수 있다.
- `zeroToTen.any { isEven(it) }`: IntRange의 시그니처를 보면, [IntProgression](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/-int-progression/)의 상속을 받는데, `Iterable<Int>` 인터페이스의 구현체이다. 이는 위의 `inline fun <T> Iterable<T>.any(predicate: (T) -> Boolean): Boolean` 시그니처에 해당하는 any 확장함수이다. 이 확장함수는 `(T) -> Boolean` 시그니처의 함수를 받는데, `isEven`는 이 시그니처에 부합하는 함수이므로 `.any { n -> isEven(n) }`와 동일한 람다함수 표기이다.
- `zeroToTen.any(isEven)}`: 람다 표기법이 아닌 그냥 함수로 전달하는 방식의 코드이다.


```kt
val isEven: (Int) -> Boolean = { it % 2 == 0 }
val zeroToTen = 0..10

val odds = zeroToTen.map { it * 2 + 1 }
println("odds.any { isEven(it) } is ${odds.any { isEven(it) }}") // false

```

- `val odds = zeroToTen.map { it * 2 + 1 }`: 마찬가지로 [IntRange](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.ranges/-int-range/)의 인터페이스인 `Iterable<Int>`의 [확장 함수인 map](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/map.html)으로 `inline fun <T, R> Iterable<T>.map(transform: (T) -> R): List<R>` 시그니처이다. `odds`는 `map` 함수의 반환 값이므로 `List<R>`의 타입이다.
- `odds.any { isEven(it) }`: ` List<R>`는 타입 인터페이스이므로, 이 인터페이스 사양을 만족하는 `ArrayList<R>`, `LinkedList<R>` 등의 구현체가 런타임에는 반환된다. 여기서는 `ArrayList<Int>`가 반환된다. ` List<R>`는 `Iterable` 인터페이스의 상속을 받는데, any 메소드는 `inline fun <T> Iterable<T>.any(predicate: (T) -> Boolean): Booleann` 시그니처의 확장 함수가 사용되었다. 제네릭을 사용하는 경우는 보통 이 시그니처가 사용되며, 요소가 원시 타입을 사용하는 경우에는 `**Array.any()` 쪽이 사용된다. 실제 런타임에 생성되는 타입에 대해서는 성능 최적화를 고려할 정도가 아니라면 생각하지 않아도 된다. `ArrayList<R>`, `LinkedList<R>`등의 구현체는 코틀린의 내부 구현에 맡기고, 인터페이스에 부합하는 코드를 작성하면 된다.

```kt
val isEven: (Int) -> Boolean = { it % 2 == 0 }
val zeroToTen = 0..10

val emptyList = emptyList<Int>()
println("emptyList.any { true } is ${emptyList.any { true }}") // false

```
- `val emptyList = emptyList<Int>()`: Int 타입의 빈 List 컬렉션을 만든다.
- `emptyList.any { true }`: `emptyList.any { it -> true }`와 동일한 표현으로 `it`이 생략된 표현 방식이다. 여기서 `any`에 적용되는 시그니처는 `inline fun <T> Iterable<T>.any(predicate: (T) -> Boolean): Boolean`에 해당한다.

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
```

- `val emptyList = emptyList<Int>()`: Int 타입의 빈 List 컬렉션을 만든다.
- `emptyList.any()`: any에 어떠한 인자도 전해지지 않은 `fun <T> Iterable<T>.any(): Boolean` 시그니처에 해당한다.

```kt
val nonEmptyList = listOf(1, 2, 3)
println("nonEmptyList.any() is ${nonEmptyList.any()}") // true
```
- `val nonEmptyList = listOf(1, 2, 3)`: Int 타입의 비어있지 않은 List 컬렉션을 만든다.
- `nonEmptyList.any()`: 인자로 아무것도 전달되지 않을 때는 요소가 유무로 참 거짓을 반환하므로 요소가 있으므로 `true`를 반환한다.

---

## 시그니처

```
fun <K, V> Map<out K, V>.any(): Boolean
```

## 설명

> Returns true if map has at least one entry.

## 예제

```kt
val emptyMap = emptyMap<String, Int>()
val nonEmptyMap = mapOf("a" to 1, "b" to 2)

println("emptyMap.any() = ${emptyMap.any()}")       // false
println("nonEmptyMap.any() = ${nonEmptyMap.any()}") // true
```
- `val emptyMap = emptyMap<String, Int>()`: 요소의 키가 String, 요소의 값이 Int 타입인 Map 자료 유형의 빈 컬렉션을 만든다.
- `val nonEmptyMap = mapOf("a" to 1, "b" to 2)`: Map 자료 유형의 각 요소로 전달된 값이, String, Int으로 된 데이터가 세팅된 요소의 키가 String, 요소의 값이 Int 타입인 Map 자료 유형이 생성된다.
- `emptyMap.any()`: 요소가 없는 빈 맵이므로 false
- `nonEmptyMap.any()`: 요소가 존재하는 맵이므로 true

---

## 시그니처

```
inline fun <K, V> Map<out K, V>.any(predicate: (Map.Entry<K, V>) -> Boolean): Boolean
```

## 설명

> Returns true if at least one entry matches the given predicate.

## 예제

```kt
val ages = mapOf(
    "Alice" to 25,
    "Bob" to 32,
    "Charlie" to 17,
    "Diana" to 29
)

// Check if there is anyone under 18
val hasMinor = ages.any { it.value < 18 }
println("Has a minor: $hasMinor") // true (Charlie)

// Check if all names start with 'A' — this uses any to show the opposite case
val hasNameNotStartingWithA = ages.any { !it.key.startsWith("A") }
println("Has name not starting with A: $hasNameNotStartingWithA") // true

// Check if anyone is exactly 30 years old
val hasThirty = ages.any { entry -> entry.value == 30 }
println("Has someone aged 30: $hasThirty") // false

```

- `val ages = mapOf("Alice" to 25, "Bob" to 32, "Charlie" to 17, "Diana" to 29)`: 키가 String, 값이 정수인 Map 자료 유형의 값을 생성한다.
- `val hasMinor = ages.any { it.value < 18 }`: 나이가 기준 미만인 사람이 있는지의 결과를 반환하는 것으로, 람다함수는 각 요소를 받아 해당 요소의 value가 18 미만인 대상이 존재하는지 확인한다.
- `val hasNameNotStartingWithA = ages.any { !it.key.startsWith("A") }`: 이름이 A로 시작하지 않는 요소가 있는지 확인하는 결과를 반환하는 것으로, 이 자료 구조에서는 키가 사람의 이름이므로 `it.key`로 접근하고, `key`가 문자열이므로 `startsWith` 확장함수를 사용할 수 있으며, 문자열이 A로 시작하지 않는지 확인한다.
- `val hasThirty = ages.any { entry -> entry.value == 30 }`: 30살인 요소가 있는지 확인하는 것으로 파라메터를 포함한 람다함수를 전달 받아 생략형인 `it`을 사용하지 않는 방식으로 활용되었다. Map 요소의 값을 접근하므로 `entry.value`으로 확인한다.

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

`@ExperimentalUnsignedTypes` 에노테이션은 kotlin의 정식 API에는 추후 내부 동작의 변경 가능성이 있는 실험적인 도입인 것을 알려주기 위한 용도로 사용되었다.

컴파일 할 때, 이 에노테이션이 있는 부분을 알려줄 수 있기 때문에, 버전업을 진행할 때, 문제가 될 수 있는 대상을 골라서 찾아내기 위한 용도 및 이 기능을 사용할 때는 추후 변할 수 있다는 것을 다른 개발자에게 알리고 주의를 주기 위한 용도의 에노테이션이라고 이해하면 된다.

## 설명

> Returns true if array has at least one element.

## 예제

```kt
val uints: UIntArray = uintArrayOf(1u, 2u, 3u)
val emptyUInts: UIntArray = uintArrayOf()

println("uints.any() = ${uints.any()}")           // true (not empty)
println("emptyUInts.any() = ${emptyUInts.any()}") // false (empty)

val ulongs: ULongArray = ulongArrayOf(10uL, 20uL)
val ubytes: UByteArray = ubyteArrayOf(0u, 255u)
val ushorts: UShortArray = ushortArrayOf(100u, 200u)

println("ulongs.any() = ${ulongs.any()}")   // true
println("ubytes.any() = ${ubytes.any()}")   // true
println("ushorts.any() = ${ushorts.any()}") // true
```

- 

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

## References

- https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/any.html
