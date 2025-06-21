# 1. HTTP / HTTPS

## 1-0. HTTP란?

- **HyperText Transfer Protocol** : 웹에서 데이터 전송을 위한 프로토콜
- **클라이언트-서버 모델** : 요청-응답 방식
- **무상태(stateless)** : 각 요청이 독립적
- **TCP 기반** : 신뢰성 있는 전송 보장

## 1-1. HTTP 메서드

### 1. GET - 데이터 조회

```http
GET /users/123 HTTP/1.1
Host: api.example.com
Accept: application/json

특징:
✅ 데이터 조회만
✅ URL에 파라미터 포함
✅ 캐싱 가능
✅ 브라우저 히스토리에 남음
❌ 데이터 길이 제한
❌ 보안에 취약 (URL에 노출)
```

### 2. POST - 데이터 생성/전송

```http
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Content-Length: 45

{
  "name": "김철수",
  "email": "kim@example.com"
}

특징:
✅ 데이터 생성/전송
✅ Body에 데이터 포함
✅ 데이터 크기 제한 없음
✅ 상대적으로 안전
❌ 캐싱 불가
❌ 브라우저 뒤로가기 시 재전송 경고
```

### 3. PUT - 데이터 수정/생성

```http
PUT /users/123 HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "name": "김철수",
  "email": "kim.new@example.com"
}

특징:
✅ 전체 리소스 교체
✅ 멱등성 보장 (여러 번 실행해도 결과 동일)
✅ 리소스가 없으면 생성
```

### 4. DELETE - 데이터 삭제

```http
DELETE /users/123 HTTP/1.1
Host: api.example.com

특징:
✅ 리소스 삭제
✅ 멱등성 보장
⚠️ 실제 삭제 vs 논리적 삭제는 구현에 따라 다름
```

### 5. 기타 메서드들

- **PATCH** : 부분 수정
- **HEAD** : 헤더 정보만 조회
- **OPTIONS** : 서버가 지원하는 메서드 확인

## 1-2. HTTP 상태 코드

| 코드 범위 | 의미            | 주요 예시                                                           |
| --------- | --------------- | ------------------------------------------------------------------- |
| 1xx       | 정보            | 100 Continue                                                        |
| 2xx       | 성공            | 200 OK, 201 Created, 204 No Content                                 |
| 3xx       | 리다이렉션      | 301 Moved Permanently, 302 Found, 304 Not Modified                  |
| 4xx       | 클라이언트 오류 | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found     |
| 5xx       | 서버 오류       | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

## 1-3. HTTP 헤더와 쿠키

### 주요 요청 헤더

```http
GET /api/users HTTP/1.1
Host: example.com                    ← 서버 도메인
User-Agent: Mozilla/5.0...           ← 브라우저 정보
Accept: application/json             ← 받고 싶은 데이터 형식
Accept-Language: ko-KR, en-US        ← 선호 언어
Authorization: Bearer eyJ0eXAi...    ← 인증 토큰
Cookie: sessionid=abc123; theme=dark ← 쿠키
Cache-Control: no-cache              ← 캐시 제어
```

### 주요 응답 헤더

```http
HTTP/1.1 200 OK
Content-Type: application/json       ← 응답 데이터 형식
Content-Length: 1234                 ← 응답 크기
Set-Cookie: sessionid=xyz789         ← 쿠키 설정
Cache-Control: max-age=3600          ← 캐시 유효시간
Location: /users/456                 ← 리다이렉션 주소
Server: nginx/1.18.0                 ← 서버 정보
```

### 쿠키의 역할

```
1. 첫 로그인: 클라이언트 → 서버
   POST /login (username, password)

2. 로그인 성공: 서버 → 클라이언트
   Set-Cookie: sessionid=abc123; HttpOnly; Secure

3. 이후 요청: 클라이언트 → 서버
   Cookie: sessionid=abc123
   (자동으로 포함되어 사용자 식별)
```

## 1-4. HTTPS와 SSL/TLS

### HTTP vs HTTPS

| 특성       | HTTP      | HTTPS                       |
| ---------- | --------- | --------------------------- |
| **보안**   | 평문 전송 | 암호화 전송                 |
| **포트**   | 80        | 443                         |
| **속도**   | 빠름      | 약간 느림 (암호화 오버헤드) |
| **신뢰성** | 낮음      | 높음                        |
| **SEO**    | 불리      | 유리                        |

### SSL/TLS 핸드셰이크 과정

```
클라이언트                     서버
    |                          |
    |------ Client Hello ----->|  1. 암호화 방식 제안
    |                          |
    |<----- Server Hello ------|  2. 암호화 방식 선택 + 인증서
    |                          |
    |--- Certificate Verify -->|  3. 인증서 검증
    |                          |
    |------ Key Exchange ----->|  4. 대칭키 교환
    |                          |
    |<---- Finished Message ---|  5. 핸드셰이크 완료
    |                          |
    |     암호화된 통신 시작      |
```

## 1-5. HTTP/1.1 vs HTTP/2 vs HTTP/3

### HTTP/1.1 특징

```
특징:
✅ 가장 널리 사용
✅ 텍스트 기반 프로토콜
✅ Keep-Alive 지원
❌ Head-of-Line Blocking
❌ 동시 연결 수 제한 (보통 6개)

예시:
GET /image1.jpg HTTP/1.1
GET /image2.jpg HTTP/1.1  ← 순차적 처리
GET /image3.jpg HTTP/1.1
```

### HTTP/2 특징

```
특징:
✅ 바이너리 프로토콜
✅ 멀티플렉싱 (동시 요청/응답)
✅ 서버 푸시
✅ 헤더 압축 (HPACK)
✅ 스트림 우선순위

예시:
Stream 1: GET /image1.jpg
Stream 2: GET /image2.jpg  ← 동시 처리
Stream 3: GET /image3.jpg
```

### HTTPS/3 특징

```
특징:
✅ UDP 기반 (QUIC 프로토콜)
✅ 연결 설정 시간 단축
✅ 패킷 손실에 강함
✅ 연결 마이그레이션
⚠️ 아직 일부 서버에서만 지원

장점: 모바일 환경에서 특히 효과적
```

<br>

# 2. DNS (Domain Name System)

## 2-0. DNS란?

- **도메인 네임 시스템** : 도메인 이름을 IP 주소로 변환
- **분산 데이터베이스** : 전 세계에 분산된 네임 서버들
- **계층적 구조** : 루트 -> TLD -> 도메인 -> 서브도메인

## 2-1. DNS 동작 원리

### DNS 질의 과정

```
사용자가 www.example.com 입력
         ↓
1. 로컬 DNS 캐시 확인
         ↓ (캐시 없음)
2. 로컬 DNS 서버 질의
         ↓
3. 루트 DNS 서버 질의 → .com 네임서버 주소 응답
         ↓
4. .com 네임서버 질의 → example.com 네임서버 주소 응답
         ↓
5. example.com 네임서버 질의 → www.example.com의 IP 주소 응답
         ↓
6. IP 주소를 사용자에게 반환
```

### 상세 질의 흐름

```
사용자 PC                로컬 DNS            루트 서버       .com 서버    example.com 서버
   |                        |                   |              |              |
   |--www.example.com?----->|                   |              |              |
   |                        |---.com 서버는?---->|              |              |
   |                        |<--192.5.5.241-----|              |              |
   |                        |                   |              |              |
   |                        |--example.com 서버는?------------->|              |
   |                        |<--203.0.113.8--------------------|              |
   |                        |                   |              |              |
   |                        |-----www.example.com의 IP는?--------------------->|
   |                        |<----93.184.216.34-------------------------------|
   |                        |                   |              |              |
   |<--93.184.216.34--------|                   |              |              |
```

## 2-2. DNS 레코드 종류

### 주요 DNS 레코드 타입

| 레코드    | 설명                 | 예시                                  |
| --------- | -------------------- | ------------------------------------- |
| **A**     | 도메인 → IPv4 주소   | example.com → 93.184.216.34           |
| **AAAA**  | 도메인 → IPv6 주소   | example.com → 2606:2800:220:1:248:... |
| **CNAME** | 도메인 → 다른 도메인 | www.example.com → example.com         |
| **MX**    | 메일 서버            | example.com → mail.example.com        |
| **NS**    | 네임 서버            | example.com → ns1.example.com         |
| **PTR**   | IP → 도메인 (역방향) | 93.184.216.34 → example.com           |
| **TXT**   | 텍스트 정보          | SPF, DKIM 등                          |

## 2-3. DNS 계층 구조

### 계층적 네임스페이스

```
                    . (루트)
                  /     \
              .com     .org    .net    .kr
               /          \              \
         google.com    example.org    naver.kr
            /               \              \
    www.google.com   www.example.org   www.naver.kr
```

### DNS 서버 종류

1. **루트 네임 서버** : 최상위 13개 서버 (a-m.root-servers.net)
2. **TLD 네임 서버** : .com, .org, .kr 등 관리
3. **권한 네임 서버** : 특정 도메인의 실제 정보 보유
4. **로컬 DNS 서버** : ISP에서 제공, 캐싱 역할

<br>

# 3. FTP, SMTP, POP3, IMAP

## 3-1. FTP (File Transter Protocol)

### FTP 특징

- **파일 전송 전용 프로토콜** : 대용량 파일 전송에 최적화
- **2개 포트 사용** : 제어용(21번), 데이터용(20번)
- **능동 / 수동 모드** : 방화벽 환경에 따라 선택

### FTP 연결 과정

```
FTP 클라이언트                 FTP 서버
      |                         |
      |-- 제어 연결 (포트 21) --> |  1. 명령어 전송용
      |                         |
      |-- 데이터 연결 (포트 20)--> | 2. 파일 전송용
      |                         |
      |     USER username       |  3. 사용자 인증
      |     PASS password       |
      |                         |
      |    LIST, RETR, STOR     |  4. 파일 명령어
```

### 능동 모드 vs 수동 모드

```
능동 모드 (Active):
클라이언트가 데이터 포트를 열고 서버가 연결
문제: 클라이언트 방화벽에서 차단 가능

수동 모드 (Passive):
서버가 데이터 포트를 열고 클라이언트가 연결
장점: 방화벽 문제 해결
```

## 3-2. SMTP (Simple Mail Transfer Protocol)

### SMTP 역할

- **메일 전송 프로토콜** : 클라이언트에서 서버로, 서버 간 전송
- **포트 25, 587, 465** : 각각 다른 용도
- **텍스트 기반** : 사람이 읽을 수 있는 명령어

### SMTP 전송 과정

```
메일 클라이언트          SMTP 서버          수신자 SMTP 서버
      |                    |                      |
      |--HELO example.com->|                      |
      |<-250 Hello---------|                      |
      |                    |                      |
      |--MAIL FROM: a@x.com|                      |
      |<-250 OK------------|                      |
      |                    |                      |
      |--RCPT TO: b@y.com->|                      |
      |<-250 OK------------|                      |
      |                    |                      |
      |--DATA------------->|                      |
      |--메일 내용--------->|                      |
      |--.(끝 표시)-------->|                      |
      |<-250 Message sent--|                      |
      |                    |                      |
      |                    |--메일 전송----------->|
```

## 3-3. POP3 vs IMAP

### POP3 (Post Office Protocol 3)

```
특징:
✅ 메일을 로컬로 다운로드
✅ 서버에서 메일 삭제 (기본값)
✅ 단순하고 빠름
✅ 저장 공간 절약
❌ 여러 기기에서 동기화 어려움
❌ 서버에서 검색 불가

동작 과정:
1. 서버 연결 (포트 110)
2. 사용자 인증
3. 메일 목록 확인
4. 메일 다운로드
5. 서버에서 삭제
6. 연결 종료
```

### IMAP (Internet Message Access Protocol)

```
특징:
✅ 메일을 서버에 보관
✅ 여러 기기에서 동기화
✅ 서버에서 검색 가능
✅ 폴더 구조 지원
✅ 부분 다운로드 가능
❌ 서버 저장 공간 필요
❌ 항상 네트워크 연결 필요

동작 과정:
1. 서버 연결 (포트 143/993)
2. 사용자 인증
3. 폴더 선택
4. 메일 헤더 확인
5. 필요한 메일만 다운로드
6. 서버와 동기화 유지
```

### POP3 vs IMAP 비교

| 특성          | POP3          | IMAP          |
| ------------- | ------------- | ------------- |
| **저장 위치** | 로컬          | 서버          |
| **다중 기기** | 어려움        | 쉬움          |
| **검색**      | 로컬만        | 서버에서 가능 |
| **동기화**    | 없음          | 실시간        |
| **네트워크**  | 다운로드 시만 | 항상 필요     |
| **저장 공간** | 로컬 사용     | 서버 사용     |

<br>

# 4. DHCP (Dynamic Host Configuration Protocol)

## 4-0. DHCP란?

- **자동 IP 할당 프로토콜** : 네트워크 설정을 자동으로 배정
- **UDP 기반** : 포트 67(서버), 68(클라이언트)
- **임대 방식** : 일정 시간 동안만 IP 사용 권한 부여

## 4-1. DHCP 동작 과정

### 4단계 DHCP 협상 (DORA)

```
클라이언트                       DHCP 서버
    |                              |
    |---- DISCOVER (브로드캐스트) -->|  1. IP 주소 요청
    |                              |
    |<--- OFFER (유니캐스트) -------|  2. IP 주소 제안
    |                              |
    |---- REQUEST (브로드캐스트) --->|  3. IP 주소 요청 확정
    |                              |
    |<--- ACK (유니캐스트) ---------|  4. IP 주소 할당 확인
```

### 각 단계별 설명

1. `DISCOVER` : 클라이언트가 브로드캐스트로 "IP 주소 필요!" 요청
2. `OFFER` : 서버가 "192.168.1.100 어때?" 제안 (IP, 게이트웨이, DNS 포함)
3. `REQUEST` : 클라이언트가 "그 IP 주세요!" 확정 요청
4. `ACK` : 서버가 "확정! 24시간 사용하세요" 최종 할당

### DHCP 할당 방식

- **동적 할당** : IP 풀에서 자동 할당 (가장 일반적)
- **정적 할당** : MAC 주소 기반 고정 IP 할당 (서버용)

## 4-2. 주요 DHCP 정보

| 항목          | 설명           | 예시          |
| ------------- | -------------- | ------------- |
| IP 주소       | 할당받을 IP    | 192.168.1.100 |
| 서브넷 마스크 | 네트워크 범위  | 255.255.255.0 |
| 게이트웨이    | 라우터 주소    | 192.168.1.1   |
| DNS 서버      | 도메인 변환    | 8.8.8.8       |
| 임대 시간     | 사용 가능 시간 | 24시간        |
