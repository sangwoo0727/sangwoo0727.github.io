---
title: "[데이터베이스] 논리적 데이터 모델 - 관계 데이터 모델의 제약"
categories:
  - DataBase
tags:
  - DataBase
comments:
  - true
read_time: false
toc: true
toc_sticky: true
---
이전 포스팅과 이어서 계속해서 우리는 논리적 데이터모델에서 가장 많이 사용하는 관계 데이터 모델에 대해 다루고 있다.

관계 데이터 모델에는 어떠한 기본적인 제약 사항 등이 있는데 이번 포스팅에서는 그것에 대해 다뤄보려 한다.

바로 시작하자!

## 관계 데이터 모델의 제약
관계 데이터 모델에서 정의하고 있는 기본 제약 사항은 키와 관련한 무결성 제약조건이다.

무결성은 데이터에 결함이 없는 상태, 즉 데이터가 정확하고 유효하게 유지된 상태를 말한다.

무결성 제약 조건의 목적은 데이터베이스에 저장된 데이터의 무결성을 보장하고, 데이터베이스의 상태를 일관되게 유지하는 것이다.

데이터베이스 내부의 데이터를 보호한다는 점에서 무결성은 보안과 유사하다.

하지만, 보안이 권한이 없는 사용자로부터 데이터를 보호하는 것으로 표현한다면, 무결성은 권한이 있는 사용자의 잘못된 요구에 의해 데이터가 부정확해지지 않도록 보호하는 것이라고 할 수 있다.

데이터베이스가 C,U,D 작업으로 상태가 변하더라도 무결성 제약조건은 반드시 지켜져야 한다.

이러한 무결성 제약 조건에는 __개체 무결성 제약 조건__ 과 __참조 무결성 제약조건__ 이 있다.

데이터베이스의 상태를 일관성 있게 유지하기 위해서는 위의 두 제약 조건을 만족시켜야 한다.

## 개체 무결성 제약 조건(Entity Integrity Constraint)
개체 무결성 제약조건은 __기본키를 구성하는 모든 속성은 널 값을 가지면 안된다는 규칙이다.__

기본키를 구성하는 속성 전체나 일부가 널 값이 되면 튜플의 유일성을 판단할 수 없어 기본키의 목적을 상실하게 된다.

만약 기본키의 속성 중 널 값이 있다면, 기본키를 가지고 서로 다른 튜플인지 구분할 수 없다. 즉 하나의 릴레이션에는 동일한 튜플이 존재할 수 없다는 릴레이션의 고유의 특성을 만족시킨다고 장담할 수 없으므로, 개체 무결성 제약조건을 위반하게 된다.

이를 해결하기 위해서는 새로운 튜플이 삽입되는 연산과 기존 튜플의 기본키 속성 값이 변경되는 연산이 발생할 때 기본키에 널 값이 포함되는 상황에서는 연산의 수행을 거부하면 된다. 사용자가 하는 것이 아니라 데이터베이스 관리 시스템이 수행하므로 우리는 새로운 릴레이션을 생성할 때, 기본키를 어떤 속성들로 구성할 것인지만 전달해주면 된다.

## 참조 무결성 제약 조건(Referential Integrity Constraint)
참조 무결성 제약조건은 외래키에 대한 규칙으로 연관된 릴레이션들에 적용된다.

참조 무결성 제약조건이란 __외래키는 참조할 수 없는 값을 가질 수 없다는 규칙이다.__ 외래키는 다른 릴레이션의 기본키를 참조하는 속성이고, 릴레이션 간의 관계를 표현하는 역할을 한다. 만약 외래키가 자신이 참조하는 릴레이션의 기본키와 상관이 없는 값을 가지게 되면 두 릴레이션을 연관시킬 수 없게 된다.

즉, 외래키는 참조 가능한 값만 가져야 한다는 것이다.

반면 외래키가 널값인 경우는 어떨까? 팀정보가 담긴 릴레이션이 있고, 사용자정보가 있는 릴레이션이 있다고 해보자. 사용자는 팀 릴레이션을 참조하며, 사용자 릴레이션의 팀이름 속성은 외래키로 팀 정보 릴레이션을 참조하게 된다. 이때 사용자정보 릴레이션의 팀이름 속성이 널인 것은 존재하지 않는 팀이름을 참조했다고 볼 수 없다.

그러므로 참조 무결성 제약조건을 만족시키려면 외래키가 참조 가능한 값만 가져야하지만, 널 값을 가진다고 해서 참조 무결성 제약 조건을 위반한 것으로 보기 어렵다.

데이터 베이스의 상태가 변해도 참조 무결성 제약조건은 만족시켜야 한다.

팀정보가 담긴 릴레이션에 새로운 팀 튜플을 삽입하는 것은 참조 무결성 제약조건을 위반하는 작업이 아니다. 다만 개체 무결성 제약조건을 위반하지 않도록 팀 이름 속성 값을 삽입해야 한다. (기본키는 팀이름으로 했다고 가정)

그리고 사용자 릴레이션에 새로운 사용자 튜플을 삽입할 때는 참조 무결성 제약조건을 위반하지 않는지 확인해야 한다.

즉, 팀 릴레이션의 기본키인 팀이름 속성 값으로 존재하지 않는 값은 사용자 릴레이션의 팀이름 속성으로 지정하지 않아야 한다.

이러한 연산을 수행하면 수행을 거부당한다.

팀 릴레이션에 존재하는 튜플을 삭제하는 연산 역시 참조 무결성 제약조건을 위반하지 않는 경우에 수행해야 한다. 즉, 아슬란이라는 팀이름을 가지고 있는 사용자 릴레이션의 강상우 사용자가 있다고 할 때, 팀 릴레이션에서 아슬란 튜플을 삭제하면 사용자 릴레이션의 강상우 튜플의 팀은 존재하지 않는 팀에 소속되어 있는 것으로 부정확한 데이터가 된다.

반면 사용자 릴레이션에서 튜플을 삭제하는 것은 어떠한 제약조건도 위반하지 않는다.

추가적으로, 팀 릴레이션에서 기본키인 팀이름 속성 값을 변경하려 할 때, 사용자 릴레이션의 외래키인 팀 이름 속성 값에 연관된 값이 존재한다면 변경 연산을 수행하지 않거나 사용자 릴레이션에 외래키인 팀이름 속성 값 역시 새로운 값으로 변경해야 참조 무결성 제약조건을 만족시키게 된다.

사용자 릴레이션의 외래키인 팀이름 속성 값을 변경하려 할때도 반드시 존재하는 팀 릴레이션의 기본키인 팀이름 속성값 중 하나로 변경하고 이외에는 허용하지 않아야 한다.

이러한 작업들은 데이터베이스 관리 시스템이 자동으로 수행하므로 사용자는 새로운 릴레이션을 생성할 때마다 어떤 속성들이 외래키이고 어떤 릴레이션의 기본키를 참조하면 되는지 데이터베이스 관리 시스템에게 알려주기만 하면 된다.

## 출처
__데이터베이스 개론 2판, 한빛아카데미__



