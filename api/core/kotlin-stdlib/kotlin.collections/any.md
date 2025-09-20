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

---

## 시그니처

```
inline fun <T> Array<out T>.any(predicate: (T) -> Boolean): Boolean(source)
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

---

## 시그니처

```
fun <T> Iterable<T>.any(): Boolean
```

## 설명

> Returns true if collection has at least one element.

- 컬렉션이 적어도 하나의 요소를 가지면 true를 반환한다.

## 

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

## References

- https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/any.html
