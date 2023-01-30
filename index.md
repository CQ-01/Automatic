# 인덱스
## INDEX 개요
### INDEX 개념
- 인덱스 생성 컬럼의 데이터를 정렬하여 별도 메모리 공간에 물리적 주소와 함께 저장해 검색속도 상향
- 데이터의 중복도가 높지 않아야 함
- 테이블 공간대비 약 10-20%공간 차지로 테이블 데이터가 많은 경우 최초 INDEX 생성시 많은 시간 소요
## INDEX 종류/특징
### 클러스터형 인덱스
- PRIMARY KEY
- 테이블당 한개 생성
- 물리적 데이터 정렬
- PK가 없을 경우 UNIQUE NOT NULL 칼럼
- 인덱스 생성시 데이터 전체 재정렬
- 보조 인덱스보다 검색속도는 빠르나 데이터 입력/수정/삭제는 느림
### 보조 인덱스
- 테이블당 여러개 생성 가능
- 데이터 페이지의 위치 정보를 인덱스로 구성
- 여러 컬럼을 조합하여 생성 가능
- 인덱스 생성시 데이터 정렬 불필요
- 여러개 생성 가능하나 많으면 성능저하
## INDEX 생성/삭제
### 인덱스 생성
```bash
# 생성 구문
create [unique] index 인덱스명 on 테이블명 (인덱스컬럼1, 인덱스컬럼2);
```
- unique : 고유 인덱스 생성시 사용
- create index로 생성시 보조인덱스
### 인덱스 삭제
```bash
# 삭제 구문
drop index 인덱스명 on 테이블명;
# PK제거 시 클러스터 인덱스 삭제됨
alter table 테이블명 drop primary key;
```
- 보조 인덱스 부터 삭제
- 활용도가 적은 인덱스 삭제
## INDEX 성능
### 인덱스 성능
```bash
# 인덱스 생성 후 테이블 통계정보 생성
analyze table 테이블명;
# 인덱스 사용현황(인덱스명, 검색 건수 등) 확인
explain select 문장;
# 강제 인덱스 제외
explain select 컬럼 from 테이블명 ignore index(인덱스명) where 조건문;
# 칼럼 가공시 인덱스 미적용
explain select 칼럼 from 테이블명 where left (칼럼, 2) = 값;
# 강제 인덱스 적용
explain select 컬럼 from 테이블명 use index(인덱스명) where 조건문;
```
### 인덱스 설정 기준
- where 조건에 자주 사용되는 컬럼
- join이 자주 사용되는 컬럼
- 범위로 사용하거나 집계함수 사용되는 컬럼
- order by 절에 자주 사용되는 컬럼

### 인덱스 설정시 고려사항
- insert/update/delete 빈도 수가 많은 경우
- insert가 빈번한 테이블은 PK보다 UNIQUE kEY 설정 고려
- 생성한 인덱스 중 사용빈도가 적은 인덱스는 제거