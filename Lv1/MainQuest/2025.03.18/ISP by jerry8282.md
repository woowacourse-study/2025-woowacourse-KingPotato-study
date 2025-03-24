# ✅ 인터페이스 분리 원칙

## 인터페이스 분리 원칙이란?

- 인터페이스 분리 원칙이란 **객체는 `자신이 사용하는 메서드`에만 의존해야 한다**는 법칙이다.
- 다시말해, 객체가 `사용하지 않는 메서드`를 의존해서는 안 된다는 뜻이다.

![image.png](attachment:8a86dc85-d261-41cc-86b8-3340e152ad95:image.png)

위 그림을 보면 User1,2,3가 OPS를 상속하고 있다. 
하지만, User1은 ops1메소드만 필요하다고 생각하면 User1은 OPS 인터페이스 내에 ops1만 가져오지 못하고 쓰지도 못하는 ops2, ops3도 override를 해야 될 것이다. 

아래 코드로 설명을 하겠다.

```kotlin
interface Animal{
    fun swim()

    fun fly()

    fun eat()
}

class Fish : Animal{
    override fun swim() {
        println("물고기가 헤엄칩니다.")
    }
    override fun fly() {
        println("사용안함")
    }
    override fun eat() {
        println("물고기가 밥을 먹습니다.")
    }
}

class Bird : Animal{
    override fun swim() {
        println("사용안함")
    }
    override fun fly() {
        println("새가 날아다닙니다.")
    }
    override fun eat() {
        println("새가 밥을 먹습니다.")
    }
}
```

위 코드를 보면 Fish와 Bird가 Animal이라는 interface를 상속받는다.
하지만, Animal이라는 인터페이스는 너무 많은 기능을 포함하고 있어 
현재 Fish와 Bird가 사용하지 않는 메소드까지 현재 가지고 있습니다.

```kotlin

interface Fish {
    fun swim()
}
interface Bird {
    fun fly()
}
interface Animal {
    fun eat()
}

class Goofy : Fish, Animal {
    override fun swim() {
        println("물고기가 헤엄칩니다.")
    }
    override fun eat() {
        println("물고기가 밥을 먹습니다.")
    }
}

class Pigeon : Bird, Animal {
    override fun fly() {
        println("새가 날아다닙니다.")
    }
    override fun eat() {
        println("새가 밥을 먹습니다.")
    }
}

```

위 내용들을 보기 전 SAM interface에 대해 공부를 하였을 대 저는 왜 interface를 잘게 나눈다고 하지? 혹은, interface에 왜 메서드를 하나만 사용하지? 라고 생각이 들었는데 위와 같이 isp원칙을 생각하면 interface를 최대한 작게 사용하는 것이 올바른 방법이라고 생각합니다.
