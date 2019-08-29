---
title: "[안드로이드 스튜디오] AsyncTask 란?"
categories:
  - AndoridStudio
tags:
  - AndroidStudio
  - AsyncTask
comments:
  - true
---

## [안드로이드 스튜디오] AsyncTask 란?

* 안드로이드 프로젝트를 진행하면서 AsyncTask를 사용한 코드를 참 많이 접하고 사용하였는데, 정확히 어떤 역할을 하는 것인지 잘 몰라서 찾아보았다.
* AsyncTask는 __백그라운드 스레드와 메인스레드를 같이 쓰기 쉽게 설계가 되어있다.__
* 비동기 작업을 손쉽게 할 수 있도록 안드로이드 api 3부터 제공하는 기능.
* AsyncTask를 사용하면 백그라운드에서 원격의 이미지 파일을 다운받는 등의 시간소모를 메인 스레드에 영향을 주지 않으면서 처리 가능.

### AsyncTask 관련 메소드
* __execute()__ : 생성한 AsyncTask 상속한 객체를 실행시켜서 이 객체는 이때부터 백그라운드 작업을 수행하기 시작. 필요한 경우에 그 결과를 메인 스레드에서 실행.
* __cancel()__ : execute()와 반대로 작업을 취소할 수 있다.
* __onPreExecute()__ : background 작업을 알리는 즉 시작되지마자 실행될 코드. progress.dialog등 다양한 popup 메시지들을 주로 사용.
* __doInBackground(String... params)__ : 보통 일반적으로 네트워크 등을 포함한 다양한 일처리등을 해준다.
* __onPostExecute(String s)__ : background에서 처리한 부분을 마지막으로 처리하는 부분이다. ui에 어떤것을 전달하든, 어떤 결과를 출력할 때 사용됨.

### AsyncTask를 new 하면 나오는 인자.
* AsyncTask<Params,Progress,Result>
* __Params__ : Background 작업을 할 때 필요한 데이터 타입을 정의한다.
* __Progress__ : Background 작업 도중 진행하는데 사용될 데이터 타입 정의.
* __Result__ : 작업 결과로서 리턴 받을 데이터 타입을 정의.

```java
class TestAsyncTask extends AsyncTask<String, Void, String>{
    @Override
    protected void onPreExecute() {
        super.onPreExecute();
        // doInBackground가 실행되기 전에 실행
    }
    @Override
    protected String doInBackground(String... strings) {
        // 파라미터에는 AsyncTask의 Param(첫번째)의 영향을 받는다.
        return null; // 리턴값은 AsyncTask의 Result(세번째)의 영향을 받는다.
    }
    @Override
    protected void onProgressUpdate(Void... values) {
        // 로딩중임을 알리는 창을 띄울 수 있으며 진행상황에 대한 업데이트를 정의한다.
        super.onProgressUpdate(values);
    }
    @Override
    protected void onPostExecute(String s) {
        // 후속작업.
        super.onPostExecute(s);
    }
}
```