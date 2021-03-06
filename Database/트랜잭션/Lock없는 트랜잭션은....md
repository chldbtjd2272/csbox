## Lock 없이 트랜잭션을 동시에 실행한다면


1. Lost Update Problem
  
  - 두 개의 트랜잭션이 동시에 한 아이템의 데이터를 변경했을 때 발생하는 문제점

  - 트랜잭션을 지원하는 데이터베이스에서는 발생하면 안 됨



2. (P1) Dirty Read Problem

  - 한 트랜잭션에서 변경한 값을 다른 트랜잭션에서 읽을 때 발생하는 문제
  
  - 트랜잭션에서 처리중인 / 아직 커밋되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용
  
  - 오라클은 이 레벨을 지원하지 않음




3. (P2) Non-repeatable Read Problem

  - 한 트랜잭션에서 같은 값을 두 번 읽었을 때 각각 다른 값이 읽히는 경우
  
  - 대부분의 DBMS가 기본모드로 채택하고 있는 일관성 모드

  - DB2, SQL Server, Sybase의 경우 읽기 공유 Lock을 이용해서 구현, 하나의 레코드를 읽을 때 Lock을 설정하고 해당 레코드에서 빠지는 순간 Lock해제

  - Oracle은 Lock을 사용하지 않고 쿼리시작 시점의 Undo 데이터를 제공하는 방식으로 구현



4. (P3) Phantom Read Problem

  - 선행 트랜잭션이 읽은 데이터는 트랜잭션이 종료될 때까지 후행 트랜잭션이 갱신하거나 삭제하는 것을 불허함으로써 같은 데이터를 두번 쿼리했을 때 일관성 있는      결과를 리턴

  - Phantom Read 현상 발생

  - DB2, SQL Server의 경우 트랜잭션 고립화 수준을 Repeatable Read로 변경하면 읽은 데이터에 걸린 공유 Lock을 커밋할 때까지 유지하는 방식으로 구현

  - Oracle은 이 레벨을 명시적으로 지원하지 않지만 for update절을 이용해 구현 가능,
    SQL Server등에서도 for update 절을 사용할 수 있지만 커서를 명시적으로 선언할 때만 사용 가능함
    
    
    
    
    
    ![isolation level에 따른 오류발생](https://github.com/chldbtjd2272/csbox/blob/master/Database/images/Lock%EC%97%86%EB%8A%94.png)
