---
title: "[안드로이드 스튜디오] 2.로그인 화면 디자인하기"
categories:
  - AndoridStudio
tags:
  - AndroidStudio
  - xml
  - java
comments:
  - true
---

## [안드로이드 스튜디오] 2.로그인 화면 디자인하기

### 기본적인 틀 작성

* 구글에서 app design 샘플 화면을 하나 보고 카피를 해보았다.

* 대략적으로 모든 레이아웃들이 가로로 나열되다가, 비밀번호 찾기와 회원가입 뷰만 세로로 나열된다.

* 이 경우 첫번째 포스팅에서 한대로 가장 바깥을 감싸는 LinearLayout 을 만들고, orientation 을 vertical로 설정해준다.

* 텍스트뷰, 이미지뷰, 버튼 뷰등을 이용하여 xml 파일을 구성해준다.

* 마지막 완성화면에 첨부하겠지만 비밀번호 찾기와 회원가입은 세로로 배열되어있기 때문에 LinearLayout 안에 새로 LinearLayout을 만들고 orientation을 horizontal로 설정한 뒤, 그 안에 두개의 텍스트 뷰를 넣는다.

### 머터리얼 디자인 가져다 쓰기

* 구글에 Andorid support design이라고 친 후 홈페이지에 들어간 후, 디자인파트를 찾아본다.

* 'com.android.support:design:27.1.1' 이런식으로 명령을 입력하라는 문장이 보일 것

* 내 프로젝트 gradle 폴더에서 gradle(module app) 파일에 implementation 'com.android.support:design:27.1.1' 이런 명령어를 써준다.

* 단, 뒤의 숫자는 내 프로젝트 숫자와 일치하게 써야한다.

* 머터리얼 디자인을 안드로이드에서 제공하니깐 가져다 쓰는 것이다.

* 추가하였으면 sync now 버튼을 눌러 다운을 받자.

### 온라인 포토샵 사용하기

* 인터넷에서 원하는 이미지의 색상이 있으면 이미지를 새탭에서 연 후 주소를 복사해 놓는다.

* 구글에 online photoshop을 검색하여 pixlr Editor에서 주소를 붙혀넣은 후, 색상을 추출한다.

* 그 6자리 숫자가 그 색상의 번호

### 아이콘 받아오기

* iconfinder나 material icon 사이트에서 원하는 아이콘들을 다운받는다. (라이센스 허가된 아이콘들만 가능하다.)

* 다운받은 아이콘 그림들을 프로젝트에 넣어줘야한다. 일단은 기본으로 res 폴더 안에있는 drawble 에 추가해주자.

### 각자 버튼 효과주기

* drawble 폴더 아래에 xml 파일을 하나 만들어주고, 이 파일이 디자인할 것들을 저장시켜 놓는다.

* 그 후, 메인xml에서 위의 효과를 주고싶으면, 저 위치를 표시하면 된다.

* 아래 코딩 참조

```xml
<Button
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:background="@drawable/btn_facebook"
        android:textColor="#ffffff"
        android:text="Login with FaceBook"
        android:layout_marginBottom="20dp"/>
```

* 이 버튼 뷰는 페이스북 로그인 버튼을 디자인 하는 뷰이다.

* background에 btn_facebook.xml을 참조하고 있다.

```xml
<?xml version="1.0" encoding="utf-8"?>

<selector xmlns:android="http://schemas.android.com/apk/res/android"
    >
    <item android:state_enabled="true">
        <shape>
            <solid android:color="#3a5994">

            </solid>
            <corners android:radius="20dp">

            </corners>
        </shape>
    </item>

</selector>
```

* 위 코드가 btn_facebook.xml 파일의 코드이다. 기본 상태의 버튼은 색상이 #3a5994 이고, 둥글기가 20dp인 버튼 background를 지정한 코드이다.

### edittext 뷰 꾸미기

* 머터리얼 디자인에서 받아온 것으로 edittext를 꾸미려면, 그냥 edittext 뷰를 사용하지 않고 다른 방식으로 코딩을 한다.

```xml
<android.support.design.widget.TextInputLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <android.support.design.widget.TextInputEditText
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:hint="Password"
            android:layout_marginBottom="30dp"/>
    </android.support.design.widget.TextInputLayout>
```

### login.xml 풀코드

```xml
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingLeft="25dp"
    android:paddingRight="25dp"
    android:paddingTop="20dp"
    >
   <TextView
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:gravity="center"
       android:textColor="@android:color/holo_blue_dark"
       android:textSize="25sp"
       android:text="LOGIN"
       android:layout_marginBottom="20dp"/>

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="100dp"
        android:background="@null"
        android:src="@drawable/login_icon"
        android:gravity="center"
        android:layout_marginBottom="20dp"
        />

    <android.support.design.widget.TextInputLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <android.support.design.widget.TextInputEditText
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:hint="Email"
            android:layout_marginBottom="16dp"/>
    </android.support.design.widget.TextInputLayout>

    <android.support.design.widget.TextInputLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <android.support.design.widget.TextInputEditText
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:hint="Password"
            android:layout_marginBottom="30dp"/>
    </android.support.design.widget.TextInputLayout>

    <Button
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:background="@drawable/btn_blue"
        android:textColor="#ffffff"
        android:text="LOGIN"
        android:layout_marginBottom="16dp"/>
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:text="OR"
        android:layout_marginBottom="16dp"/>
    <Button
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:background="@drawable/btn_facebook"
        android:textColor="#ffffff"
        android:text="Login with FaceBook"
        android:layout_marginBottom="20dp"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:weightSum="10">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:text="Find Password"
            android:textSize="25sp"
            android:layout_weight="5"/>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:text="Sign Up"
            android:textColor="#db7b7b"
            android:textSize="25sp"
            android:layout_weight="5"/>
    </LinearLayout>

</LinearLayout>
```

![](/assets/img/AndroidStudio/0504_4.png)

---

* 틀린 부분이 존재할 수 있습니다.

* 공부한 부분을 혼자 적어두기 위해 포스팅한것이기 때문에 정리된 포스팅이 아닙니다.

* 출처: '센치한 개발자' 유투브 강의
