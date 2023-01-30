# 전체 텍스트 검색과 파티션
## 전체 텍스트 검색 개요
- 긴 문장으로 구성된 텍스트 내용을 빠르게 검색
- 저장된 텍스트에서 키워드를 추출하여 인덱스로 설정하는 방식
- 첫 글자 뿐만 아니라 중간 단어와 문장으로도 인덱스 생성가능
- char, varchar, text타입에만 인덱스 생성 가능
```bash
select * from 테이블명 where 검색컬럼명 like '단어%';
select * from 테이블명 where 검색컬럼명 like '%단어%';
```
## 전체 텍스트 인덱스 생성, 삭제
### 전체 텍스트 인덱스 생성
```bash
# 테이블 생성 시 인덱스 생성
create table 테이블명(
    컬럼 설정, full text 인덱스명(컬럼명));
# alter 명령어로 테이블 생성 후 인덱스 생성
alter table 테이블명
    add fulltext 인덱스명(컬럼명));
# create 명령어로 테이블 생성 후 인덱스 생성
create fulltext index 인덱스명
     on 테이블명(컬럼명);
```
### 전체 텍스트 인덱스 삭제
```bash
# alter 명령어 사용
alter table 테이블명
    drop index 인덱스명;
# drop 명령어 사용
drop index 인덱스명
    on 테이블명;
```
### 중지단어
- 긴 문장은 효율성을 위해 검색에서 무시할 만한 단어들은 제외
## 전체 텍스트 검색 쿼리
### 검색 쿼리
```bash
# 테이블 생성시 인덱스 생성
match(컬럼1, 컬럼2...)
against (검색표현식
    in natural language mode
    |in boolean mode
    |with query expansion);
```
- in natural language mode
  - 옵션 미설정 시 기본 제공모드
  - 정확히 일치하는 단어만 검색
  - 두 단어중 하나 포함여부 검색 : against('단어1 단어2')
- in boolean mode
  - 검색어를 포함한 단어나 문장 검색 가능
  - + : 반드시 포함, - : 제외, * : 부분검색
- with query expansion
  - 검색 완료 후 관련있는 내용 추가 검색 결과 제공
### IN NATURAL LANGUAGE MODE
```bash
# article 중 영화 단어가 있는 데이터 조회
select * from newspaper where match(article) against('영화');
# article 중 영화 또는 배우 단어가 있는 데이터 조회
select * from newspaper where match(article) against('영화배우');
```
### IN BOOLEAN MODE
```bash
# article 중 영화 단어가 있는 데이터 조회
select * from newspaper where match(article) against('영화*' in boolean mode);
# article 중 영화 또는 배우 단어가 있는 데이터 조회
select * from newspaper where match(article) against('영화배우', in boolean mode);
# 영화가 정확히 들어간 결과 중 공포가 반드시 포함된 데이터 조회
select * from newspaper where match(article) against('영화+공포' in boolean mode);
# 영화 단어가 정확히 들어간 결과 중 남자가 포함되지 않은 데이터 조회
select * from newspaper where match(article) against('영화-남자' in boolean mode);
```
### WITH QUERY EXPANSION
- 검색 후 데이터와 관련있는 내용 추가 검색 결과 포함해 제공
### 인덱스 생성 규칙 설정
```bash
# 인덱스 생성 단어 최소길이 확인
show variables like 'innodb_ft_min_token_size';
# 2글자까지 인덱스 생성되도록 변경 (my.ini 파일 [mysqld] 아래쪽에 세팅값 추가)
innodb_ft_min_token_size=2
# MariaDB restart
net stop mariadb
net start mariadb
```
### 중지(제외) 단어 설정
```bash
# 중지(제외) 단어 설정 테이블 생성
create table user_stopword(value varchar(30));
# 중지(제외) 단어 insert
insert into user_stopword values('단어1', '단어2'...);
# innodb GLOBAL 변수 설정/확인
set global innodb_ft_server_stopword_table = 'fulltextdb/user_stopword';
show global variables like 'innodb_ft_server_stopword_table';
# 전체 텍스트 인덱스 생성/확인
create fulltext index idx_description on fulltextTBL(description);
select * from information_schema.innodb_ft_index_table;
```
## 파티션 개요와 실습
### 파티션 생성
```bash
# range partition 생성
create table 테이블명(컬럼설정)
partition by range(컬럼명)
    (partition 파티션명 values less than (숫자),
    partition 파티션명 values less than (숫자),
    partition 파티션명 values less than maxvalue);
```
- 파티션 테이블에 PK 설정 불가
- 파티션 키로 설정한 컬럼을 포함하여 PK설정은 가능
- 숫자형 데이터만 파티션 키 설정 가능
```bash
# list partition 생성
create table 테이블명(컬럼설정)
partition by list column (컬럼명)
    partition 파티션명 values in (값1, 값2...),
    partition 파티션명 values in (값3, 값4...);
```
### 파티션 확인
```bash
# 파티션 정보 조회
select table_schema, table_name, partition_name, partition_ordinal_position, table_rows
from information_schema.partitions
where table_name = 테이블명;
# 테이블 검색 시 사용된 파티션 테이블 확인
explain paritition select 문장;
```
### 파티션 관리
```bash
# 파티션 분리
alter table 테이블명
    reorganize partition 분리할_파티션명 into
    (partition 분리_파티션명1 values less than (숫자),
    partition 분리_파티션명2 values less than maxvalue);
# 파티션 병합
alter table 테이블명
    reorganize partition 합할_파티션명1, 합할_파티션명2 into
    (partition 파티션명 values less than(숫자));
# 파티션 삭제
alter table 테이블명
    drop partition 파티션명;
# 파티션 재구성 (분리/합한뒤 수행)
optimizez table 테이블명
```