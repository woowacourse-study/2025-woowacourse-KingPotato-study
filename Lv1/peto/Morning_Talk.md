## 🤔 생성자 주입과 컴포지션 무엇을 선택해야할까 ?

### 💉 생성자 주입

- 클래스의 생성자 파라미터로 객체를 주입하는 것

```kotlin
class ViewController(
    private val inputView: InputView,
    private val outputView: OutputView
) 
```

### 🩻 컴포지션

- 객체를 프로퍼티로 갖는 것
- 인스턴스를 생성하는 `Class`는 인스턴스화 하는 `class`에 **의존**한다.
    - `ViewController Class`는 `inputView, OutputView class`에 의존한다.

```kotlin
class ViewController {
    private val inputView = InputView("페토")
    private val outputView = OutputView()
}
```

💡 **컴포지션에 대해 더 알고싶다면 ? Effective Kotlin Item 35. 상속보다 컴포지션을 사용하라**

## 📕 Example **scenario**

```kotlin
class Student(
    val name: String,
    val age: Int
//  val hoby: String    
)

class Inject(val item: Student) {  }

class Composition { 
	val item = Student("페토", 20) 
}
```

### 컴포지션일 때

```kotlin
class Student(
    val name: String,
    val age: Int
    val hoby: String
)

class Composition { 
	val items = Student("페토", 99, "축구") 
}
```

- `CompositionController Class` 또한 수정해야 한다.
- **왜 ?**
    - `CompositionController Class` 가 `Student Class`를 인스턴스화 하는 **책임**을 가지고 있다.
    - `Student Class`에 의존
- **무엇이 문제일까요 ?**
    - 두 Class는 **전혀 다른 책임을 가진 독립적인 객체**
    - 한 쪽의 클래스에 수정이 일어남으로써 이를 활용하는 클래스에서도 수정이 발생
    - **🚨 결합도의 증대 🚨**

### 🤝 결합도(Coupling)란?

- 클래스가 서로 얼마나 강하게 연결되어 있는지를 나타내는 지표
- 결합도가 높을수록 한 클래스가 변경될 때, 다른 클래스도 영향을 받음 → 유지보수 어려움

💡 결합도와 응집도가 왜 중요한지 궁금하다면 ? 
https://medium.com/@soqlehsoqleh/solid-a3717cb99ae8

### 생성자 주입일 때

```kotlin
class Student(
    val name: String,
    val age: Int
    val hoby: String
)

class Inject(val student: Student)
```

- `Student`의 변경이 `InjectController`에 직접적인 영향을 주지 않음
- **두 클래스 간의 결합도를 낮추어 유지보수성 향상**

### 🌠 활용 예시

```kotlin
class DependencyInjector {
    fun injectViewController(): ViewController {
        val inputView = injectInputView()
        val outputView = injectOutputView()
        val viewController = ViewController(inputView, outputView)
        return viewController
    }

    private fun injectInputView() = InputView()
    private fun injectOutputView() = OutputView()
}

fun main() {
    val di = DependencyInjector()
    di.injectViewController()
}
```

### 🍀 레아의 의견

> 💡 **InputView와 OutputView를 꼭 Controller에서 인스턴스로 가져야 할까 ?**

안드로이드 프로그래밍에선 메모리 사용 최적화를 위해 싱글톤을 사용하는 것을 지양하는 의견이 많다. 나도 이 의견에 전적으로 동의해 싱글톤은 가급적 지양한다.

하지만 현재와 같이 `Kotlin Console Programming` 환경에선 안드로이드에서와 같은 제약이 없으니 `Object`를 사용해 `View Class`들을 싱글톤으로 만들어 `Controller`와 `View Class`간에 의존성을 제거할 수 있다.
