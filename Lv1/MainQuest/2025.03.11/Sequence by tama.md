# Sequence

Kotlin에서 **컬렉션(Collection)** 을 사용하면, 연산이 진행될 때마다 *새로운 컬렉션*이 만들어진다. 

→ ⚠️ **새로운 컬렉션을 생성하는 비용** 이 발생 !! 특히, **대량 데이터** 처리 시에는 성능 저하 발생할 수 있다.

반면, **Sequence** 는 **지연 실행(Lazy Evaluation)** 을 지원하여, map()이나 filter() 같은 연산이 즉시 실행되지 않는다(즉시 새로운 컬렉션이 생성되지 않는다).
이러한 연산들은 **연산을 추가하는 역할만 하며** ,  **실제 연산은 toList(), count() 같은 최종 연산이 호출될 때 수행된다.**

특히, Sequence는 최종 연산을 호출할 때마다 원소들을 처음부터 다시 처리하기 때문에, **각 원소에 접근하는 연산**을 수행할 때 이점이 있다.

### 🧐 **stateless vs Stateful 연산과 Sequence**

- **Stateless (상태가 없는) 연산**
    - 원소들을 개별적으로 처리하며 **이전 원소의 영향을 받지 않음**
    - 이전 상태를 기억하지 않아도 된다.
    - ❗ **`Sequence`와 함께 사용할 때 큰 성능 이점**
    - 예 : map(), filter(), take(), drop()

  ```
  val numbers = listOf(1, 2, 3, 4, 5)
  val result = numbers.asSequence().map { it * 2 }.filter { it > 5 }
  ```

- **Stateful (상태가 있는) 연산**
    - **전체 데이터를 고려해야 하는 연산**
    - ⚠️ 원소 수가 많을 경우 **비효율적**
    - ❗ **`Sequence`를 사용해도 중간 연산이 바로 실행되지 않음 → 사용성 떨어짐**
    - 예: sorted(), distinct(), groupBy()
    → sorted()는 전체 데이터를 정렬해야 하므로, 모든 원소를 한 번에 처리해야 한다.

  ```
  val numbers = listOf(3, 1, 4, 1, 5, 9, 2).asSequence().sorted()
  ```


> 📌 **데이터 크기와 연산 방식에 따라 Collection과 Sequence를 적절히 선택해야한다**
