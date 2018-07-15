# Constraint Layout

## Constraint Layout은 왜 나오게 됐을까?

### '기존의 LinearLayout, RelativeLayout, FrameLayout 중 가장 성능이 좋은 레이아웃은 무엇일까?'에 대한 고민에서부터 시작해 보자.

- FrameLayout > LinearLayout > RelativeLayout 순으로 성능이 좋다.
- FrameLayout은 뷰에 gravity만 있을 뿐, 별도의 연산이 없으므로 가장 빨리 그려진다.
- LinearLayout과 RelativeLayout의 성능에는 큰 차이가 있는 것은 아니나, RelativeLayout는 id가 쌍방으로 묶여 있는 경우도 있고 모든 뷰가 그 기반이 되는 뷰를 찾아야 하는 연산과 복잡성이 증대될 수 있다는 점에서 더 복잡하다.
- 이러한 뷰의 최적화에 대한 고민과, 성능적인 면을 개선한 레이아웃이 바로 constraint layout!

## Constraint Layout의 성능이 좋은 이유는 무엇일까?

- 뷰 계층을 간단하게 하면서도 필요한 것을 구현할 수 있다.
- 뷰 호출 횟수 비교(간단한 UI의 경우로 Test)
  - LinearLayout: onMeasure() 10회 호출, 2depth
  - RelativeLayout: onMeasure() 14회 호출, 1depth
  - ConstraintLayout: onMeasure() 8회 호출, 1depth

## Constraint Layout을 왜 써야 할까?

- RelativeLayout에서는 불가능했던 자식 뷰 간의 상호 관계를 정의할 수 있다.
  - 두 View를 위 아래로 붙여서 컨테이너 중앙에 배치하기
  - 형제 View들과의 관계를 정의해서 레이아웃을 구성한다는 점은 RelativeLayout과 비슷하지만, 보다 유연하고 다양한 기능 제공
- weight를 쓰기 위해 LinearLayout을 써야만 했던 뷰 비율 조절도 가능
  - app:layout_constraintHorizontal_chainStyle
  - match_constraint 뷰 안에 app:layout_constraintHorizontal_weight
- 다양하고 유용한 기능들 제공
  - Barrier, Dimension, Guideline...

## 다양하고 유용한 기능

- **위치 속성**
  - layout_constraintTop_toTopOf
  - layout_constraintTop_toBottomOf
  - layout_constraintBottom_toTopOf
  - layout_constraintBottom_toBottomOf
  - layout_constraintLeft_toTopOf
  - layout_constraintLeft_toBottomOf
  - layout_constraintLeft_toLeftOf
  - layout_constraintLeft_toRightOf
  - layout_constraintRight_toTopOf
  - layout_constraintRight_toBottomOf
  - layout_constraintRight_toLeftOf 
  - layout_constraintRight_toRightOf 
  - left, right 정렬에 대해 start, end속성 지원
- **Chain**
  - 뷰끼리 체인으로 묶여 서로를 잡아당긴다는 개념으로, 묶인 것들끼리 뷰 공간을 공유함
  - LinearLayout의 weight 속성과 비슷한 개념
- **Barrier**
  - 뷰 공간을 분할해 놓는다는 개념으로, 장벽을 세움으로써 단을 나눠 놓는 것
  - 할당된 공간이 정해져 있지 않아서 뷰가 겹치는 것의 해결이 가능함
    - 예시 https://constraintlayout.com/basics/barriers.html
- **Dimension**
  - 정해진 비율로 뷰를 출력하고 싶을 때 비율을 정해 놓을 수 있는 기능
  - 피드 형식의 이미지뷰에서 유용하게 사용할 수 있음
- **Guideline**
  - 말 그대로 가이드 라인. 가이드라인을 세워 놓고 그 가이드라인에 기반하여 위, 아래, 왼쪽... 등으로 뷰 배치 가능
  - 가이드라인에 뷰를 딱 붙여 놓으면 그 가이드 라인에 맞춰서 뷰가 이동 가능!
    - 예시 https://constraintlayout.com/basics/guidelines.html



더 많은 예시는 ConstraintLayout 공식 홈페이지(https://constraintlayout.com)에서!