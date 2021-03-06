# 데이터 웨어하우스

## DW; Data Warehouse

기업의 정보분석 요구를 충족시키기 위해 분석, 가공된 데이터를 저장 및 관리하는 기술

## DW의 특징

### 주제 중심적(Subject Oriented)

- 분석하고자 하는 주제를 중심으로 데이터를 구성
- 특정 업무 기능이나 응용 프로그램에 종속되지 않는 데이터 구조를 지원

### 통합 구조(Integrated)

- 업무 기능별로 관리되는 다수의 운영 데이터를 통합하여 전사적 관점에서 중복을 최소화
- 데이터의 정합성과 물리적 통일성을 갖는 통합된 데이터 구조를 지원
- 전사적인 데이터 표준화를 통해 데이터 통일성(속성 이름, 데이터 표현, 계산 단위 등) 확보
- 데이터 획득 시 데이터 통합을 위한 일련의 변환 작업을 수행

### 시계열 데이터(Time Variant)

- 오랜 기간 축적된 데이터를 통해 과거와 현재의 경향 분석 가능
- 일정 기간 동안의 업무 변화 내지는 발전의 추세 분석에 필요
- 이력 데이터를 통해 시간 경과에 따른 데이터의 변화 과정 파악
- 스냅샷 생성
    - 키 구조에 시간 요소를 추가하여 레코드 생성
    - 이벤트 발생 시점의 일자 또는 시간 저장

### 비휘발성(Non Volatile)

- 초기 데이터 적재 이후에는 데이터의 갱신·삭제 없이 검색·조회만 수행
- 데이터 변경이 발생하더라도 변경을 직접 반영하지 않고 스냅샷 형태로 반영
- 장애 발생 시 데이터의 복구, 트랜잭션과 데이터의 무결성 유지, 교착상태의 탐지·대응이 매우 단순
- 데이터 갱신 이상에 대한 고려가 불필요하고, 정규화 및 반정규화에 대한 융통성의 증가

### 기반 기술

데이터 웨어하우스를 구현하기 위한 기술

- ETL: DW에 저장할 데이터를 추출, 전송, 저장하는 엔진
- ODS: 데이터가 DW에 저장되기 전에 가공을 위해 임시로 저장되는 저장소
- CEP: 복잡한 실시간 데이터로부터 필요한 데이터를 추출하기 위한 프로세싱 기술
- CDC: 변경된 데이터를 캡쳐해 타겟 시스템으로 전송하는 기술

### 활용 기술

데이터 웨어하우스를 사용하는 기술

- OLAP: DW를 기반으로 데이터를 분석하고 활용하기 위한 프로세싱
- 비즈니스 인텔리전스: 기업 내 데이터를 취합 및 분석하여 인사이트 도출
- 데이터 마트: 주로 DW를 기반으로 DM이 만들어짐. 구현 형태에 따라 반대로 될 수도 있음

## DM; Data Mart

전사적으로 구축된 데이터 웨어하우스로부터 특정 주제, 부서 중심으로 구축된 소규모 단일 주제의 데이터 레파지토리

- 특정 부서의 의사 결정 지원을 목적으로 하는 부서별 또는 부분별 데이터 웨어하우스
- 일반적으로 한 기업 내에 복수개의 데이터 마트가 존재
    - 부서별로 구축
    - 업무 기능별로 구축
- 전사적 통합성을 염두에 두고 데이터 마트가 데이터 웨어하우스보다 먼저 구축될 수도 있음

### 데이터 마트의 특징

- 분석 요건 중심
    - 전사적 데이터 웨어하우스의 데이터를 분석 요건에 적합한 구조로 재구성
- 요약 데이터
    - 추세, 패턴 분석 및 데이터 접근이 용이
    - 필요시 일부 상세 데이터 포함
- 제한된 이력 데이터
    - 분석에 필요한 이력 데이터만을 포함
- 유연성과 접근성
    - 다양한 질의나 요구를 충족하는 다차원 구조