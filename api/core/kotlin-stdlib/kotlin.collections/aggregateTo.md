# aggregateTo

## 시그니처

```
inline fun <T, K, R, M : MutableMap<in K, R>> Grouping<T, K>.aggregateTo(
    destination: M, 
    operation: (key: K, accumulator: R?, element: T, first: Boolean) -> R
): M
```

> Groups elements from the Grouping source by key and applies operation to the elements of each group sequentially, passing the previously accumulated value and the current element as arguments, and stores the results in the given destination map.

> The key for each element is provided by the Grouping.keyOf function.

## 설명

## 예제

```kt
val numbers = listOf(3, 4, 5, 6, 7, 8, 9)

val aggregated = numbers.groupingBy { it % 3 }.aggregateTo(mutableMapOf()) { key, accumulator: StringBuilder?, element, first ->
    if (first) // first element
    StringBuilder().append(key).append(":").append(element)
    else
    accumulator!!.append("-").append(element)
}

println(aggregated.values) // [0:3-6-9, 1:4-7, 2:5-8]

// aggregated is a mutable map
aggregated.clear() 

```
- `val numbers = listOf(3, 4, 5, 6, 7, 8, 9)`: 요소가 3, 4, 5, 6, 7, 8, 9으로 나열되어 있는 List 타입의 자료구조를 생성한다.
- `aggregated = numbers.groupingBy { it % 3 }`: `List` 타입은 `Collection` 인터페이스를 구현한 자료구조의 타입이며, `Collection` 인터페이스의 `groupingBy` 함수의 람다 함수가 반환하는 값을 키로 하여 데이터를 묶으며 키에 대응하는 값은 리스트(리스트로 만들어졌으므로)가 된다.
- `aggregateTo(mutableMapOf()) {`: `aggregateTo`의 첫 번째 인자로는 `mutableMapOf()`라는 함수를 받고, 두 번째 인자로는 중괄호 이하의 람다식을 받는데 이 람다식은 trailing lambda syntax를 사용하여, 함수의 마지막 인자를 밖으로 빼어 중괄호 식으로 표현한 것이다. 원래는 `.aggregateTo(mutableMapOf(), { key, accumulator, element, first -> ... })`가 되는 것인데 코틀린의 문법으로 밖으로 뺀 것이다.

## References
- https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/aggregate-to.html
