## CAP이론

    Consistency (일관성)     Availability (가용성)  Partitions Tolerance (분리 내구성)

    셋 모두 만족시키는 시스템은 구현할 수 없다. (트랜잭션의 일관성과 의미가 다소 다름)


1. Consistency (일관성)
    
      - 트랜잭션의 C
        트랜잭션에서의 ACID 'C'가 아니다! ACID의 'C'는 "데이터는 항상 일관성 있는 상태를 유지해야 하고 데이터의 조작 후에도 무결성을 해치지 말아야 한다"는 속성이다.
    
     - CAP의 ‘C’
      "Single request/response operation sequence"의 속성을 나타낸다. 그 말은 쓰기 동작이 완료된 후 발생하는 읽기 동작은 마지막으로 쓰여진 데이터를 리턴해야 한다는 것을 의미한다.
      모든 노드가 같은 시간에 같은 데이터를 보여줘야 한다. (저장된 데이터까지 모두 같을 필요는 없음)

2. Availability (가용성)

    - "특정 노드가 장애가 나도 서비스가 가능해야 한다"라는 의미를 가진다.
      데이터 저장소에 대한 모든 동작(read, write 등)은 항상 성공적으로 리턴되어야 한다. 

3. Partitions Tolerance (분리 내구성)

    - 노드간에 통신 문제가 생겨서 메시지를 주고받지 못하는 상황이라도 동작해야 한다.
    - Availablity와의 차이점은 Availability는 특정 노드가 “장애”가 발생한 상황에 대한 것이고 Tolerance to network Partitions는 노드의       상태는 정상이지만 네트워크 등의 문제로 서로간의 연결이 끊어진 상황에 대한 것이다


출처: http://hamait.tistory.com/197 [HAMA 블로그]