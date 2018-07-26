# Creating Animations With MotionLayout for Android

- 복잡한 애니메이션을 구현하는 것은 시간 소비가 큰 작업이다. MotionLayout은 XML만으로도 복잡한 애니메이션 구현을 쉽고 자연스럽게 해 준다! 
- 액티비티 코드 내에는 클릭 리스너를 설정하는 작업이나 계산값이 필요한 경우의 계산, XML 파일에서 정의한 동작의 불러오기 정도만 구현이 된다.
- Android support library에 추가돼 있고, ConstraintLayout을 extend한다.
- API 레벨 18 이상부터 사용 가능
- MotionLayout이 수행할 작업에 대한 실제 동작 정의는 레이아웃 파일에 포함되지 않고, 별도의 xml 파일 내에 포함됨(MotionScene)
- MotionLayout에는 View와 properties만 포함
- MotionScene 안에 정의된 ConstraintSets들의 트랜지션을 지원함



# 구현 방법

1. dependencies에 constraint layout 2.0.0 버전 추가

2. ConstraintLayout을 extend하는 motion layout을 사용하고 싶은 레이아웃에 선언(View)

3. 애니메이션을 구현할 xml 파일을 xml 패키지 내에 만든다

4. 3번의 xml 파일 안에 MotionScene을 만들고, 애니메이션의 시작점과 끝나는 점을 지정해서 애니메이션을 만든다

   4-1. 시작점과 끝나는 점은 ConstraintSet으로 만들고 id로 구분한다

5. 시작점에서 끝나는 점으로 가는 동작은 Transition에서 지정한다.

   5-1. 시작점: app:constraintSetStart, 끝나는 점: app:constraintSetEnd

   5-2. duration, OnClick, OnSwipe 등의 다양한 동작들도 Transition 내에서 구현 가능

6. 시작에서 끝으로 진행되는 애니메이션 사이에 다른 동작들을 추가하고 싶다면, Transition 내에 Keyframe > KeyPosition들을 추가한다. 단순히 지정한 위치로 이동하는 것이 아니라 이동하는 모양을 지정할 수도 있다!(KeyCycle)

   