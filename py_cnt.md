# 파이썬과 MariaDB 연동
## 환경설정
- cmd : pymysql 설치

## 입력 프로그램
### 파이썬 코딩 순서
- MariaDB 연결 : pymysql.connect(연결옵션)
- 커서 생성 : 연결자.cursor()
- 데이터 입력 : 커서명.execute("insert 문장")
- 입력 데이터 저장 : 연결자.commit()
- MariaDB 연결종료 : 연결자.close()
### 파이썬 예제
```bash
import pymysql

conn = pymysql.connect(host='localhost', port=3306, user='root', password='1234', db='sqldb')
cur = conn.cursor()
sql = "insert into usertbl(userid, name, brithyear, addr) values('KIM', '김씨', 1991, '서울');"
cur.execute(sql)
conn.commit()
conn.close()
```
## 조회 프로그램
### 파이썬 코딩 순서
- MariaDB 연결 : 연결자 = pymysql.connect(연결옵션)
- 커서 생성 : 커서명 = 연결자.cursor()
- 데이터조회 : 커서명.execute("SELECT문")
- 조회 데이터 출력 : 커서명.fetchone()
- MariaDB 연결종료 : 연결자.close()
### 파이썬 예제
```bash
import pymysql

conn = pymysql.connect(host='localhost', port=3306, user='root', passwd='1234', db='sqldb')
cur = conn.cursor()
sql = "SELECT userid, name FROM userTBL;"
cur.execute(sql)
while True:
	row = cur.fetchone()
	if row == None: break;
print(row[0], row[1])
conn.close()
```