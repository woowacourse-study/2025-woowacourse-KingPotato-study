> Fragment를 add()와 replace()로 추가할 수 있는데, 두 방법을 잘못 섞어쓰면 어떤 문제가 발생할 수 있을까요?


> Fragment를 교체하면서 매번 addToBackStack(null)을 걸어주는 게 항상 좋은 방법일까요?


> Fragment의 생명주기가 Activity의 생명주기에 따라 움직인다고 했는데, Activity가 사라져도 Fragment가 메모리에 남아 있을 수 있을까요?

> Activity 없이 Fragment만 단독으로 화면에 띄울 수 있을까요?

> 만약 태블릿 화면에서는 한 Activity 안에 두 개의 Fragment를 스마트폰 화면에서는 각각 별도의 Activity를 사용하고 싶으면 어떻게 해야 할까요?

> DataBinding을 사용하면 무조건 코드가 깔끔해질까요? 모든 프로젝트에 적용하는 것이 좋을까요?

> ViewBinding과 DataBinding 모두 findViewById를 대체할 수 있는데, 그럼 ViewBinding만 써도 충분하지 않을까요? 왜 굳이 DataBinding을 쓰는 걸까요?

> DataBinding 표현식에서 복잡한 if-else 로직이나 여러 계산식을 넣으면 어떤 문제가 생길까요

> Presenter를 DataBinding으로 View에 넘기면 어떤 문제가 발생할까요?

> ViewBinding과 DataBinding 중 어떤 것을 선택할지, 기준을 세워보세요.

> Fragment에서 DataBinding 객체를 onDestroyView에서 해제하지 않으면 무슨 문제가 발생할까요?
