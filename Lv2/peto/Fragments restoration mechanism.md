# 📱 Fragment 자동 복원 매커니즘 with Custom Construct

### 🔄 구성 변경 시 Fragment는 어떻게 복원될까?
안드로이드 시스템은 화면 회전 등의 구성 변경이 발생하면 다음과 같은 절차를 따릅니다:

+ 현재 Activity가 파괴되고(`onDestroy`) 새로 생성(`onCreate`)
+ 이때 내부에 있는 모든 `Fragmen`t들도 함께 파괴됨
+ `FragmentManager`가 자동으로 이전 상태를 저장하고, 다시 복원

#### 💡 상태 저장은 언제?
```
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    // 이 시점에 FragmentManager가 상태를 저장함
}
```
여기서 저장되는 내용은 다음과 같습니다:

+ 각 `Fragment`의 클래스 이름
+ `setArguments()`로 전달한 `Arguments Bundle`
+ `onSaveInstanceState()`에서 개발자가 저장한 상태값
+ `View`의 상태 (EditText 입력값, 스크롤 위치 등)
+ `Fragment` 백스택 상태

#### 🔁 자동 복원 흐름 요약
+ Activity.onSaveInstanceState() → 내부적으로 `FragmentManager.saveAllState()` 호출
+ `Fragment.onSaveInstanceState(Bundle)`에서 추가 상태 저장 가능
+ 시스템이 `Activity` 및 `Fragment` 파괴
+ 새 `Activity` 생성 → 전달된 `savedInstanceState`에서 `Fragment` 복원
+ `Fragment.onCreate(savedInstanceState)` 호출 → 여기서 상태 복원 가능
+ `onCreateView() → onViewCreated() → UI 자동 복원`

#### ⚠️ 커스텀 생성자 쓰면 안 되는 이유

```
public class BadFragment extends Fragment {
    public BadFragment(int value) {
        // 값 초기화
    }
}
```
+ FragmentManager는 리플렉션을 통해 해당 Fragment 클래스의 매개변수 없는(public no-arg) 생성자를 호출하여 인스턴스를 생성
+ 기본 제공되는 FragmentFactory는 오직 무인자 생성자만을 호출하며, 커스텀 생성자를 사용하는 경우 이를 지원하지 않음
+ 예를 들어 MyFragment(int arg) 같은 커스텀 생성자만 정의해 두고 기본 생성자를 제공하지 않으면, 복원 시점에 FragmentManager가 호출할 수 있는 생성자가 없어 `InstantiationException`이 발생
+ 실제 구현을 보면, `Fragment.instantiate()`는 내부적으로 clazz.newInstance()를 호출하는데, 이 메서드는 반드시 파라미터 없는 기본 생성자를 사용해야 합니다
+ 또한, 화면 회전 등의 재생성 때 `FragmentManager`는 기존에 커스텀 생성자로 초기화했던 필드값이나 인수를 복원해 주지 않으므로 데이터가 모두 유실
  + 예를 들어 new MyFragment(123)으로 인스턴스를 만들었더라도 재생성 시에는 new MyFragment()가 호출되어 123 값이 손실
  + 이로 인해 null 참조나 초기화 누락이 발생할 수 있습니다

. 따라서 공식 권장 방식은 생성자를 오버로딩하지 않고, 필요한 인자는 Bundle을 통해 setArguments()/getArguments()로 주고받는 것
+ Android 예제 코드에서도 newInstance() 같은 팩터리 메서드로 Bundle을 설정한 후 Fragment를 생성하는 패턴을 일관되게 사용합니다

#### ✅ Bundle과 newInstance() 패턴
Android에서는 다음과 같은 패턴을 공식적으로 권장합니다:
```
class MyFragment : Fragment(R.layout.my_fragment) {

    companion object {
        fun newInstance(someValue: Int): MyFragment {
            val fragment = MyFragment()
            val args = Bundle().apply {
                putInt("some_int", someValue)
            }
            fragment.arguments = args
            return fragment
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val value = requireArguments().getInt("some_int")
        // 값 사용
    }
}
```
