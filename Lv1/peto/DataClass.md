# 📌 Data Class

- `Kotlin`에서 데이터의 집합을 표현할 때 사용되는 `Class`
- `final class`이기 때문에 **상속을 허용하지 않는다.**

`Data Class`는 컴파일된 형태를 보면 일반 `class`와의 차이를 알 수 있다.

```kotlin
data class Person(
    val name: String,
    val age: Int
)
```

![image](https://github.com/user-attachments/assets/20546bdb-185f-40e0-9022-b9ef05c3de64)

일반 클래스와의 차이점은 `componentN`, `copy`, `equals`, `hashCode`, `toString` 메소드가 오버라이드 된다. 

#### 각 메소드의 역할을 알아보자

### `componentN`

- **주 생성자에 선언된 N번째 프로퍼티 반환**

```kotlin
data class Person(
    val name: String,
    val age: Int,
)

fun main(){
    val person = Person("Chan", 99)
    println(person.component1())
    println(person.component2())
}

// chan
// 99
```

- `componentN` 함수는 선언된 **주 생성자의 파라미터 개수만큼 자동 생성된다.**
- 즉, `data class`가 가지는 프로퍼티 개수에 따라 `component1()`, `component2()`, ..., `componentN()`이 자동으로 만들어진다.

```kotlin
data class Person(
    val name: String,
    val age: Int,
    val nickName: String,
    val nickName1: String,
    val nickName2: String,
    val nickName3: String,
    val nickName4: String,
)

// compile
public final data class Person public constructor(name: kotlin.String, age: kotlin.Int, nickName: kotlin.String, nickName1: kotlin.String, nickName2: kotlin.String, nickName3: kotlin.String, nickName4: kotlin.String) {
    public final val name: kotlin.String /* compiled code */

    public final val age: kotlin.Int /* compiled code */

    public final val nickName: kotlin.String /* compiled code */

    public final val nickName1: kotlin.String /* compiled code */

    public final val nickName2: kotlin.String /* compiled code */

    public final val nickName3: kotlin.String /* compiled code */

    public final val nickName4: kotlin.String /* compiled code */

    public final operator fun component1(): kotlin.String { /* compiled code */ }

    public final operator fun component2(): kotlin.Int { /* compiled code */ }

    public final operator fun component3(): kotlin.String { /* compiled code */ }

    public final operator fun component4(): kotlin.String { /* compiled code */ }

    public final operator fun component5(): kotlin.String { /* compiled code */ }

    public final operator fun component6(): kotlin.String { /* compiled code */ }

    public final operator fun component7(): kotlin.String { /* compiled code */ }

    public final fun copy(name: kotlin.String = COMPILED_CODE, age: kotlin.Int = COMPILED_CODE, nickName: kotlin.String = COMPILED_CODE, nickName1: kotlin.String = COMPILED_CODE, nickName2: kotlin.String = COMPILED_CODE, nickName3: kotlin.String = COMPILED_CODE, nickName4: kotlin.String = COMPILED_CODE): Person { /* compiled code */ }

    public open operator fun equals(other: kotlin.Any?): kotlin.Boolean { /* compiled code */ }

    public open fun hashCode(): kotlin.Int { /* compiled code */ }

    public open fun toString(): kotlin.String { /* compiled code */ }
}

```

## `toString`

- `data class`의 프로퍼티 출력 형태를 재정의 할 수 있다.

```kotlin

data class Person(
    val name: String,
    val age: Int,
){
    override fun toString(): String {
        return "이름은 $name 나이는 $age 입니다."
    }
}

fun main(){
    val person = Person("Chan", 99)
    print(person.toString())
}
//이름은 Chan 나이는 99 입니다.

```

## `Copy`

- 기존의 객체를 `shallow copy`한 객체를 만든다.
- **Human Example scenario** [ 사물함 내에 물건을 담은 서랍장이 있다. ]
    - 깊은 복사 : 사물함에 있는 **물건을 그대로**지만 **기존의 물건이 아닌 새로운 물건**으로 채운 것
    - 얕은 복사 : 사물함 내에 **물건들은 그대로**, 내부의 물건도 **동일한 물건**
- **Launguage Example scenario**

```kotlin
data class Person(
    val name: String,
    val books: List<String>
)

fun main(){
    val person = Person("chan", listOf("코틀린", "자바"))
    val person2 = person.copy()
    println(person == person2) // true
    println(person === person2) // false
    println(person.books == person2.books) // true
    print(person.books[0] === person2.books[0]) // true

}
```

- `person == person2`
    - `equeals`를 `override` 하기 때문에 내부 구성 요소가 같음
- `person === person2`
    - `equeals`를 사용한 구성 요소간의 비교가 아닌 **객체의 메모리 주소값**을 비교한다.
    - 원본 객체를 복사해 새로운 객체를 만드는 것으로 **원본 객체와는 다른 주소값**을 가진다.
- `person.books == person2.books`, `person.books[0] === person2.books[0]`
    - 동일한 원본 객체(List)의 주소를 참조한다.

## `Equals & HashCode`

- `Equals`와 `HashCode` 간엔 반드시 지켜야 하는 규약이 있다.
    - Class에 `equals`를 정의했다면, 반드시 `hashCode`도 재정의해야 한다.
    - 2개 객체의 `equals`가 동일하다면 반드시 `hashCode`도 동일한 값이 리턴되어야 한다.
    - 이런 조건을 지키지 않을 경우 `HashMap`, `HashSet` 등의 컬렉션 사용 시 문제가 생길 수 있다.

## 🤔 Data Class vs Class
`Data class`와 일반 `class` 중 어떤 것을 사용해야할까 ?

`Data Class`의 기능에 대해 다시 생각해보자. 

일반 클래스와의 차이점은 `componentN`, `copy`, `equals`, `hashCode`, `toString` 메소드가 오버라이드 된 다고 했다.

즉 이 메소드들은 **무조건**  **override**된다. 

그렇다면, 이 메소드들을 사용하지 않는다면 불필요한 메소드들이 오버라이드 되며 이로 인해 불필요한 **오버헤드가 발생**할 것이다. 

페어 프로그래밍을 하며 페어와 토론한 결과 위 메소드들을 사용할 필요가 없다면 일반 class를 사용했다.

그 동안 나는 당연한 것 처럼 data class만을 사용해왔다. 

앞으로는 두 클래스를 사용하기 위한 1차적인 기준은 오버라이드 되는 메소드들의 사용 여부다.
