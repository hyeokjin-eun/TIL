# Transaction (트랜젝션)

## 목차
1. [Transaction 이란 ?](#Transaction-이란-?)
2. [Transaction 의 특징은 ?](#Transaction-의-특징은-?)
3. [Transaction 의 상태 ?](#Transaction-의-상태-?)
4. [Transaction Isolation Level 이란 ?](#Transaction-Isolation-Level-이란-?)
5. [Transaction Isolation Level 의 종류는 ?](#Transaction-Isolation-Level-의-종류는-?)
6. [CRUD 사용시 Transaction Isolation Level 을 어떻게 설정해야 할까 ?](#CRUD-사용시-Transaction-Isolation-Level-을-어떻게-설정해야-할까-?)
7. [Transaction Isolation Level 결론은 ?](#Transaction-Isolation-Level-결론은-?)

## Transaction 이란 ?
**데이터베이스의 상태를 변경시키기 위해 실행하는 작업의 단위** 입니다.
<br>

질의어를 통해 데이터베이스에 접근하는 것을 의미합니다.

## Transaction 의 특징은 ?
**Transaction 은 아래 4가지 특징**을 가집니다. 일반적으로 이 특징을 ACID 라고 부릅니다.
* 원자성(Atomicity)
* 일관성(Consistency)
* 독립성(Isolation)
* 지속성(Durability)

### 원자성이란 ?
원자성은 Transaction 이 데이터베이스에 모두 반영되거나, 모두 반영되지 않아야 한다는 뜻입니다.

### 일관성이란 ?
일관성은 Transaction 의 작업 처리 결과는 항상 일관성이 있어야 합니다.

### 독립성이란 ?
독립성은 둘 이상의 Transaction 이 동시에 병행되고 있을 때, Transaction 끼리 서로 관련이 없어야 합니다.

### 지속성이란 ?
지속성은 Transaction 이 성공적으로 완료가 되면, 영구 반영이 되어야하는걸 의미합니다.

Transaction 은 위 4가지 특징을 만족하기 위해 Commit, Rollback 연산을 통해 위 특징을 만족시킵니다.

### Commit ?
Commit 은 하나의 Transaction 에 대한 작업이 성공적으로 종료되고, 데이터베이스가 일관성있는 상태일 경우 Transaction 관리자에게 연산이 완료 되었음을 알려줍니다. 

### Rollback ?
Rollback 은 하나의 Transaction 처리가 비정상 종료되어 일관성을 깨뜨릴때, 원자성에 의해 모든 Transaction 내 작업을 취소하는 연산입니다.

## Transaction 의 상태 ?
**Transaction 의 상태는 5가지의 상태**가 있습니다.
* Active
* Filed
* Aborted
* Partially Committed
* Committed

### Active ? 
Active 는 Transaction 이 정상적으로 실행중인 상태를 의미합니다.

### Failed ?
Filed 는 Transaction 실행에 오류가 발생하여 중단된 상태를 의미합니다.

### Aborted ?
Aborted 는 Transaction 이 비정상적으로 종료되어 Rollback 연상을 수행한 상태를 의미합니다.

### Partially Committed
Partially Committed 는 Transaction 의 마지막 연산까지 실행 했지만, Commit 연산이 실행되기 직전의 상태를 의미합니다.

### Committed
Committed 는 Transaction 이 성공적으로 종료되어 Commit 연산을 실행한 후의 상태입니다.

## Transaction Isolation Level 이란 ?
Transaction 은 ACID 의 특징을 가진다고 앞서 설명했을 것이다. 하지만 특징을 너무 타이트 하게 지키게 되면 Transaction 처리시 동시성에 대한 효율이 떨어지게 됩니다.
<br>

동시성 효율을 높이기 위해 Transaction 은 Isolation Level 을 두어 동시성에 대한 효율을 높입니다.

## Transaction Isolation Level 의 종류는 ?
**Transaction Isolation Level 은 4가지 레벨이 존재**합니다.
* READ UNCOMMITTED
* READ COMMITTED
* REPEATABLE READ
* SERIALIZABLE

### READ UNCOMMITTED ?
Select 쿼리 실행시 다른 Transaction 에서 Commit 되지 않은 데이터를 읽을 수 있습니다.
Commit 되지 않는 데이터를 읽는 현상을 Dirty Read 라고 말하며, 한번도 Commit 되지 않는 데이터를 읽을수 있어 유의 해야 합니다.

### READ COMMITTED ?
Commit 이 완료된 데이터만 Select 시 에 보이는 수준을 보장하는 Level 입니다.<br>
대부분의 DBMS 는 READ COMMITTED Level 을 기본으로 설정합니다.
READ COMMITTED 에서는 Dirty Read 와 같은 현상은 일어나지 않습니다. 하지만 Transaction 처리시 Update 나 레코드에 변경이 생겼을때 테이블에 바로 반영이 된 상태가 되는데,
Select 시 Update 전의 데이터를 가져올 수 있는 것은 Update 시 Undo 영역에 백업된 레코드의 값을 가져오기 때문입니다.
이러한 경우 Transaction-1 에서 업데이트 완료후 Commit 여부에 따라 Transaction-2 에서 2개 이상의 Select 쿼리의 값이 달라질수 있다는 단점이 있습니다.

### REPEATABLE READ ?
READ COMMITTED 와는 다르게 REPEATABLE READ 는 한 Transaction 안에서 반복적으로 Select 를 수행하더라도 읽어 들이는 값이 변화하지 않음을 보장합니다.
REPEATABLE READ Transaction 은 처음으로 Select 을 수행한 시간을 기록한 뒤 그 이후에 모든 Select 마다 해당 시점을 기준으로
Consistent Read 를 수행하여 줍니다.
그러므로 Transaction 도중 다른 Transaction 이 Commit 되더라도 새로이 Commit 된 데이터는 보이지 않습니다. 이러한 점이 가능한 이유는 첫 Select 시에 생성된 Snapshot 을 읽기 때문이다.

### SERIALIZABLE ?
SERIALIZABLE 은 모든 작업을 하나의 트랜젝션에서 처리하는 것과 같은 가장 높은 고립 수준을 제공합니다.
READ COMMITTED, REPEATABLE READ 두개의 공통적 이슈는 하나의 Transaction 에서 Update 명령이 유실되거나 덮어 씌워지는 Phantom Read 가 발생할 수 있지만,
SERIALIZABLE 에서는 Select 쿼리가 모두 공유 상태로 자동 변경되어 락상태로 동작하게 된다.
하지만 Select 쿼리에 락이 걸리기 때문에 DEADLOCK 에 유의하여 사용해야한다.

## CRUD 사용시 Transaction Isolation Level 을 어떻게 설정해야 할까 ?
Read 를 사용할 시에는 변경된 데이터를 읽어오는것을 방지하기 위해서 Repeatable Read 를 사용하는 것이 적당해 보인다.

Create, Update, Delete 의 경우 데이터의 유실이나 덮어 씌워지는 현상인 Phantom Read 현상을 방지하기 위해 Serializable Read 사용하는것이 적당해 보인다. 

## Transaction Isolation Level 결론은 ?
| Level             | Dirty Read | Non-Repeatable Read | Phantom Read | 
|-------------------|------------|---------------------|--------------|
| Read Uncommitted  |Y           |Y                    |Y             |
| Read Committed    |N           |Y                    |Y             |
| Repeatable Read   |N           |N                    |Y             |
| Serializable Read |N           |N                    |N             |