---
title: "[안드로이드 스튜디오] 1.기초"
categories:
  - AndoridStudio
tags:
  - AndroidStudio
  - xml
  - java
comments:
  - true
toc: true
toc_sticky: true
---
## [안드로이드 스튜디오] 1. 기초

### 프로젝트 생성 후 기본 내용
* 안드로이드 스튜디오는 화면을 만드는데 그 화면을 액티비티라고 부르며, 화면 안에 들어갈 버튼, 이미지, 체크박스 등을 어딘가에 기록해서 이 화면과 매칭시켜야 하는데, 그 기록은 레이아웃에 한다.

* 즉 , 액티비티와 레이아웃이 한 쌍으로 움직여야 동작한다.

* androidManifest.xml은 이 앱에 대한 기본 설정이 들어가있는 부분.

* activity_main.xml에서 디자인 부분을 눌렀을 때, 레이아웃 중 중요한 것들로는 constraint, grid, linear, relative 등이 있다.

### 레이아웃 - linearlayout

* 주로 가장 겉에서 감싸는 레이아웃으로 linear나 relative 레이아웃을 많이 사용한다.

* linearlayout은 orientation 명령어를 통해 가로로 내용을 쓸지, 세로로 써서 구성할지를 정할 수 있다.

```xml
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="1"
        android:textSize="20sp"
        >
    </TextView>
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="2"
        android:textSize="20sp"
        >
    </TextView>

</LinearLayout>
```

* 위의 경우에는 화면에 1과 2라는 텍스트가 가로로 나열되서 출력된다.

![](/assets/img/AndroidStudio/0504_1.png)

* 위의 코드에서 orientation 부분을 vertical 로 바꾸면, 12출력이 위아래로 배치된다.

* 또한 LinearLayout에선 각 숫자가 적혀있는 텍스트 뷰가 차지하는 비율을 설정할 수 있다.

* weightSum 명령어를 통해 총 비율을 써준다. (코드 예시로는 9).

* 그 후 각각의 텍스트 뷰안에서 layout_weight 명령어를 통해 총 크기 9 중 얼만큼을 가져갈것인지를 적는다.

```xml
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:weightSum="9"
    >
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="1"
        android:textSize="50sp"
        android:layout_weight="1"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="2"
        android:textSize="50sp"
        android:layout_weight="5"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="3"
        android:textSize="50sp"
        android:layout_weight="3"/>

</LinearLayout>
```

![](/assets/img/AndroidStudio/0504_3.png)

### 레이아웃 - RelativeLayout

* RelativeLayout은 단어 그대로 서로의 관계를 통해서 위치를 지정할 수 있다.

* linear와 달리, orientation명령어를 사용하지 않고, 각각의 텍스트뷰에서 위치를 다른 텍스트 뷰를 통해서 위치를 지정할 수 있다.

```xml
<?xml version="1.0" encoding="utf-8"?>

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="1"
        android:textSize="20sp"
        android:layout_toLeftOf="@+id/TextView_2"
        android:layout_centerVertical="true"
        >
    </TextView>
    <TextView
        android:id="@+id/TextView_2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="2"
        android:textSize="20sp"
        android:layout_centerInParent="true"
        >
    </TextView>
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="3"
        android:textSize="20sp"
        android:layout_toRightOf="@+id/TextView_2"
        android:layout_centerVertical="true"
        >
    </TextView>
</RelativeLayout>
```

* 위의 코드를 보면, 먼저 텍스트 뷰중 2가 쓰여있는 텍스트뷰는 부모의 가운데라고 명령한다. 그 후, 1과 3의 텍스트 뷰는 2의 텍스트 뷰와의 관계를 통해 위치를 지정할 것이다.

* 2 텍스트뷰의 이름을 지정해야 1과 3을 2를 통해서 위치를 잡을 수 있으므로, 2번 텍스트 뷰의 이름을 id 명령어를 통해 TextView_2 로 이름을 지정한다.

* 1과 3을 toLeftOf 와 toRightOf 명령어를 통해 2번 텍스트뷰 왼쪽과 오른쪽에 배치한다.

* 하지만 2 텍스트 뷰는 부모의 가운데로 놓겠다고 해서, 가로 세로의 중간에 와있지만, 1과 3은 단순히 2의 왼쪽 , 오른쪽으로 배치하겠다고 했기때문에 위쪽에 배치가 되어있다.

* centerVertical 명령어를 통해 중간에 위치하게 만들어준다.

![](/assets/img/AndroidStudio/0504_2.png)

* 같은 방식으로 2번 텍스트의 위, 아래에 1번 3번 텍스트뷰를 두고싶으면, above, below 명령어를 사용하여 2번을 기준으로 배치하면 된다.

* 레이아웃안에 레이아웃을 중첩으로도 만들 수 있다.


---

#### 유투브 강의를 보며 혼자 공부한 부분이기때문에 틀린부분이 존재할 수 있습니다.

#### 출처: '센치한 개발자' 유투브 강의
