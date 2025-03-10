# 📕 코틀린 소개
## **1. 코틀린은 정적 타입 지정 언어다**

> 💡정적 타입 지정 언어 : 프로그램 구성 요소의 타입을 컴파일 시점에 알 수 있다.

- **Compile Time**
    - 사람이 읽을 수 있는 언어로 표현된 코드를 **기계(JVM)가 읽을 수 있는 파일로 변환**한다.
    - 결과물로 .**class file**이 생성된다.
- **Run Time**
    - Compile Time에 생성된 `.class file`를 파일을 읽기 시작하는 시점

## 2. 코틀린의 변수와 클래스

- `final class` : 상속이 불가능한 class
    - 코틀린의 `class`는 기본적으로 상속이 불가능한 `final class`다.
    - 상속이 가능하게 만드는 키워드 `open`

```kotlin

class Person{
	val name: String
	var age: Int
	constructor(name: String, age:Int){
		this.name = name
		this.age = age
	}
}

// 이런 자바 스타일의 코드 초기화 기법에서 **변수의 선언과 초기화를 동시에** 하기 위해 만든 것이 생성자.
class Person(val name: String, var age: Int)

```

```kotlin
class Person(
	val name: String, 
	var age: Int,
	// 프로퍼티가 아닌 그냥 생성자의 파라미터 즉, getter, setter가 생성되지 않는다.
	nickName: String 
)

```

```kotlin
class Person(
    val name: String,
    var age: Int,
    nickName: String
){
    constructor(name: String, age: Int) : this(name, age, name)
}

class Person(
    val name: String,
    var age: Int,
    nickName: String = name // default value
) // 두 개의 생성자를 가진 class
```

- 필드
- 프로퍼티 : `field + getter + setter`
    
> 💡 프로퍼티의 탄생 배경 : 관습적으로 작성하는 코드(getter, setter)를 언어 자체 레벨에서 제공하자
    
- 코틀린은 두 키워드 (`val` 및 `var`)를 사용해 변수(**프로퍼티**)를 선언한다.

- **값이 변경되지 않는 변수**를 선언하며 자바의 **final**을 의미한다.
- 정확히는 “읽기 전용” 프로퍼티에 대한 선언이다.
- 프로퍼티 자체는 변경할 수 없지만 값을 변경할 수 있는 타입(Ex. MutableList)를 사용할 수 있기 때문

**var(variable)**

- 값이 변경될 수 있는 변수

## **3. 코틀린 컴파일**

```kotlin
class Person(
    val name: String,
    val age: Int
) {
    var nickname: String? = null
}

fun main(){
    val person = Person("찬호", 99)
}
```

Kotlin Compile File은 `build/classes/kotlin/main/파일명`에서 확인할 수 있다.

![image](https://github.com/user-attachments/assets/847f3c80-ebab-445f-b8c0-3234ee5f843d)


반대로 Java Code가 역으로 디컴파일 된 코드는 `Menu > Tools > Kotlin > Show Kotlin Bytecode`
![image](https://github.com/user-attachments/assets/25c11f60-ddcc-4867-8d5f-8fc2f9afb778)

```
@Metadata(
   mv = {2, 1, 0},
   k = 1,
   xi = 48,
   d1 = {"\u0000\u0018\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0000\n\u0002\u0010\u000e\n\u0000\n\u0002\u0010\b\n\u0002\b\u000b\u0018\u00002\u00020\u0001B\u0017\u0012\u0006\u0010\u0002\u001a\u00020\u0003\u0012\u0006\u0010\u0004\u001a\u00020\u0005¢\u0006\u0004\b\u0006\u0010\u0007R\u0011\u0010\u0002\u001a\u00020\u0003¢\u0006\b\n\u0000\u001a\u0004\b\b\u0010\tR\u0011\u0010\u0004\u001a\u00020\u0005¢\u0006\b\n\u0000\u001a\u0004\b\n\u0010\u000bR\u001c\u0010\f\u001a\u0004\u0018\u00010\u0003X\u0086\u000e¢\u0006\u000e\n\u0000\u001a\u0004\b\r\u0010\t\"\u0004\b\u000e\u0010\u000f¨\u0006\u0010"},
   d2 = {"LPerson;", "", "name", "", "age", "", "<init>", "(Ljava/lang/String;I)V", "getName", "()Ljava/lang/String;", "getAge", "()I", "nickname", "getNickname", "setNickname", "(Ljava/lang/String;)V", "Sources of kotlin-racingcar.main"}
)
public final class Person {
   @NotNull
   private final String name;
   private final int age;
   @Nullable
   private String nickname;

   public Person(@NotNull String name, int age) {
      Intrinsics.checkNotNullParameter(name, "name");
      super();
      this.name = name;
      this.age = age;
   }

   @NotNull
   public final String getName() {
      return this.name;
   }

   public final int getAge() {
      return this.age;
   }

   @Nullable
   public final String getNickname() {
      return this.nickname;
   }

   public final void setNickname(@Nullable String var1) {
      this.nickname = var1;
   }
}
// TestKt.java
import kotlin.Metadata;

@Metadata(
   mv = {2, 1, 0},
   k = 2,
   xi = 48,
   d1 = {"\u0000\b\n\u0000\n\u0002\u0010\u0002\n\u0000\u001a\u0006\u0010\u0000\u001a\u00020\u0001¨\u0006\u0002"},
   d2 = {"main", "", "Sources of kotlin-racingcar.main"}
)
public final class TestKt {
   public static final void main() {
      new Person("찬호", 99);
   }

   // $FF: synthetic method
   public static void main(String[] args) {
      main();
   }
}
```
