# ✅ 함수형 인터페이스

## `함수형 인터페이스`란?

하나의 추상 멤버 함수만 있는 인터페이스를 **기능 인터페이스** 또는 

**단일 추상 메서드 (Single Abstract Interface) 줄여서 SAM 이라고 한다.**

## `함수형 인터페이스`를 왜 쓰는가?

우선, 왜 하나의 인터페이스에 여러 메서드를 쓰면 되지 왜 하나의 인터페이스에 하나의 메서드가 필요해??

바로 익명의 내부 클래스를 사용할 때 하나의 인터페이스에 여러 메서드가 있을 경우 처리하기가 까다로워서 하나의 인터페이스에 하나의 추상메서드만 존재하는 패턴이 사용되었습니다.

- 위의 내용에 대해 자세한 건 기니 이걸 보시길..
    
    SAM 인터페이스라는 말은 코틀린에서 생겨난 개념이 아니라 저 옛날 자바 8이 나타났을 때부터 존재하던 말이다. 자바 8 이전에는 람다식이 지원되지 않아서 당시 개발자들은 익명 내부 클래스를 사용해 함수를 매개변수로 전달하거나 콜백을 사용하곤 했다. 그리고 익명 내부 클래스를 사용할 때 하나의 인터페이스에 여러 메서드가 있을 경우 처리하기가 까다로워서 하나의 인터페이스에 하나의 추상 메서드만 존재하는 패턴이 사용되었다. 
    

## 함수형 인터페이스는 알겠는데 `SAM 변환`은 뭐야?

SAM변환은 람다를 사용하여 코드를 더 간결하고 읽기 쉽게 만드는 데 사용이 됩니다.

## **`SAM 변환`을 쓰지 않은 경우**

```kotlin
fun interface IntPredicate {
    fun accept(i: Int): Boolean
}

fun main(){
    val isEven = object : IntPredicate {
        override fun accept(i: Int): Boolean {
            return i % 2 == 0
        }
    }
}
```

만약 위의 코드처럼 SAM 변환을 사용하지 않으면 위 코드와 같이 코드가 상당히 길어진다.

`아니 accept를 오버라이드를 하는 건 알겠는데 저기 object는 뭐지?`

이건 간단히 찾아봤는데 프로그램에서 일시적으로 사용되고 버려지는 객체라고 생각하면 편하다.

만약, 위의 코드에서 object를 사용하지 않는다면

```kotlin
fun interface IntPredicate {
    fun accept(i: Int): Boolean
}

class EvenPredicate : IntPredicate {
    override fun accept(i: Int): Boolean {
        return i % 2 == 0
    }
}

fun main(){
    val isEven = EvenPredicate()
}
```

위와 같이 클래스를 하나로 생성하게 되는 것이다.

### 여기서 질문 → 위의 익명 객체는 객체 생성에 대한 비용이 발생할까?

정답은 발생한다.

따라서, 익명객체 생성 비용을 없애기 위해 람다식을 사용하여 불필요한 객체 생성 비용을 없애준다.

## 실습 1번

```kotlin
interface MoveStrategy {
    val isMovable: Boolean
}

data class Car(val name: String, val position: Int) {
    fun move(moveStrategy: MoveStrategy): Car {
        if (moveStrategy.isMovable) {
            return copy(position = position + 1)
        }
        return this
    }
}

class CarTest {
    @Test
    fun 이동() {
        val car = Car("jason", 0)
        val actual: Car = car.move(object : MoveStrategy {
            override val isMovable: Boolean = true
        })
        assertThat(actual).isEqualTo(Car("jason", 1))
    }

    @Test
    fun 정지() {
        val car = Car("jason", 0)
        val actual: Car = car.move(object : MoveStrategy {
            override val isMovable: Boolean = false
        })
        assertThat(actual).isEqualTo(Car("jason", 0))
    }
}
```

## 위의 패턴을 보니 CarTest 안에서 object가 있네? 위에서 배운 걸 활용하여 익명 객체의 사용을 없애자!

### 첫번째 방법 → 익명 객체인 object를 사용했네? 클래스를 따로 생성해볼까?

```kotlin
interface MoveStrategy {
    val isMovable: Boolean
}

data class Car(val name: String, val position: Int) {
    fun move(moveStrategy: MoveStrategy): Car {
        if (moveStrategy.isMovable) {
            return copy(position = position + 1)
        }
        return this
    }
}

class CarStatusMove : MoveStrategy {
    override val isMovable: Boolean = true
}

class CarStatusStop : MoveStrategy {
    override val isMovable: Boolean = false
}

class CarTest {
    @Test
    fun 이동() {
        val car = Car("jason", 0)
        val actual: Car = car.move(CarStatusMove())
        assertThat(actual).isEqualTo(Car("jason", 1))
    }

    @Test
    fun 정지() {
        val car = Car("jason", 0)
        val actual: Car = car.move(CarStatusStop())
        assertThat(actual).isEqualTo(Car("jason", 0))
    }
}
```

object인 익명 객체가 사라져서 가독성이 좋아졌지만 여전히 객체 생성 비용이 발생하고 있다.

### 두번째 방법 → SAM변환을 사용해서 객체 생성 비용을 없애자!

```kotlin
fun interface MoveStrategy {
    fun isMovable(): Boolean
}

data class Car(val name: String, val position: Int) {
    fun move(moveStrategy: MoveStrategy): Car {
        if (moveStrategy.isMovable()) {
            return copy(position = position + 1)
        }
        return this
    }
}

class CarTest {
    @Test
    fun 이동() {
        val car = Car("jason", 0)
        val actual: Car = car.move{ true }
        assertThat(actual).isEqualTo(Car("jason", 1))
    }

    @Test
    fun 정지() {
        val car = Car("jason", 0)
        val actual: Car = car.move { false }
        assertThat(actual).isEqualTo(Car("jason", 0))
    }
}
```

### 실습 코드 디컴파일

```kotlin
public final class CarTest {
   @Test
   public final void 이동() {
      Car car = new Car("jason", 0);
      Car actual = car.move(new MoveStrategy() {
         private final boolean isMovable = true;

         public boolean isMovable() {
            return this.isMovable;
         }
      });
      Assertions.assertThat(actual).isEqualTo(new Car("jason", 1));
   }

   @Test
   public final void 정지() {
      Car car = new Car("jason", 0);
      Car actual = car.move(new MoveStrategy() {
         private final boolean isMovable;

         public boolean isMovable() {
            return this.isMovable;
         }
      });
      Assertions.assertThat(actual).isEqualTo(new Car("jason", 0));
   }
}

```

### 첫번째 방법 디컴파일

```kotlin
public final class CarTest {
   @Test
   public final void 이동() {
      Car car = new Car("jason", 0);
      Car actual = car.move((MoveStrategy)(new CarStatusMove()));
      Assertions.assertThat(actual).isEqualTo(new Car("jason", 1));
   }

   @Test
   public final void 정지() {
      Car car = new Car("jason", 0);
      Car actual = car.move((MoveStrategy)(new CarStatusStop()));
      Assertions.assertThat(actual).isEqualTo(new Car("jason", 0));
   }
}

```

### 두번째 방법 디컴파일

```kotlin
public final class CarTest {
   @Test
   public final void 이동() {
      Car car = new Car("jason", 0);
      Car actual = car.move(CarTest::이동$lambda$0);
      Assertions.assertThat(actual).isEqualTo(new Car("jason", 1));
   }

   @Test
   public final void 정지() {
      Car car = new Car("jason", 0);
      Car actual = car.move(CarTest::정지$lambda$1);
      Assertions.assertThat(actual).isEqualTo(new Car("jason", 0));
   }

   private static final boolean 이동$lambda$0() {
      return true;
   }

   private static final boolean 정지$lambda$1() {
      return false;
   }
}

```
