---
title: "[MySQL] 계정 생성 및 권한 부여"
categories:
  - Database
tags:
  - MySQL
comments:
  - true
---
## [MySQL] 계정 생성 및 권한 부여

* 이번 포스팅은 MySQL 접속 계정 생성 및 삭제 그리고 계정 별 권한 설정 방법에 대한 글이다.

### 루트 계정을 통하여 접속

![](/assets/img/mysql/08271.png)

* 먼저 mysql monitor를 통해 MySQL에 mysql -uroot -p 명령어를 통해 접속한다.

### 스키마 변경 및 테이블 확인

![](/assets/img/mysql/08272.png)

* 계정 정보를 보기 위해 스키마를 mysql로 변경한 후, select 명령문을 통해 현재 데이터베이스에 있는 계정정보를 확인한다.
* host가 localhost로 되어있는 계정은 외부에서 접속할 수 없는 계정이다.
* host가 %로 되어있는 계정은 어떤 ip에서든 접속할 수 있는 계정이다.

### 계정 생성

![](/assets/img/mysql/08273.png)

* 먼저 create user '계정아이디'@localhost identified by '비밀번호'; 명령문을 통해 localhost에서 접속할 수 있는 계정을 만든다.

![](/assets/img/mysql/08274.png)

* localhost에서 접속할 수 있는 계정이 만들어진 것을 볼 수 있다.

![](/assets/img/mysql/08275.png)

* 마찬가지로 모든 외부 ip에서 접속할 수 있는 계정을 똑같은 명령문을 통해 만들 수 있다. 이 경우 localhost 대신 %를 입력하면 된다.

### 권한 부여

* grant all privileges on 스키마명.테이블명 to '계정명'@'호스트'; 명령을 통해 계정에 선택한 스키마와 테이블에 모든 권한을 줄 수 있다.

![](/assets/img/mysql/08276.png)

### 권한 삭제

* revoke all on 스키마명.테이블명 from '계정명'@'호스트'; 명령을 통해 계정에 부여했던 권한을 취소할 수 있다.

![](/assets/img/mysql/08277.png)

### 계정 삭제

* drop user 계정명@호스트; 를 통해 생성했던 계정을 삭제할 수 있다.

![](/assets/img/mysql/08278.png)
