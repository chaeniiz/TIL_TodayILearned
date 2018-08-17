## Navigation Architecture Component

- 이 화면 저 화면으로 왔다갔다 하는 네비게이션 작업을 간단하게 만들어 주는 컴포넌트
- Fragment Transaction, Up and Back, Passing Arguments, Deep Links, Error-prone Boilerplate



## Navigation Editor

- Canary build 버전에서 default로 지원
- 화면 간의 이동 경로를 드래그로 연결
- 화면 이동 애니메이션도 원클릭으로 지원
- and so on...



## Set up

1. build.gradle

```
dependencies {
    def nav_version = "1.0.0-alpha05"

    implementation "android.arch.navigation:navigation-fragment:$nav_version" // use -ktx for Kotlin
    implementation "android.arch.navigation:navigation-ui:$nav_version" // use -ktx for Kotlin

    // optional - Test helpers
    androidTestImplementation "android.arch.navigation:navigation-testing:$nav_version" // use -ktx for Kotlin
}
```

2. navigation.xml
   - res > New > Android Resource File > 'Navigation' Resource type
   - 네비게이션 할 요소들의 정의
     - 어떤 요소들이 네비게이션을 할 것인지
   - 요소의 액션 정의(이동 경로 설정 등)
     - A Fragment에서 B Fragment로 이동한다
     - 이동 시 부드럽게 넘어가는 효과 등의 애니메이션을 준다
     - 등등...
   - Design 탭에서 클릭 한 번으로 설정 가능!

```nav_graph.xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android">
</navigation>
```



## 이동경로 연결

blankFragment -> blankFragment2로 이동하는 경로를 설정한다면,

```nav_graph.xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android"
    app:startDestination="@id/blankFragment"> // 첫 경로 설정
    <fragment
        android:id="@+id/blankFragment"
        android:name="com.example.cashdog.cashdog.BlankFragment"
        android:label="fragment_blank"
        tools:layout="@layout/fragment_blank" >
        <action
            android:id="@+id/action_blankFragment_to_blankFragment2"
            app:destination="@id/blankFragment2" /> // action으로 이동 경로 설정
            // beginTransaction() replace... 등등의 작업을 대신 해 주어 따로 설정할 필요 X
    </fragment>
    <fragment
        android:id="@+id/blankFragment2"
        android:name="com.example.cashdog.cashdog.BlankFragment2"
        android:label="fragment_blank_fragment2"
        tools:layout="@layout/fragment_blank_fragment2" />
</navigation>
```



## NavHost 설정

- NavHost: 화면이 swap 되는 네비게이션이 일어나는 영역을 지정
- 네비게이션이 일어날 상위 화면 위에 지정
- 방금 전에 navigation 패키지에 만든 navGraph를 설정해 줌

```activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <fragment
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/my_nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        app:navGraph="@navigation/nav_graph"
        app:defaultNavHost="true" // system back button 누를 시 fragment가 닫히도록 함
        />

</android.support.constraint.ConstraintLayout>
```



## UI 위젯과 이동경로 연결

1. 상위 화면 내에 NavController 선언

```MainActivity.kt
val navController = findNavController(R.id.nav_host)
NavigationUI.setupActionBarWithNavController(this, navController)
NavigationUI.setupWithNavController(nav_bottom, navController)
```

2. button 등의 위젯에서 navigate() 작업 수행

```MainFragment.kt
btnDetail.setOnClickListener { view ->
    view.findNavController().navigate(R.id.action_mainFragment_to_detailActivity)
}
```



## Menu UI 컴포넌트와 연동(ex. BottomNavigationView)

1. bottomNavigationView 선언 내에 menu 연결

```main_activity.xml
<com.google.android.material.bottomnavigation.BottomNavigationView
    android:id="@+id/nav_bottom"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:layout_constraintBottom_toBottomOf="parent"
    app:menu="@menu/menu_bottom_nav" />
```

2. menu 내에 action 연결

```menu_bottom_nav.xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@id/mainFragment"
        android:title="MainFragment" />

    <item
        android:id="@id/action_mainFragment_to_settingsFragment"
        android:title="SettingsFragment" />
</menu>
```

3. Activity에 setupWithNavController()

```MainActivity.kt
NavigationUI.setupWithNavController(nav_bottom, navController)
```



## Navigate to Activity

1. 새로운 navigation_graph.xml를 만들어 연동

```nav_graph_detail.xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/detailFragment">

    <fragment
        android:id="@+id/detailFragment"
        android:name="com.rfrost.navigationsample.DetailFragment"
        android:label="DetailFragment"
        tools:layout="@layout/detail_fragment" />
</navigation>
```

```detail_activity.xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".DetailActivity">

    <fragment
        android:id="@+id/nav_host"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph_detail" />
        // 새로운 nav_graph 참조
</androidx.constraintlayout.widget.ConstraintLayout>
```

2. activity에 연동

```DetailActivity.kt
val navController = findNavController(R.id.nav_host)
NavigationUI.setupActionBarWithNavController(this, navController)
```


## 발표 후 논의점

- 뷰페이저로는 사용이 불가능할까?
  - ios처럼 바텀 네비게이션 바 기반으로, 뷰페이저 없이 화면 전환에 용이하게 만든 것이 아닐까?
- 프래그먼트간이 아닌 액티비티간 이동도 용이할까?
  - 가능
- argument 전달도 가능할까?
  - 가능



## 참고 링크

https://www.youtube.com/watch?v=8GCXtCjtg40
https://developer.android.com/topic/libraries/architecture/navigation/navigation-implementing

