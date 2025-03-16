## 🎯 간결한 코드를 만들자

이는 람다의 존재 이유이며 람다가 탄생하게 된 목적이다.

- 함수를 만들기 위한 **최소한의 코드만 필요**하다.
- **다른 함수에 직접 삽입** 할 수 있다.
- 람다 식은 항상 중괄호로 둘러싸여 있다.
- 화살표(→) : 파라미터와 본문을 구분

<img width="649" alt="image" src="https://github.com/user-attachments/assets/3e41787d-93f9-431b-a605-d4150628fe79" />


```kotlin
// 식이 본문인 함수
fun sum(value: Int, value2: Int): Int = value + value2
```

♦️ **함수의 이름을 지정하지 않고** 변수에 담을 수 있다.(**Annoymous Function**)

♦️ **가장 마지막**에 작성한 수식의 결과를 반환한다.

```kotlin
val sum: (Int, Int) -> Int = { a1: Int, a2: Int -> a1 + a2 }
```

♦️ **타입 생략**이 가능하다

```kotlin
val sum: (Int, Int) -> Int = { a1, a2 -> a1 + a2 }
```

## Kotlin Collection Lamda Function Example

- Person Class List 원소 중 가장 나이가 많은 객체를 찾는다.

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 29), Person("Bob", 31))
    val max = people.maxBy("페토", { person -> person.age })
}
```

- 현재 `maxby`는 어떤 타입을 인자로 받아서 어떤 타입을 반환할까?
    
    ```kotlin
    val maxBy = (Int) -> Int
    ```
    
<img width="679" alt="image" src="https://github.com/user-attachments/assets/f7213dd8-7368-445d-b734-f458a9993b52" />

### 현재 코드를 개선해보자

- 함수 호출시 맨 뒤에 있는 인자가 람다 식이면 람다를 괄로 밖으로 뺄 수 있다

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 29), Person("Bob", 31))
    val max = people.maxBy(){ person -> person.age }
}
```

<img width="709" alt="image" src="https://github.com/user-attachments/assets/7b9b7a36-b9c8-43eb-82a9-21d9d03f65c6" />

- 람다가 유일한 인자이며 마지막 인자라면 호출시 괄호를 생략할 수 있다.

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 29), Person("Bob", 31))
    val max = people.maxBy{ person -> person.age }
}
```

- 람다의 파라미터가 하나뿐이고 그 타입을 컴파일러가 추론할 수 있다면 `it`을 바로 쓸 수 있다.

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    val people = listOf(Person("Alice", 29), Person("Bob", 31))
    val max = people.maxBy{ it.age }
}
```
