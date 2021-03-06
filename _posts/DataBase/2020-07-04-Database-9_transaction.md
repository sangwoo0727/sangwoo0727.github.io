---
title: "[데이터베이스] 트랜잭션"
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
## 트랙잭션이란?
이번 포스팅에서는 트랜젝션에 대해 알아보려 한다.

데이터베이스는 다수의 사용자가 동시에 발생하더라도 항상 모순이 없는 정확한 데이터를 유지해야 하며, 장애가 발생하더라도 빠른 시간안에 원래 상태로 복구해야 한다.

데이터베이스 관리 시스템은 데이터베이스가 항상 정확하고 일관된 상태를 유지할 수 있도록 다양한 기능을 제공하는데, 그 중심에 트랜잭션이 있다.

__트랜잭션은 하나의 작업을 수행하는 데 필요한 데이터베이스의 연산들을 모아놓은 것으로, 데이터베이스에서 논리적인 작업의 단위가 된다.__

또한, 장애가 발생하였을 때 데이터를 복구하는 작업의 단위도 된다.

트랜잭션을 설명할 때 항상 예를 드는 내용이 있다. 그 예시를 통해 트랜잭션을 이해해보자.

상우가 대상이에게 5000원을 이체하는 상황이 있다고 해보자.

이 트랜잭션은 2개의 UPDATE 문이 필요하다. 상우의 계좌에서 5000원을 인출하는 UPDATE 문과 대상이의 계좌에 5000원을 입금하는 UPDATE 문이다.

만약 첫 번째 UPDATE 문이 실행된 후 시스템에 장애가 발생하여 두 번째 UPDATE문이 실행되지 않으면, 상우는 대상이에게 5천원을 입금했지만, 대상이는 이 돈을 받지 못하게 된다.

이러한 모순된 상황이 발생하지 않으려면, 두가지 방법이 있다.

시스템이 정상적으로 작동하게 되었을 때, 두번째 UPDATE 문을 실행하여 트랜잭션의 모든 UPDATE 문이 정상적으로 실행되게 하거나,
첫번째 UPDATE 문의 실행을 취소하여 데이터베이스를 트랜잭션 작업 전 상태로 돌아가게 해야한다.

이처럼 트랜잭션의 모든 명령문이 완벽하게 처리되거나 하나도 처리되지 않아야 데이터베이스가 모순이 없는 일관된 상태를 유지할 수 있다. __트랜잭션은 데이터베이스에 장애가 발생하였을 때 복구 작업을 수행하거나, 다수의 사용자가 동시에 사용할 수 있도록 제어작업을 하는데 중요한 단위로 사용된다.__

그러므로, 데이터베이스의 무결성과 일관성을 보장하려면 작업을 수행하는 데 필요한 연산들을 하나의 트랜잭션으로 제대로 정의하고 관리해야 한다.

## 트랜잭션의 특성(ACID 특성)
트랜잭션이 성공적으로 처리되어 데이터베이스의 무결성과 일관성이 보장되려면 __원자성, 일관성, 격리성, 지속성__ 네가지의 특성이 만족해야 하고, 이를 ACID 특성이라고 한다.

__첫 번째로, 원자성(Atomicity)는 트랜잭션을 구성하는 연산들이 모두 정상적으로 실행되거나 하나도 실행되지 않아야 한다는 all-or-nothing 방식을 의미한다.__

위의 계좌 트랜잭션을 예로 든 것 처럼, 트랜잭션을 수행하다가 장애가 발생하여 작업을 완료하지 못했다면, 지금까지 실행한 연산처리를 모두 취소하고 데이터베이스를 트랜잭션 작업 전의 상태로 되돌려 트랜잭션의 원자성을 보장해야 한다.

즉, 트랜잭션의 원자성을 보장하려면, __장애가 발생했을 때 데이터베이스의 원래 상태로 복구하는 회복 기능이 필요하다.__

__두 번째로, 일관성(Consistency)는 트랜잭션이 성공적으로 수행된 후에도 데이터베이스가 일관된 상태를 유지해야 함을 의미한다.__

트랜잭션이 수행되기 전에 데이터베이스가 일관된 상태였다면 트랜잭션의 수행이 완료된 후 결과를 반영한 데이터베이스도 또다른 일관된 상태가 되어야 한다.

__세 번째로, 격리성(Isolation)은 고립성이라고도 하는데, 현재 수행 중인 트랜잭션이 완료될 때까지 트랜잭션이 생성한 중간 연산 결과에 다른 트랜잭션들이 접근할 수 없음을 의미한다.__

데이터베이스 시스템에서는 여러 트랜잭션이 동시에 수행되지만 각 트랜잭션이 독립적으로 수행될 수 있도록 다른 트랜잭션의 중간 연산 결과에 서로 접근하지 못하게 한다.

예를 들어, 상우가 대상이의 계좌에 5천원을 이체하고, 대상이의 계좌에 1000원을 입금하는 트랜잭션이 동시에 수행되는 상황이 있다고 해보자.

트랜잭션이 수행되기전, 상우의 계좌 잔액은 5천원이고, 대상이의 계좌 잔액은 0원이라고 가정한다.

상우의 계좌에서 5천원을 감소시키는 첫번째 연산이 실행된 후, 대상이의 계좌 금액을 5천원 증가시키는 연산이 실행되기 전에 계좌 입금 트랜잭션이 대상이의 계좌에 1000원을 입금하고, 대상이의 계좌 잔액을 여전히 0원으로 알고있는 계좌이체 트랜잭션의 두번째 연산이 실행되면 대상이의 계좌 잔액이 1000원과 5천원이 되는 일관되지 않은 정보가 된다.

이런 이유로, 트랜잭션의 수행 과정에서는 생성되는 중간 연산의 결과에 다른 트랜잭션이 접근할 수 없도록 하여 트랜잭션의 격리성을 보장해야 한다.

__네 번째로, 지속성(Durability)은 영속성이라고도 하는데 트랜잭션이 성공적으로 완료된 후 데이터베이스에 반영한 수행 결과는 어떠한 경우에도 손실되지 않고 영구적이어야 함을 의미한다.__

## 트랜잭션의 특성을 보장하기 위한 기능

![](/assets/img/DataBase/20200704_3.jpeg)

데이터베이스 관리 시스템은 트랜잭션의 네가지 특성을 보장하기 위한 지원 기능을 제공한다.

먼저, 원자성을 보장하기 위해서는, 데이터베이스의 원래 상태로 복구하는 회복기능이 필요하다.

일관성과 격리성을 보장하기 위해서는, 여러개의 트랜잭션이 병행 수행되면서 같은 데이터에 접근하여 연산을 실행하더라도 문제가 발생하지 않게 트랜잭션의 수행을 제어하는 병행 제어 기능이 필요하다.

마지막으로, 지속성을 보장하기 위해서 시스템에 장애가 발생했을 때 데이터베이스를 원래 상태로 복구하는 회복 기능이 필요하다.

각 기능에 대해서는 다음 포스팅에서 자세히 알아보겠다.

## 트랜잭션의 연산
트랜잭션의 연산에는 Commit 연산과 Rollback 연산이 있다.

__commit 연산은 트랜잭션의 수행이 성공적으로 완료되었음을 선언하는 연산이다.__

commit 연산이 실행된 후에야 트랜잭션의 수행 결과가 데이터베이스에 반영되어 데이터베이스가 일관된 상태를 지속적으로 유지하게 된다.

__rollback 연산은 트랜잭션의 수행이 실패했음을 선언하는 연산이다.__

rollback 연산이 실행되면 트랜잭션이 지금까지 실행한 연산의 결과가 취소되고 트랜잭션이 수행되기 전의 상태로 돌아간다.

그렇기 때문에, 트랜잭션 수행 전의 일관된 상태로 되돌려 모순이 발생하지 않게 한다.

## 트랜잭션의 상태
트랜잭션은 __활동, 부분완료, 완료, 실패, 철회__ 다섯가지의 상태 중 하나에 속하게 된다.

트랜잭션이 수행되기 시작하면 활동 상태가 되고, 활동 상태의 트랜잭션이 마지막 연산을 처리하고 나면 부분완료 상태가 되어, 마지막으로 commit연산을 실행하면 완료 상태가 된다.

활동 상태나 부분완료 상태에서 다양한 원인에 의해 트랜잭션은 실패 상태가 될 수 있다. 실패 상태의 트랜잭션은 rollback연산의 실행으로 철회 상태가 된다.

완료 상태나 철회 상태가 되면 트랜잭션이 종료된 것으로 판단한다.

__활동 상태__ 는 트랜잭션이 수행되기 시작하여, 현재 수행중인 상태를 말한다.

__부분 완료 상태__ 는 트랜잭션의 마지막 연산이 실행된 직후의 상태를 부분 완료 상태라 한다. 부분 이라는 단어가 들어갔지만, 트랜잭션의 모든 연산을 처리한 상태이므로 헷갈리지 말자! 모든 연산은 처리가 끝났지만, 트랜잭션이 수행된 최종 결과는 아직 데이터베이스에 반영되지 않은 상태이다.

__완료 상태__ 는 트랜잭션이 성공적으로 완료되어 commit연산을 실행한 상태를 말한다. 데이터베이스는 새로운 일관된 상태가 되며 트랜잭션을 종료한다.

__실패 상태__ 는 다양한 이유로 트랜잭션의 수행이 중단된 상태를 말한다. (하드웨어,소프트웨어 문제, 트랜잭션 내부의 오류 등)

__철회 상태__ 는 트랜잭션을 수행하는 데 실패하여 rollback 연산을 실행한 상태를 말한다. 철회 상태가 되면 지금까지 실행한 트랜잭션의 연산을 모두 취소하고 트랜잭션이 수행되기 전의 데이터베이스 상태로 되돌리면서 트랜잭션이 종료된다.

철회 상태 이후는 두가지로 나뉘는데, 트랜잭션 내부 문제가 아니라 하드웨어나 소프트웨어 오류로 철회 상태가 된 경우에는 철회된 트랜잭션을 다시 시작한다. 반면, 데이터가 존재하지 않거나 트랜잭션의 논리적인 오류가 원인인 경우에는 철회된 트랜잭션을 폐기한다.

## 출처
__데이터베이스 개론 2판, 한빛아카데미__



