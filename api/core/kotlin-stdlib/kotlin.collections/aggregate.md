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
- `val numbers = listOf(3, 4, 5, 6, 7, 8, 9)`: 리스트 자료구조의 값으로 3, 4, 5, 6, 7, 8, 9를 넣은 값을 만든다. [listOf](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/list-of.html)는 리스트 타입의 값을 반환하는 함수이다. [List](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/-list/)는 순서가 있지만 키는 없는 자료구조로 위치값(인덱스)에 의해 순서를 판별하는 자료 구조이다. 배열은 크키를 변경할 수 없지만, 리스트는 크기가 동적으로 변경되는 자료구조이다.
- `numbers.groupingBy { it % 3 }` : `numbers`라는 리스트 타입의 객체의 `groupingBy` 메소드로 콜백 람다 표현식의 `{ it % 3 }`에 따라 리스트의 각각의 순회의 요소값을 3으로 나눈 나머지 값을 기준으로 그룹핑하겠다는 의미를 가진다. `List` 타입은 코틀린의 [Collection](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/-collection/) 인터페이스의 구현체로 `groupingBy` 메소드는 `Collection` 인터페이스의 공통 메소드이다. [groupingBy](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/grouping-by.html) 메소드는 Grouping 형식의 타입을 반환하는데 이는 그룹핑의 기준 값을 키로 하고 각각의 키에 대응하는 값으로 List 값을 만들기 때문이다.
- `.aggregate { ... }`: `groupingBy`의 반환 타입은 [Grouping](https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/-grouping/) 인터페이스의 구현체이다. 코틀린은 `Grouping` 타입에 대해 `aggregate`라는 확장함수(`Grouping` 타입을 인자로 전달하는 함수를 마치 메소드 처럼 .으로 접근할 수 있게 만드는 구문 설탕을 제공하는 확장 함수를 추가하는 방식이다.)
- `{ key, accumulator: StringBuilder?, element, first -> ... }`: 이 방식은 람다 표기법으로 아래와 같이 익명함수를 사용하는 방식도 가능하며, 기명 함수를 정의한 후 전달하는 것도 가능하다.
```kt
val aggregated = numbers.groupingBy { it % 3 }
    .aggregate(fun(key: Int, accumulator: StringBuilder?, element: Int, first: Boolean): StringBuilder {
        return if (first) {
            StringBuilder().append(key).append(":").append(element)
        } else {
            accumulator!!.append("-").append(element)
        }
    })
```

## References
- https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/aggregate.html
