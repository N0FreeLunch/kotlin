# aggregate

## 시그니처

```
inline fun <T, K, R> Grouping<T, K>.aggregate(
    operation: (key: K, accumulator: R?, element: T, first: Boolean) -> R
): Map<K, R>
```

> Groups elements from the Grouping source by key and applies operation to the elements of each group sequentially, passing the previously accumulated value and the current element as arguments, and stores the results in a new map.
- Grouping 소스의 요소들을 키(key)별로 그룹화한 뒤, 이전에 누적된 값과 현재의 요소를 인자로서 전달하여 각 그룹의 요소에 순차적으로 적용한다. 그리고 결과를 새로운 맵으로 저장한다.

> The key for each element is provided by the Grouping.keyOf function.
- 각 요소의 키는 Grouping.keyOf 함수에 의해 제공됩니다.

### Return
> a Map associating the key of each group with the result of aggregation of the group elements.
- 그룹 원소의 집계 결과와 각 그룹의 키를 연관(키에 대응하는 값이 존재하는 형태)된 단일 Map(을 반환)

### Parameters

> `operation`: function is invoked on each element with the following parameters:
- `operation`: 뒤따르는 파라메터 (key, accumulator, element, first)와 각 요소로 호출되는 함수
> `key`: the key of the group this element belongs to;
- `key`: 이 요소가 속해 있는 그룹의 키
> `accumulator`: the current value of the accumulator of the group, can be null if it's the first element encountered in the group;
- `accumulator`: 그룹 단위로 누산되는 현재(operation 함수가 호출될 때 인자로 전달되는 요소에 대한 처리 단계에서의) 값으로 이 값은 그룹의 첫 번째 요소를 처리할 때 null이 될 수 있다.
> `element`: the element from the source being aggregated;
- `element`: 집계가 되고 있는 (그룹)소스의 요소 
> `first`: indicates whether it's the first element encountered in the group.
- `first`: 그룹 내에서 조우한 첫 번째 요소인지 아닌지 (불리언 값으로) 나타냄

## 예제

```kt
val numbers = listOf(3, 4, 5, 6, 7, 8, 9)

val aggregated = numbers.groupingBy { it % 3 }.aggregate { key, accumulator: StringBuilder?, element, first ->
    if (first) // first element
    StringBuilder().append(key).append(":").append(element)
    else
    accumulator!!.append("-").append(element)
}

println(aggregated.values) // [0:3-6-9, 1:4-7, 2:5-8] 

```

## References
- https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/aggregate.html
