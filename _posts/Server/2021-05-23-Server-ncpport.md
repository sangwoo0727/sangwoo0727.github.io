---
title: "NCP 외부 접속 허용하기"
categories:
  - Server
read_time: false
tags:
  - Server
comments:
  - true
toc: true
toc_sticky: true
---
## 개요
* NCP를 사용하면서 필요한 내용들에 대해 정리를 한다.
* 이번 포스팅은 네이버 클라우드 플랫폼에서 서버를 생성하고, 외부 접속이 가능하도록 만들어주는 부분에 대해 포스팅을 한다.
* 포트포워딩을 통해 ssh 접속(22번 포트)는 허용하였지만, 로컬에서 프로젝트를 개발하며 서버에 만들어둔 데이터베이스에 접속을 하기 위해서 3306 포트도 허용이 필요했다.(프로젝트에서 ssh 터널링을 하지 않고)
* workbench에서는 tcp/ip over ssh 방법으로 서버에 ssh 접속을 하고, 이 후 3306포트를 통해 db에 접속하는 것이 가능했지만, 로컬에서 프로젝트를 개발하며 데이터베이스에 접속을 허용하기 위해서 필요했던 내용을 정리한다.

## 1. 공인 IP 신청
* 나는 무료로 micro server를 생성했다. 데이터베이스 역시 직접 구축을 해보고 싶어서 데이터베이스가 딸린 서버를 생성하지 않고, mariadb를 설치했다.
* 외부에서 서버로 접속하기 위해서는 공인 IP가 필요했다. 
* ncp 콘솔창의 아래 그림과 같이 Public IP에서 공인 IP 신청을 했다.
* 데이터베이스가 잡고있는 3306 포트에 접속하기 위해서는 publicip:3306으로 접속을 할 수 있다.

![](/assets/img/Server/20210523_server_1.png)

## 2. ACG 설정
* 단순히 공인 IP와 포트번호만 있다고 해서 외부에서 접속을 할 수 있는 것은 아니다.
* NCP에서는 ACG라는 기능을 이용해서 외부에서 접근가능한 IP와 port를 선택할 수 있다.

![](/assets/img/Server/20210523_server_2.png)

* 현재 테스트를 위해 접근 허용을 0.0.0.0/0(전체) 전체 ip에 허용해주었지만, 필요한 ip에 한하여 등록해주는 것이 좋다.
* 3306 포트까지 입력을 해줌으로써, 외부에서 해당 포트를 통해 접속할 수 있도록 만들어주었다.

## 3. DB 외부 허용 설정
* NCP에서 제공해주는 데이터베이스를 사용할 경우 수정할 필요가 없지만, 직접 설치하여 구축하는 과정이기 때문에 설정파일에서 외부접속을 위한 설정을 수정해주어야 한다.
* 나는 MariaDB를 사용하고 있기 때문에 아래와 같이 설정 파일로 들어갔다.

```
/etc/mysql/mariadb.conf.d/50-server.cnf

# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 0.0.0.0 # 127.0.0.1을 다음과 같이 변경
```
* 이후, 외부에서 접속할 수 있도록 데이터베이스 계정도 권한을 주어야 한다.

```sql
create user 계정@localhost identified by '패스워드';
create user 계정@'%' identified by '패스워드';
grant all privileges on whistleon.* to '계정'@localhost;
grant all privileges on whistleon.* to '계정'@'%';
flush privileges;
```

## 4. 리눅스 방화벽 설정
* 리눅스의 방화벽 설정을 통해 Mariadb 포트를 열어주어야 한다.
* 나는 Ubuntu를 사용하고 있기 때문에 다음과 같은 명령어로 진행하였다.

```
$ sudo apt update && sudo apt install firewalld -y
$ sudo firewall-cmd --version
0.4.4.5
$ sudo firewall-cmd --permanent --zone=public --add-port=3306/tcp
success
$ sudo firewall-cmd --reload
success
```

* 해당 공인 IP와 포트를 이용해 접속을 해보면 잘 되는 것을 확인할 수 있다.


