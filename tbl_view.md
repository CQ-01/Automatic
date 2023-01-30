# 테이블과 뷰
## TABLE 생성
```bash
# 테이블 생성 및 키 설정
create table 테이블명(컬럼명 타입(길이) [not null] [primary key],
칼럼명 타입(길이) [not null], [foreign key references 테이블명(칼럼명)]);
```
### 데이터 생성
```bash
insert into 테이블명 values (칼럼1 데이터, 칼럼2 데이터...);
```

## 제약 조건
### 제약조건 설정
- 데이터무결성 확보를 위함
- primary key : 기본키
- foreign kwy : 외래 키
- unique kwy : 유일 키, null 가능
- check : 조건 맞지 않으면 입력 불가
- default 정의 : 자동입력 기본 값
- null 허용 : null 허용여부
### PRIMARY KEY
- 테이블 내 데이터 구분 식별자
- 확장성, 대표성 고려
- 중복 불가, null 불가
- 칼럼 2개 이상 조합하여 PK설정 가능
```bash
alter table 테이블명 add primary key(컬럼1, 컬럼2...);
# 키 생성
alter table 테이블명 drop primary key;
# 키 삭제
show index form 테이블명
# 키 확인
```
### FOREIGN KEY
- 외래키 테이블 입력 시 기준 테이블에 데이터 존재해야 함
- 기준테이블에서 PK칼럼, unique 제약 조건 설정된 칼럼 참조
```bash
# 테이블 생성 시 외래 키 지정
create table 테이블명(컬럼설정, constraint 외래키명 foreign key(컬럼명) references 기준테이블명(기준테이블 컬럼명);)
# 테이블 생성 후 외래 키 지정
alter table 테이블명 add constraint 외래키명 foreign key(컬럼명) reference 기준테이블명(기준테이블 컬럼명);
# 외래 키 삭제
alter table 테이블명 drop constraint 외래키명;
# 기준테이블 데이터 수정/삭제 시 외래 키 테이블 동시 적용
alter table 테이블명 add constraint 외래키명 foreign key(컬럼명) references 기준테이블명(기준테이블 컬럼명) on update cascade on delete cascade;
```
### UNIQUE KEY
- 중복되지 않는 유일값만 허용
- NULL값 허용(PK와의 차이점)
```bash
# 테이블 설정 후 유니크키 설정
alter table 테이블명 add constraint 유니크키명 unique key(컬럼명);
```
### CHECK
- 데이터 입력 조건을 설정하여 조건에 부합하는 데이터만 저장가능
```bash
# 테이블 생성 후 체크키 설정
alter table 테이블명 add constraint 체크명 check (조건식);
```
### DEFAULT
- 값 없이 입력 시 자동으로 입력되는 기본 값
```bash
# 테이블 생성 후 디폴트값 설정
alter table 테이블명 alter column 컬럼명 set default 컬럼값;
```
### NULL 허용
```bash
# 테이블 생성 후 NULL 설정
alter table 테이블명 modify column 컬럼명 컬럼타입(길이) NULL/NOT NULL;
```
## TABLE 수정/삭제
### TABLE 압축
- 대용량 테이블 공간 절약
- 데이터 insert 시간 증가
```bash
# 생성 구문
create table 테이블명(컬럼설정) row_format=compressed;
```
### 임시 TABLE
- 필요에 의해 잠깐 사용
- 생성한 클라이언트에서만 사용가능
- 세션 종료 시 자동 삭제
```bash
# 생성 구문
create temporary table 테이블명(컬럼 설정);
```
### TABLE 삭제
- 외래키 제약 조건의 기준테이블 삭제 불가(외래 키 테이블 삭제 후 삭제 가능)
```bash
# 기본 구문
drop table 테이블명;
# 외래 키 테이블 검색
select * from information_schema.check_constraints where constraints_schema = 데이터베이스명 and table_name = 테이블명;
```
### TABLE 수정
```bash
# 기본 구문
alter table 테이블명 [add/drop/modify/change] 컬럼명
```
- add : 컬럼 추가
- drop : 컬럼 삭제
- modify : 컬럼 속성 변경
- change : 컬럼명 변경
## VIEW
### VIEW
- select를 통해 조회한 결과를 테이블 형태로 볼 수 있도록 생성
```bash
# 뷰 생성
create view 뷰명 as select 조회문장;
# 뷰 삭제
drop view 뷰명;
```
- 보통 읽기 전용으로 활용
- 뷰를 통해 원본테이블 수정/삭제 가능
- 사용자별 데이터 및 칼럼에 대한 접근권한 제어가능
- 복잡한 쿼리 단순화 가능