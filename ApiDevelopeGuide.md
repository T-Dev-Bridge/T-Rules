# API 개발 가이드

## 목차
1. [기본 원칙](#1-기본-원칙)
2. [API 설계 규칙](#2-api-설계-규칙)
3. [URL/엔드포인트 규칙](#3-url-엔드포인트-규칙)
4. [HTTP 메소드 사용](#4-http-메소드-사용)
5. [응답 형식](#5-응답-형식)
6. [상태 코드](#6-상태-코드)
7. [에러 처리](#7-에러-처리)
8. [버전 관리](#8-버전-관리)

## 1. 기본 원칙

### 1.1 RESTful 설계 원칙
- 리소스 중심의 API 설계
- 클라이언트-서버 분리
- 무상태성(Stateless) 유지
- 캐시 가능성 고려
- 계층화된 시스템 허용

### 1.2 명명 규칙
- URL은 명사 사용
- 소문자 사용
- 단어 구분은 하이픈(-) 사용
- 명확하고 직관적인 이름 사용

## 2. API 설계 규칙

### 2.1 기본 URL 구조
```
https://api.example.com/v1/resources
```

### 2.2 리소스 명명
- 복수형 명사 사용
- 일관된 명명 규칙 적용

```
# Good
/users
/orders
/products

# Bad
/getUserList
/create-order
/product
```

### 2.3 계층 구조
- 리소스 간의 관계를 URL에 표현
```
/users/{userId}/orders
/orders/{orderId}/items
```

## 3. URL/엔드포인트 규칙

### 3.1 경로 파라미터
```
/users/{userId}
/orders/{orderId}
```

### 3.2 쿼리 파라미터
```
/users?role=admin&status=active
/products?category=electronics&sort=price
```

### 3.3 필터링, 정렬, 페이징
```
# 필터링
/products?category=electronics&minPrice=100

# 정렬
/products?sort=price:desc

# 페이징
/products?page=1&size=20
```

## 4. HTTP 메소드 사용

### 4.1 기본 CRUD 작업
| 메소드 | 용도 | 예시 |
|--------|------|------|
| GET | 리소스 조회 | `GET /users/{id}` |
| POST | 리소스 생성 | `POST /users` |
| PUT | 리소스 완전 수정 | `PUT /users/{id}` |
| PATCH | 리소스 부분 수정 | `PATCH /users/{id}` |
| DELETE | 리소스 삭제 | `DELETE /users/{id}` |

### 4.2 메소드별 요청 본문
```json
// POST /users
{
  "name": "홍길동",
  "email": "hong@example.com"
}

// PATCH /users/{id}
{
  "name": "홍길동2"
}
```

## 5. 응답 형식

### 5.1 기본 응답 구조
```json
{
  "success": "true",
  "data": {
    // 실제 데이터
  }
}
```

### 5.2 목록 조회 응답
```json
{
  "success": "true",
  "data": {
    "list": [...],
    "pagination": {
      "length": 746,
      "size": 15,
      "page": 0,
      "lastPage": 50,
      "startIndex": 0,
      "endIndex": 15
    }
  }
}
```

### 5.3 에러 응답
```json
{
  "success": "false",
  "message": "E4000 사용자 조회 중 오류발생"
}
```

## 6. 상태 코드

### 6.1 성공 상태 코드
| 코드 | 의미 | 사용 시점 |
|------|------|-----------|
| 200 | OK | 요청 성공 |
| 201 | Created | 리소스 생성 성공 |
| 204 | No Content | 성공했지만 응답 데이터 없음 |

### 6.2 클라이언트 에러 상태 코드
| 코드 | 의미 | 사용 시점 |
|------|------|-----------|
| 400 | Bad Request | 잘못된 요청 |
| 401 | Unauthorized | 인증 필요 |
| 403 | Forbidden | 권한 없음 |
| 404 | Not Found | 리소스 없음 |
| 409 | Conflict | 리소스 충돌 |

### 6.3 서버 에러 상태 코드
| 코드 | 의미 | 사용 시점 |
|------|------|-----------|
| 500 | Internal Server Error | 서버 내부 오류 |
| 502 | Bad Gateway | 게이트웨이 오류 |
| 503 | Service Unavailable | 서비스 이용 불가 |

## 7. 에러 처리

### 7.1. 시스템 에러 (1000번대)
| 코드 | 한글 메시지 | 영문 메시지 | HTTP 상태 코드 | 설명 |
|------|------------|-------------|----------------|------|
| E1000 | 시스템 오류가 발생했습니다. | System error occurred. | 500 | 내부 서버 오류 |
| E1001 | 서비스가 일시적으로 제공되지 않습니다. | Service temporarily unavailable. | 503 | 서비스 일시 중단 |
| E1002 | 데이터베이스 연결에 실패했습니다. | Database connection failed. | 500 | DB 연결 오류 |
| E1003 | 캐시 서버 오류가 발생했습니다. | Cache server error occurred. | 500 | 캐시 서버 오류 |
| E1004 | 외부 서비스 연동 중 오류가 발생했습니다. | External service integration failed. | 500 | 외부 API 연동 오류 |
| E1005 | 메시지 큐 서비스 오류가 발생했습니다. | Message queue service error occurred. | 500 | 메시지 큐 서비스 오류 |

### 7.2. 요청 유효성 검증 (2000번대)
| 코드 | 한글 메시지 | 영문 메시지 | HTTP 상태 코드 | 설명 |
|------|------------|-------------|----------------|------|
| E2000 | 잘못된 요청 형식입니다. | Invalid request format. | 400 | 잘못된 요청 형식 |
| E2001 | 필수 파라미터가 누락되었습니다. | Required parameter is missing. | 400 | 필수 파라미터 누락 |
| E2002 | 파라미터 형식이 올바르지 않습니다. | Invalid parameter type. | 400 | 파라미터 타입 불일치 |
| E2003 | 파라미터 값이 유효하지 않습니다. | Parameter validation failed. | 400 | 파라미터 유효성 검증 실패 |
| E2004 | 요청 크기가 허용 범위를 초과했습니다. | Request size exceeds limit. | 413 | 요청 크기 초과 |
| E2005 | 지원하지 않는 미디어 타입입니다. | Unsupported media type. | 415 | 지원하지 않는 미디어 타입 |

### 7.3. 인증/인가 (3000번대)
| 코드 | 한글 메시지 | 영문 메시지 | HTTP 상태 코드 | 설명 |
|------|------------|-------------|----------------|------|
| E3000 | 로그인이 필요한 서비스입니다. | Authentication required. | 401 | 인증 필요 |
| E3001 | 유효하지 않은 인증 토큰입니다. | Invalid token. | 401 | 유효하지 않은 토큰 |
| E3002 | 인증 토큰이 만료되었습니다. | Token expired. | 401 | 만료된 토큰 |
| E3003 | 접근 권한이 없습니다. | Insufficient permissions. | 403 | 권한 부족 |
| E3004 | 아이디 또는 비밀번호가 일치하지 않습니다. | Invalid credentials. | 401 | 잘못된 인증 정보 |
| E3005 | 계정이 잠겨있습니다. | Account is locked. | 403 | 계정 잠금 상태 |

### 7.4. 데이터베이스 (4000번대)
| 코드 | 한글 메시지 | 영문 메시지 | HTTP 상태 코드 | 설명 |
|------|------------|-------------|----------------|------|
| E4000 | 요청한 데이터를 찾을 수 없습니다. | Data not found. | 404 | 데이터 없음 |
| E4001 | 중복된 데이터가 존재합니다. | Duplicate entry exists. | 409 | 중복된 데이터 존재 |
| E4002 | 데이터베이스 쿼리 실행에 실패했습니다. | Database query failed. | 500 | DB 쿼리 실행 실패 |
| E4003 | 트랜잭션 처리에 실패했습니다. | Transaction failed. | 500 | 트랜잭션 실패 |
| E4004 | 데이터 무결성 오류가 발생했습니다. | Data integrity violation. | 409 | 데이터 무결성 위반 |
| E4005 | 데이터베이스 교착상태가 감지되었습니다. | Dead lock detected. | 500 | 데드락 발생 |

### 7.5. 외부 서비스 연동 (5000번대)
| 코드 | 한글 메시지 | 영문 메시지 | HTTP 상태 코드 | 설명 |
|------|------------|-------------|----------------|------|
| E5000 | 외부 서비스 응답 시간이 초과되었습니다. | External service timeout. | 504 | 외부 서비스 타임아웃 |
| E5001 | 외부 서비스에서 오류가 발생했습니다. | External service error. | 502 | 외부 서비스 오류 |
| E5002 | 외부 서비스 응답이 올바르지 않습니다. | Invalid external service response. | 502 | 외부 서비스 응답 형식 오류 |
| E5003 | 외부 서비스를 사용할 수 없습니다. | External service unavailable. | 503 | 외부 서비스 이용 불가 |
| E5004 | 외부 서비스 연결에 실패했습니다. | External service connection failed. | 502 | 외부 서비스 연결 실패 |

### 7.6. 파일 처리 (6000번대)
| 코드 | 한글 메시지 | 영문 메시지 | HTTP 상태 코드 | 설명 |
|------|------------|-------------|----------------|------|
| E6000 | 파일 업로드에 실패했습니다. | File upload failed. | 500 | 파일 업로드 실패 |
| E6001 | 파일 다운로드에 실패했습니다. | File download failed. | 500 | 파일 다운로드 실패 |
| E6002 | 지원하지 않는 파일 형식입니다. | Invalid file format. | 400 | 잘못된 파일 형식 |
| E6003 | 파일 크기가 허용 범위를 초과했습니다. | File size exceeded. | 413 | 파일 크기 초과 |
| E6004 | 스토리지 서비스 오류가 발생했습니다. | Storage service error. | 500 | 스토리지 서비스 오류 |
| E6005 | 파일을 찾을 수 없습니다. | File not found. | 404 | 파일 없음 |

### 7.7. 비즈니스 로직 (7000번대)
| 코드 | 한글 메시지 | 영문 메시지 | HTTP 상태 코드 | 설명 |
|------|------------|-------------|----------------|------|
| E7000 | 비즈니스 규칙이 위반되었습니다. | Business rule violated. | 400 | 비즈니스 규칙 위반 |
| E7001 | 프로세스 실행에 실패했습니다. | Process execution failed. | 500 | 프로세스 실행 실패 |
| E7002 | 잘못된 상태 전환입니다. | Invalid state transition. | 400 | 잘못된 상태 전이 |
| E7003 | 리소스 잠금에 실패했습니다. | Resource lock failed. | 409 | 리소스 잠금 실패 |
| E7004 | 배치 처리에 실패했습니다. | Batch process failed. | 500 | 배치 처리 실패 |


## 8. 버전 관리

### 8.1 버전 표기 방식
```
# URL 경로에 버전 표시
/v1/users
```

### 8.2 버전 업그레이드 절차
1. 새 버전 API 개발
2. 이전 버전과의 호환성 검증
3. 충분한 마이그레이션 기간 제공
4. 이전 버전 지원 종료 공지

## 참고 문헌

[1] RESTful Web API Design Guidelines  
https://github.com/microsoft/api-guidelines

[2] Google Cloud API Design Guide  
https://cloud.google.com/apis/design

[3] REST API Tutorial  
https://restfulapi.net/
