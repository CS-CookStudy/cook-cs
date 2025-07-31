# 데이터베이스 언어 정리

## 1. 데이터베이스 언어 개요

### 정의

**데이터베이스 언어**는 데이터베이스를 정의, 조작, 제어하기 위해 사용하는 특수 목적 언어입니다.

### 분류 기준

데이터베이스 언어는 **기능에 따라** 4가지로 분류됩니다:

```
데이터베이스 언어
├── DDL (Data Definition Language) - 구조 정의
├── DML (Data Manipulation Language) - 데이터 조작
├── DCL (Data Control Language) - 접근 제어
└── TCL (Transaction Control Language) - 트랜잭션 제어
```

---

## 2. DDL (Data Definition Language)

### 정의

**데이터 정의 언어**는 데이터베이스의 구조와 스키마를 정의하는 언어입니다.

### 주요 기능

- **데이터베이스 객체 생성**: 테이블, 인덱스, 뷰 등
- **구조 변경**: 기존 객체의 수정
- **객체 삭제**: 불필요한 객체 제거
- **제약조건 정의**: 무결성 규칙 설정

### 핵심 명령어

#### CREATE - 생성

```sql
-- 테이블 생성
CREATE TABLE 학생 (
    학번 VARCHAR(10) PRIMARY KEY,
    이름 VARCHAR(20) NOT NULL,
    학과 VARCHAR(30),
    학년 INT CHECK (학년 BETWEEN 1 AND 4)
);

-- 인덱스 생성
CREATE INDEX idx_학과 ON 학생(학과);

-- 뷰 생성
CREATE VIEW 3학년학생 AS
SELECT * FROM 학생 WHERE 학년 = 3;
```

#### ALTER - 수정

```sql
-- 컬럼 추가
ALTER TABLE 학생 ADD COLUMN 전화번호 VARCHAR(15);

-- 컬럼 수정
ALTER TABLE 학생 MODIFY COLUMN 이름 VARCHAR(30);

-- 제약조건 추가
ALTER TABLE 학생 ADD CONSTRAINT FK_학과
FOREIGN KEY (학과) REFERENCES 학과테이블(학과명);
```

#### DROP - 삭제

```sql
-- 테이블 삭제
DROP TABLE 임시테이블;

-- 인덱스 삭제
DROP INDEX idx_학과;

-- 뷰 삭제
DROP VIEW 3학년학생;
```

### 특징

- **즉시 실행**: DDL 명령은 즉시 커밋됨
- **스키마 변경**: 데이터베이스 구조에 영향
- **메타데이터 갱신**: 데이터 딕셔너리 자동 업데이트

---

## 3. DML (Data Manipulation Language)

### 정의

**데이터 조작 언어**는 데이터베이스의 데이터를 검색, 삽입, 수정, 삭제하는 언어입니다.

### CRUD 연산

#### SELECT - 조회 (Read)

```sql
-- 기본 조회
SELECT 이름, 학과 FROM 학생;

-- 조건 조회
SELECT * FROM 학생 WHERE 학년 = 3 AND 학과 = '컴퓨터공학과';

-- 조인 조회
SELECT 학생.이름, 수강.과목명, 수강.성적
FROM 학생 JOIN 수강 ON 학생.학번 = 수강.학번;

-- 집계 조회
SELECT 학과, COUNT(*) as 학생수, AVG(학년) as 평균학년
FROM 학생
GROUP BY 학과
HAVING COUNT(*) > 10;
```

#### INSERT - 삽입 (Create)

```sql
-- 단일 레코드 삽입
INSERT INTO 학생 (학번, 이름, 학과, 학년)
VALUES ('2023001', '김철수', '컴퓨터공학과', 1);

-- 다중 레코드 삽입
INSERT INTO 학생 VALUES
('2023002', '이영희', '전자공학과', 2),
('2023003', '박민수', '기계공학과', 3);

-- 조회 결과 삽입
INSERT INTO 우수학생
SELECT * FROM 학생 WHERE 평점 >= 3.5;
```

#### UPDATE - 수정 (Update)

```sql
-- 조건부 수정
UPDATE 학생
SET 학년 = 학년 + 1
WHERE 학과 = '컴퓨터공학과';

-- 다중 컬럼 수정
UPDATE 학생
SET 학과 = '소프트웨어학과', 학년 = 2
WHERE 학번 = '2023001';
```

#### DELETE - 삭제 (Delete)

```sql
-- 조건부 삭제
DELETE FROM 학생 WHERE 학년 = 4 AND 졸업여부 = 'Y';

-- 전체 삭제 (주의!)
DELETE FROM 임시테이블;
```

### 고급 DML 기능

- **서브쿼리**: 중첩된 질의문
- **윈도우 함수**: 분석 함수
- **CASE문**: 조건부 처리
- **집합 연산**: UNION, INTERSECT, EXCEPT

---

## 4. DCL (Data Control Language)

### 정의

**데이터 제어 언어**는 데이터베이스 접근 권한과 보안을 관리하는 언어입니다.

### 주요 명령어

#### GRANT - 권한 부여

```sql
-- 테이블 조회 권한 부여
GRANT SELECT ON 학생 TO 교무팀;

-- 다중 권한 부여
GRANT SELECT, INSERT, UPDATE ON 성적 TO 교수그룹;

-- 모든 권한 부여
GRANT ALL PRIVILEGES ON 수강 TO 관리자;

-- 권한 위임 가능하게 부여
GRANT SELECT ON 학생 TO 팀장 WITH GRANT OPTION;
```

#### REVOKE - 권한 회수

```sql
-- 특정 권한 회수
REVOKE SELECT ON 학생 FROM 교무팀;

-- 모든 권한 회수
REVOKE ALL PRIVILEGES ON 성적 FROM 임시사용자;

-- 연쇄 권한 회수
REVOKE SELECT ON 학생 FROM 팀장 CASCADE;
```

### 권한 유형

- **객체 권한**: SELECT, INSERT, UPDATE, DELETE
- **시스템 권한**: CREATE TABLE, CREATE USER, DBA
- **역할(Role)**: 권한들의 묶음

### 보안 요소

- **사용자 인증**: 로그인 검증
- **접근 제어**: 객체별 권한 관리
- **감사 추적**: 접근 기록 로깅

---

## 5. TCL (Transaction Control Language)

### 정의

**트랜잭션 제어 언어**는 트랜잭션의 실행을 제어하는 언어입니다.

### 주요 명령어

#### COMMIT - 확정

```sql
-- 트랜잭션 시작
BEGIN TRANSACTION;

-- 데이터 변경
UPDATE 계좌 SET 잔액 = 잔액 - 100000 WHERE 계좌번호 = 'A001';
UPDATE 계좌 SET 잔액 = 잔액 + 100000 WHERE 계좌번호 = 'B001';

-- 변경사항 확정
COMMIT;
```

#### ROLLBACK - 취소

```sql
-- 트랜잭션 시작
BEGIN TRANSACTION;

-- 데이터 변경
DELETE FROM 학생 WHERE 학과 = '폐지학과';

-- 문제 발생 시 취소
ROLLBACK;
```

#### SAVEPOINT - 중간 저장점

```sql
-- 트랜잭션 시작
BEGIN TRANSACTION;

-- 첫 번째 작업
INSERT INTO 학생 VALUES ('2023004', '홍길동', '컴퓨터공학과', 1);
SAVEPOINT SP1;

-- 두 번째 작업
UPDATE 학생 SET 학년 = 2 WHERE 학번 = '2023001';
SAVEPOINT SP2;

-- 세 번째 작업에서 오류 발생
DELETE FROM 학생; -- 실수!

-- SP2 지점으로 부분 롤백
ROLLBACK TO SP2;

-- 최종 확정
COMMIT;
```

### 트랜잭션 속성 제어

- **격리 수준**: READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE
- **자동 커밋**: AUTOCOMMIT ON/OFF
- **타임아웃**: 장시간 실행 트랜잭션 제어

---

## 6. 언어별 특징 비교

### 기능별 분류

| 언어 | 목적          | 주요 명령어                    | 실행 특성          |
| ---- | ------------- | ------------------------------ | ------------------ |
| DDL  | 구조 정의     | CREATE, ALTER, DROP            | 즉시 커밋          |
| DML  | 데이터 조작   | SELECT, INSERT, UPDATE, DELETE | 트랜잭션 내 실행   |
| DCL  | 접근 제어     | GRANT, REVOKE                  | 즉시 적용          |
| TCL  | 트랜잭션 제어 | COMMIT, ROLLBACK, SAVEPOINT    | 트랜잭션 경계 제어 |

### 사용 주체별 분류

- **DDL**: 데이터베이스 설계자, DBA
- **DML**: 응용 프로그램 개발자, 최종 사용자
- **DCL**: 데이터베이스 관리자(DBA)
- **TCL**: 응용 프로그램 개발자

### 영향 범위

- **DDL**: 스키마 구조 (메타데이터)
- **DML**: 실제 데이터
- **DCL**: 보안 정책
- **TCL**: 트랜잭션 상태
