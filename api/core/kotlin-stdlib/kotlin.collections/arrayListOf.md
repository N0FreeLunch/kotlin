# arrayListOf

## 시그니처

```
inline fun <T> arrayListOf(): ArrayList<T>
```

## 설명

> Returns an empty new ArrayList.
- 빈 새로운 ArrayList를 반환한다.

## 예제

```kt
val list = arrayListOf<Int>()
println("list.isEmpty() is ${list.isEmpty()}") // true

list.addAll(listOf(1, 2, 3))
println(list) // [1, 2, 3] 
```

- `val list = arrayListOf<Int>()`: Int 타입의 요소를 가진 비어 있는 배열을 만든다.
- `list.isEmpty()`: 비어 있는지 확인하기 위한 표기
- `list.addAll(listOf(1, 2, 3))`: 요소가 1,2,3이 들어 있는 List 타입의 자료 구조 생성해서 ArrayList 자료 구조에 추가, 불변이지만, 가변 구조에 데이터를 넣으므로 정상적으로 동작

## 시그니처

```
fun <T> arrayListOf(vararg elements: T): ArrayList<T>
```

## 설명

> Returns a new ArrayList with the given elements.
- 주어진 요소들을 가진 새로운 ArrayList를 반환한다.

```kt
val list = arrayListOf(1, 2, 3)
println(list) // [1, 2, 3]

list += listOf(4, 5)
println(list) // [1, 2, 3, 4, 5] 
```

- `val list = arrayListOf(1, 2, 3)`: 요소가 Int이고 요소로 1,2,3이 든 ArrayList 자료 유형의 객체를 생성한다.
- `list += listOf(4, 5)`: `+=` 연산자를 통해서 요소가 4, 5인 List 타입의 자료 유형을 생성한 후 기존의 `ArrayList`에 추가를 할 수 있다.
- 코틀린은 연산자 오버로딩이 가능한 언어로, 클래스에 대한 연산자의 동작을 정의 할 수 있다. ArrayList에 대한 `+=`는 연산자는 언어 내장으로 정의된 것이다.

## References

- https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/array-list-of.html
