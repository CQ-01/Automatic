# 스토어드 프로그램
## 스토어드 프로시저
### 프로시저 개요
- DB에서 제공하는 프로그램 언어
- 자주 사용되는 일반적인 쿼리를 모듈화하여 필요할때마다 호출
- 긴 SQL코드 대신 스토어드 프로시저 이름과 매개 변수 전송으로 결과값 확인
- Python, Java등에서 스토어드 프로시저 이름만 호출하여 간단히 수행
```bash
# 프로시저 생성
delimiter $$
create procedure 프로시저명 (IN/OUT 파라미터)
begin
    SQL프로그램 코딩 영역:
end $$
delimiter ;
call 프로시저명();
# 프로시저 삭제
drop procedure 프로시저명;
```
### 매개 변수
```bash
# 입력 매개변수 지정
in 입력_매개변수_이름 데이터_형식
# 매개변수 프로시저 실행
call 프로시저_이름(전달_값);
# 출력 매개 변수 지정
out 출력_매개변수_이름 데이터_형식
# 출력 매개 변수가 있는 프로시저 실행
call 프로시저명(@변수명);
select @변수명;
```
## 스토어드 함수
### 개요
- 사용자가 직접 만들어 사용하는 함수
```bash
delimiter $$
create function 함수명(파라미터)
returns 반환형식
begin
    SQL프로그램코딩:
    return 반환값;
end $$
delimiter;
select 함수명() [into @변수명];
```
### 프로시저와 차이점
- 입력 파라미터만 가능
- 하나의 값만 return문으로 제공 가능
- 집합 결과를 사용하는 select 사용불가
- select 문장으로 호출(프로시저는 call) 
## 커서
### 개요
- 테이블의 여러행을 조회하여 행별로 데이터를 처리
```bash
# 기본구문
declare 변수명 boolean default false;
declare 커서명 cursor for select 문장;
declare continue handler
    for not found set 체크변수 = true;
open 커서명;
루프명:loop
    fetch 커서명 into 변수명;
    if 체크변수 then leave 루프명;
    end if;
end loop 루프명;
close 커서명;
```
## 트리거
### 개요
- 테이블 CUD 발생 시 트리거 설정되어 있으면 트리거 자동 실행
- 제약조건과 함께 데이터 무결성을 위해 DBMS에서 제공하는 기능
- 스토어드 프로시저와 비슷한 문법으로 내용 작성
### 트리거 생성
```bash
delimiter $$
create trigger 트리거명
    {ager/before}{insert/update/delete}
    on 테이블명
    for each row
begin
    SQL 프로그램 코딩:
end $$
delimiter;
```
- after : 테이블 데이터에 변경이 가해진 후 작동
- before : 변경이 가해지기 전에 작동
- insert/update/delete : 트리거 이벤트 발생 기준
### 트리거 삭제
```bash
drop trigger 트리거명;
```
### 트리거 생성 임시테이블
|CUD|NEW TABLE|TARGET TABLE|OLD TABLE|COMMENT|
|--|--|--|--|--|
|INSERT(N)|NEW|NEW||NEW|
|UPDATE(O,N)|NEW|~~OLD~~,NEW|OLD|NEW,OLD|
|DELETE(O)||~~OLD~~|OLD|OLD|