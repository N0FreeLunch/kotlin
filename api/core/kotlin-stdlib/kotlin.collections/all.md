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
