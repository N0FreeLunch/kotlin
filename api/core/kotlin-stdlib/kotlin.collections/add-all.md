# add-all

## Iterable 시그니처

```
fun <T> MutableCollection<in T>.addAll(elements: Iterable<T>): Boolean
```

> Adds all elements of the given elements collection to this MutableCollection.
- 주어진 요소들의 모음(elements collection)으로 구성된 모든 요소를 이 MutableCollection에 추가한다.

### 설명

`addAll`은 `MutableCollection` 클래스의 메소드로서, `Iterable` 타입의 구현체인 요소로 이뤄진 컬렉션을 `elements`라는 매개변수로 받은 값을 `MutableCollection` 클래스의 컬렉션에 값에 추가한다.

### 예제

```kt
fun main() {
    val list = mutableListOf<String>("A", "B")

    val moreItems: List<String> = listOf("C", "D") // Iterable<String>
    list.addAll(moreItems)

    println(list) // [A, B, C, D]
}

```
- `val list = mutableListOf<String>("A", "B")`: 문자열로 이뤄진 컬렉션의 요소로 `"A"`, `"B"`가 주어졌을 때 ([`MutableList`](./-mutable-list.md)는 원소를 추가하고 제거할 수 있는 일반적인 컬렉션을 나타내는 타입, [`mutableListOf`](./mutable-list-of.md)는 인자를 받아 `MutableList` 타입의 컬렉션을 반환하는 함수)
- `val moreItems: List<String> = listOf("C", "D")`: 추가할 컬렉션 요소로 `"C"`, `"D"`가 주어졌을 때 ([`List`](./-list.md)는 읽기 전용의 컬렉션을 나타내는 타입, [`listOf`](./list-of.md))는 `List` 타입의 컬렉션을 반환하는 함수
- `list.addAll(moreItems)`: `MutableList`인 `list` 변수에 `addAll` 컬렉션 메소드를 통해서 `List` 타입의 컬렉션인 `moreItems` 변수를 대입하여 `list` 변수의 컬렉션에 요소를 추가한 것
- `println(list)`: 최종 컬렉션의 값은 `MutableList`인 `[A, B, C, D]`가 반환된다.


## Sequence 시그니처

```
fun <T> MutableCollection<in T>.addAll(elements: Sequence<T>): Boolean
```


## Array 시그니처

```
fun <T> MutableCollection<in T>.addAll(elements: Array<out T>): Boolean
```


## References 
- https://kotlinlang.org/api/core/kotlin-stdlib/kotlin.collections/add-all.html
