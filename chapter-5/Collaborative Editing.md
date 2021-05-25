# Collaborative Editing

- 실시간 동시 편집 기술
- Operation들간의 causality(인과성)/concurrency(동시성) 문제를 어떻게 해결할 것인가

## OT (Operational Transformation)

- 모든 변경에 대한 시간순 목록을 저장
- Google Docs, MS Office 에서 사용
- 모든 변경사항 기록 --> 서버에 전송 --> 순차적 실행 --> 클라이언트에 반환
- 중앙 집중식 서버/DB 의존하기 때문에 사용자가 몰릴 때 과부하 발생
- https://en.wikipedia.org/wiki/Operational_transformation

## CRDT (Conflict-free Replicated Data Type)

- real-time collaboration을 위한 새로운 종류의 분산 데이터 구조
- Integer index 대신 unique ID 사용
- No history buffer
- Conclict를 해결할 수 있는 meta data를 object와 함께 저장
- Vector clock free
- Figma, Redis, Yorkie, Wave 에서 사용
- 순서와 상관 없이 변경사항만 같으면 동일한 상태로 간주
- 중앙 DB 없이 실시간 편집 가능
- 문제로 제기되었던 속도, 용량, 기능, 복잡도 모두 개선되고 있음
- https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type