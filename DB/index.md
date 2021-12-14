# Index (인덱스)

## 목차
1. [Index 란 ?](#Index-란-?)
2. [Index 은 어떻게 동작하는가 ?](#Index-은-어떻게-동작하는가-?)
3. [Index 의 장점은 ?](#Index-의-장점은-?)
4. [Index 의 단점은 ?](#Index-의-단점은-?)
5. [Index 를 왜 사용하나요 ?](#Index-를-왜-사용하나요-?)
6. [Index 를 생성 전략은 ?](#Index-를-생성-전략은-?)
7. [Index 형태는 ?](#Index-형태는-?)

---
## Index 란 ?
**Index 는 추가 적인 쓰기 작업과 저장 공간을 활용하여 데이터 베이스 테이블의 검색 속도를 향상시키기 위한 자료 구조** 이다.
예로는 책의 목차와 같다고 생각하면 됩니다.

데이터베이스 안의 레코드를 처음부터 풀스캔하지 않고, B+Tree 로 구성된 구조에서 Index 파일 검색으로 속도를 향상 시키는 기술입니다.
<br>
<br>
## Index 은 어떻게 동작하는가 ?
DB의 테이블은 생성시 MYD, MYI, FRM 3개의 파일이 생성 됩니다.
**Index 는 MYI 파일에 선택 컬럼을 색인화 하여 저장**하게 됩니다.
사용자가 만약 Select 쿼리로 Index 가 사용하는 쿼리 사용시 Table 검색이 아닌 Tree 로 정리된 MYI 파일의 내용을 검색합니다.
<br>
<br>
## Index 의 장점은 ?
- Key 값을 기초로 Table 에서 검색과 정렬 속도롤 향상 시킵니다.
- 그룹화 작업의 속도를 향상 시킵니다.
- 인덱스를 사용하면 테이블 행의 고유성이 강화 됩니다.
- 테이블의 P-Key 는 자동으로 Index 됩니다. (Clustered Index)
- 필드 중 Data Type 으로 인해 Index 될 수 없는 필드가 존재합니다.
<br>
<br>
## Index 의 단점은 ?

- Index 된 필드에서 데이터를 업데이트 하거나, 레코드를 추가 또는 삭제 할 때 성능이 떨어집니다.
- Index 가 데이터베이스의 공간을 차지하기 때문에 추가 공간이 필요하게 됩니다. (통상 DB의 10% 내외의 공간 추가로 필요)
- Index 를 생성하는데 시간이 많이 소요될 수 있다.
- 데이터 변경 작업이 자주 일어날 경우에 Index 를 재 작성해야할 필요가 있기 때문에 성능에 영향을 끼칠수 있습니다.
<br>
<br>
## Index 를 왜 사용하나요 ?
**Index 를 사용하는 목적은 RDBMS 의 검색 속도를 향상**시키는데 있습니다.
Select 쿼리의 Where 절이나 Join 예약어를 사용하면 Index 가 사용되며, Select 쿼리의 검색 속도를 빠르게 할수 있습니다.
<br>
<br>
## Index 를 생성 전략은 ?
1. Where 절에서 사용되는 컬럼에 Index 를 사용하면 검색 속도를 향상 시킬 수 있습니다.
2. 중복되는 데이터가 최소한인 컬럼에 Index 를 걸어주는게 좋습니다. (데이터 중복이 많을 수록 Index 효용이 낮다.)
3. 외래 키가 사용되는 열은 Index 를 생성하는 것이 좋습니다.
4. Join 에 자주 사용되는 열에는 Index 를 생성하는 것이 좋습니다.
5. Insert, Update, Delete 가 빈번하게 일어나는 경우 Index 생성은 하지않는 것이 좋습니다.
6. 사용하지 않는 Index 는 삭제해줍니다.
<br>
<br>
## Index 형태는 ?
### Clustered Index
Clustered 란 군집화라는 뜻을 가지고 있으며, Clustered Index 는 군집화된 인덱스입니다.
Clustered Index 는 데이터가 Table 에 물리적으로 저장 되는 순서를 정의합니다. <br>
기본적으로 Clustered Index 는 Table 에 있는 데이터를 정렬하기 때문에 Insert 와 같은 비용이 많이 들게 됩니다.
이유는 만약, ID의 값이 1, 3, 4가 Table 에 들어있는데 ID 2를 추가하게 되면 3, 4의 저장 위치를 한칸씩 밀고 사이에 2를 끼워넣기 때문이다.
보통 DB의 Primary Key 를 설정하게 되면 Clustered Index 가 자동으로 생성 되게 된다. 또한, 한 Table 에 하나만 존재합니다.
### Non-Clustered Index
Clustered Index 의 반대로 비 군집화이며, Non-Clustered Index 는 별도의 저장 공간에 데이터를 저장합니다.
Index 는 데이터의 행에 독립적이고, 이 Index Key 값과 데이터 행을 가리키는 포인트가 존재합니다.
데이터가 저장된 형태도 정렬 되지 않은 형태로 데이터를 저장합니다. 또한, 한 테이블에 여러개의 Index 생성이 가능합니다.
<br>
<br>
## Non-Clustered Index 는 DB Insert 시 유리할까?
결론 부터 말하면 유리한것은 아니다. Clustered Index 의 경우 중간에 데이터를 삽입 할때 정렬 상태를 유지해야함으로
데이터를 정령을 계속 하면서 Insert 시 많은 작업을 요하는것 같지만, Non-Clustered Index 역시 Insert 가 발생할 때 마다
별도의 Table 에 데이터의 행을 가리키는 포인터를 Key Value 의 형태로 데이터를 계속 넣어야 하니 Insert 시 추가 작업은 필수인 것입니다.