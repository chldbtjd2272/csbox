##LOCK

- LOCK이란
    
    1. 같은 자원을 액세스하려는 다중 트랜잭션 환경에서 데이터베이스의 일관성과 무결성을 유지하려면 트랜잭션의 순차적 진행을 보장할 수 있는 직렬화(serialization) 장치가 필요하다. 영화관 좌석을 예약하는 시스템을 예로 들면, 두 명이 동시에 좌석을 요청할 때 정확히 한 명만 좌석을 배정받도록 할 수 있어야 한다. 이런 직렬화가 가능하도록 하기 위해 모든 DBMS가 공통적으로 사용하는 메커니즘이 Lock이다. 
    

-  공유 Lock과 배타적 Lock

    기본적으로 DBMS는 트랜잭션의 오퍼레이션별로 적당한 수준의 Lock을 자동으로 설정(유저가 설정 가능)
    
    가장 기본적인 Lock은 공유Lock과 배타적Lock

    1. 공유 Lcok
    
    - 공유(Shared) Lock은 데이터를 읽고자 할 때 사용된다.
    
    - 공유 Lock을 설정한 리소스에 다른 트랜잭션이 추가로 공유 Lock을 설정할 수는 있지만 배타적 Lock은 불가능하다.
      따라서 자신이 읽고 있는 리소스를 다른 사용자가 동시에 읽을 수는 있어도 변경은 불가능하다.


    2. 배타적 Lock
    
    - 배타적(Exclusive) Lock은 데이터를 변경하고자 할 때 사용되며, 트랜잭션이 완료될 때까지 유지된다.
    
    - 말 그대로 배타적이기 때문에 그 Lock이 해제될 때까지 다른 트랜잭션은 해당 리소스에 접근할 수 없다.
    
    - 변경이 불가능할 뿐만 아니라 읽기도 불가능하다.

    - 공유 Lock이든 배타적 Lock이든, 배타적 Lock을 동시에 설정할 수 없다.




- 블로킹과 교착상태

    1. 블로킹
    
    - 블로킹(Blocking)은, Lock 경합이 발생해 특정 세션이 작업을 진행하지 못하고 멈춰 선 상태를 말한다. 공유 Lock끼리는 호환되기 때문에 블로킹이         발생하지 않는다.

    - 공유 Lock과 배타적 Lock, 배타적 Lock과 배타적 Lock 서로 호환되지 않는 것끼리 블로킹이 발생할 수 있다.

    - Lock 경합이 발생하면 먼저 Lock을 설정한 트랜잭션이 완료될 때까지 후행 트랜잭션은 기다려야 하며, 이런 현상이 자주 나타난다면 사용자가 느끼는          애플리케이션 성능이 좋지 않다.

        1.) 해결책

         - 우선, 트랜잭션의 원자성을 훼손하지 않는 선에서 트랜잭션을 가능한 짧게 정의하려는 노력이 필요하다. Oracle은 데이터를 읽을 때 공유 Lock을       사용하지 않기 때문에 다른 DBMS에 비해 상대적으로 Lock 경합이 적게 발생한다. 그렇더라도 배타적 Lock끼리 발생하는 경합은 피하지 못하므로        '불필요하게' 트랜잭션을 길게 정의해선 안 된다.


         - 같은 데이터를 갱신하는 트랜잭션이 동시에 수행되지 않도록 설계하는 것도 중요하다. 특히, 트랜잭션이 활발한 주간에 대용량 갱신 작업을 수행해선 안     된다.

         - 주간에 대용량 갱신 작업이 불가피하다면, 블로킹 현상에 의해 사용자가 무한정 기다리지 않도록 적절한 프로그래밍 기법을 도입해야 한다. 예를 들어,     SQL Server에서는 세션 레벨에서 LOCK_TIMEOUT을 설정할 수 있다. 아래는 Lock에 의한 대기 시간이 최대 2초를 넘지 않도록 설정한 것이다.

            set lock_timeout 2000
            Oracle이라면 update/delete 문장을 수행하기 전에 nowait이나 wait 옵션을 지정한 select … for update 문을 먼저 수행해 봄으로써 Lock이 설정됐는지 체크할 수 있고, 발생한 예외사항(exception)에 따라 적절한 조치를 취할 수 있다.

            select * from t where no = 1 for update nowait → 대기없이 Exception을 던짐 select * from t where no = 1 for update wait 3 → 3초 대기 후 Exception을 던짐

         - 트랜잭션 격리성 수준을 불필요하게 상향 조정하지 않는다.

         - 트랜잭션을 잘 설계하고 대기 현상을 피하는 프로그래밍 기법을 적용하기에 앞서, SQL 문장이 가장 빠른 시간 내에 처리를 완료하도록 하는 것이 Lock     튜닝의 기본이고 효과도 가장 확실하다.

    2. 교착상태

    -  두 세션이 각각 Lock을 설정한 리소스를 서로 액세스하려고 마주보며 진행하는 상황을 말하며, 둘 중 하나가 뒤로 물러나지 않으면 영영 풀릴 수 없다. 흔히     좁은 골목길에 두 대의 차량이 마주 선 것에 비유하곤 한다.

    -  블로킹에서의 Lock 튜닝 방안은 교착상태 발생 가능성을 줄이는 방안이기도 하다. 여러 테이블을 액세스하면서 발생하는 교착상태는 테이블 접근 순서를 같게     처리하면 피할 수 있다. 
