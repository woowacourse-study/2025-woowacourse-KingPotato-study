### 🤔 Quiz. 아래 코드에서 `bar1` 과 `bar2` 는 어떤 차이가 있을까요?

```
class FooBar {
    val foo = 123
    val bar1 = foo
    val bar2 get() = foo
}
```

- 나의 답변  : `bar1` 는 `foo`의 초기값을 복사해 저장한다. 반면 `bar2`는 `foo`의 값이 변경될 때 마다 값이 동일하게 변경된다.

### 🍀 해석

**디컴파일 된 코드를 통해 이해해보자.**

![image](https://github.com/user-attachments/assets/772eaa9b-28c0-46db-b2d1-9ca1b9435788)


`foo`와 `bar1`는 필드가 생성 되었으며 `val`로 선언 했기에 `getter`만 생성되었다.

`bar1`은 `val bar1 = foo` 로 선언되어 있지만 실제로는 `foo`의 값을 복사하는 것이 아니라 **객체의 필드로 저장**된다.

즉, **`bar1`이 초기화될 때 `foo`의 현재 값이 필드에 저장**되며, 이후에는 변경되지 않는다.

그런데 `bar2`는 분명 `getter`가 생성 되었음에도 필드는 생성되지 않았다.

```kotlin
public final int getBar2() {
  return this.foo;
}
```

`val`을 통해 초기화한 프로퍼티는 `bar1`과 같이 디컴파일 되었을 때 `Field`, `getter`가 생성된다.

하지만 `bar2`는 현재 `getter`만 생성되어 있다.

`bar2`는 `custom getter`를 사용하기 때문에 **별도의 저장 공간(필드)을 만들 필요가 없다.**

그 대신 **호출될 때마다 `foo`의 현재 값을 반환**하는 `getter`만 존재한다.

즉, **bar2**는 **값을 저장하는 변수**가 아니라, **계산된 값을 반환하는 함수**처럼 동작한다.

### JVM 내부 관점에서 살펴보자

![image](https://github.com/user-attachments/assets/f01ad31c-1f81-4d27-b059-42341500140c)

### **Method Area (클래스 메타정보 저장 영역)**

- `FooBar` 클래스 자체의  **바이트코드**와 메서드 정보(`getter`)가 저장된다.

### **Heap (힙 메모리)**

- `FooBar` 객체가 인스턴스화 되면 **힙(Heap)** 영역에 객체가 생성된다.
- `foo, bar1` 은 `FooBar` 인스턴스에 속하는 프로퍼티이므로 `FooBar` 객체가 생성될 때 **Heap 영역에 저장된다.**
- `bar2`는 **별도의 필드를 가지지 않으며**, 매번 `getter`가 실행되어 `foo` 값을 반환한다.
    - 즉, `bar2` 자체는 `Heap`에 저장되지 않고, 실행될 때마다 `getter` 메서드가 호출되며 `foo`의 값을 참조한다.

### **Stack (스택 메모리)**

- `FooBar` 객체가 인스턴스화 되었을 때 이를 참조하는 **참조 변수(reference)**는 **Stack 영역**에 저장된다.
