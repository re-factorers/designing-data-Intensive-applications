# Data locality

## 데이터 지역성이란?

> In Hadoop, Data locality is the process of moving the computation close to where the actual data resides on the node, instead of moving large data to computation. This minimizes network congestion and increases the overall throughput of the system.

Hadoop, Spark 등에서 볼 수 있는 용어.
위 내용처럼, 데이터의 이동을 최대한 줄여 성능을 향상시킨다는 개념이다.

> 지역성의 이점은 한 번에 해당 문서의 많은 부분을 필요로 하는 경우에만 적용됨

## 지역성 관리

- 분산 시스템에서는 큰 파일을 여러 개의 블록으로 나눈 뒤, 각각 다른 서버에 저장해두고, 데이터 처리도 각각 서버에서 동시에 따로 처리하게 하여 성능을 높임
- 관련 데이터를 함께 그룹화
- Searching
- Indexing
- Scheduling

## 예시

- 구글 스패너
  - 테이블의 행을 사전편집 순으로 구성
  - 충분히 가까운 행은 동일한 디스크 블록에 저장되고, 읽기 및 캐시가 함께 수행됨
  - 가용성과 확장을 위해 여러 영역에 걸쳐 데이터를 복제하고, 각 영역에 데이터의 전체 복제본을 저장.
  - 지역성 개선을 위해 동일 테이블에서 관련된 행에 공통 키 접두어를 사용
- 다중 테이블 색인 클러스터 테이블
  - Join에 필요한 Column을 클러스터 키로 생성, 같은 클러스터 내의 Column은 미리 Join 됨
  - Index를 경유하여 클러스터를 찾아감
  - 클러스터링은 검색 속도를 향상시켜주지만, 입력, 수정 삭제 시 추가적인 부하 발생
- 칼럼 패밀리
  - 카산드라 HBase에서 사용하는 개념
  - Row 단위 저장소
  - key/value 데이터 쌍. 하나의 Row에 대량의 Column 허용
  - Column 값이 다시 여러 개의 Column의 Map으로 구성된 것을 Column family 라고 부름
