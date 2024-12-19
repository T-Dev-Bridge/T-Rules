# Java 코딩 스타일 가이드

## 목차

1. [소스 파일 기본사항](#1-소스-파일-기본사항)
2. [코드 작성 규칙](#2-코드-작성-규칙)
3. [네이밍 규칙](#3-네이밍-규칙)
4. [주석](#4-주석)

## 1. 소스 파일 기본사항

### 1.1 파일명

- 소스 파일명은 포함된 최상위 클래스의 대소문자를 구분한 이름과 `.java` 확장자로 구성
- 파일 인코딩: UTF-8

### 1.2 소스 파일 구조
1. Package 문
2. Import 문
3. 정확히 하나의 최상위 클래스

### 1.3 Import 문 형식

- static import와 non-static import를 분리
- ASCII 정렬 순서로 작성
- 와일드카드 import(`import foo.*`) 사용 금지
- 줄 바꿈 없음

```java
import java.util.List;
import java.util.ArrayList;

import static java.lang.Math.PI;
```

## 2. 코드 작성 규칙

### 2.1 중괄호

- K&R 스타일 사용
- 블록 시작 중괄호는 같은 줄에 작성

```java
if (condition) {
    // code
}
```

### 2.2 들여쓰기

- 1 Indent = 4 Spaces ( 4 bytes )
- 블록당 1단계 들여쓰기

```java
void method() {
    if(condition){
        doSomething();
    }
}
```

### 2.3 줄 길이

- 최대 100자
- 예외: package와 import 문, URL, 긴 문자열

### 2.4 줄 바꿈

- 연산자 앞에서 줄 바꿈

```java
String longName=firstName+lastName
        +middleName;
```
### 2.5 어노테이션
- @Override는 상위 클래스 메서드 재정의시 반드시 사용
- 인터페이스 메서드 구현시에도 권장
```java
@Override
protected void onCreate(Bundle savedInstanceState){...}
```
### 2.6 정적 멤버 참조

- 클래스명으로 접근

```java
Utility.calculateSomething();   // good
calculateSomething();           // bad
```

### 2.7 불변성 선호

- 가능한 한 final 사용
- 불변 객체 선호

```java
private final String name;
private static final Logger LOGGER=LoggerFactory.getLogger(MyClass.class);
```
### 2.8 예외 처리

- catch 블록이 비어있다면 이유를 주석으로 설명

```java
try{
   someOperation();
} catch (SomeException e) {
   // 이 에러는 정상적인 흐름의 일부로 무시해도 됨
}
```

## 3. 네이밍 규칙

### 3.1 기본 원칙

- 모든 식별자는 ASCII 문자와 숫자만 사용
- 명확하고 의미있는 이름 사용
- 약어 사용은 자제하며 일반적으로 통용되는 약어만 허용
- 한글 사용 금지
- 밑줄(_) 또는 달러 기호($)로 시작하지 않음

### 3.2 패키지 (Package)

- 전체 소문자 사용
- 단어 구분 없이 연속된 단어 사용
- 각 레벨의 단어는 2~15자 내외
- 레벨화된 구조 사용

#### 패키지 레벨 구조

| 레벨 | 패키지명       | 용도                                                  |
|----|------------|-----------------------------------------------------|
| 1  | annotation | 어노테이션 관련 클래스 관리                                     |
| 2  | api        | REST API와 WebSocket 관련 Controller 클래스, Swagger 인터페이스 |
| 3  | constants  | 상수 관련 클래스 관리                                        |
| 4  | crypto     | 암호화/복호화 관련 클래스 관리                                   |
| 5  | exception  | 예외 처리 관련 클래스 관리                                    |
| 6  | filter    | 필터 관련 클래스 관리                                     |
| 7  | repository| 데이터 접근 및 엔티티 클래스 관리                                 |
| 8  | service  | 비즈니스 로직 처리 서비스 클래스 관리                                    |
| 9  | util  | 유틸리티 클래스 관리                                     |

```java
com.example.project.api.test
com.example.project.service.test
com.example.project.repository.test
```

### 3.3 클래스 (Class)

- PascalCase 사용 (각 단어 첫 글자 대문자)
- 명사 또는 명사구 사용
- 간단하고 명시적으로 작성

#### 특수 클래스 명명 규칙

- Controller: `[서비스명]Controller`
- Service: `[서비스명]Service`
- Exception: `[명사]Exception`
- Interface: 형용사 또는 명사
- Test: `[테스트대상]Test`

```java
public class OrderController
public class UserService
public interface Readable
public class InvalidTokenException
public class OrderServiceTest
```

### 3.4 메소드 (Method)

- lowerCamelCase 사용 (첫 단어 소문자, 이후 단어 첫 글자 대문자)
- 동사 또는 동사구로 시작
- 기능을 명확하게 표현

#### 3.4.1 Repository 계층 메소드 명명 규칙

| 접두어 | 용도 | 예시 |
|--------|------|------|
| findBy | 단일 데이터 조회 | `findById()`, `findByEmail()` |
| findAllBy | 목록 데이터 조회 | `findAllByStatus()`, `findAllByCategory()` |
| exists | 데이터 존재 여부 확인 | `existsById()`, `existsByEmail()` |
| count | 데이터 개수 조회 | `countByStatus()`, `countAll()` |
| save | 데이터 저장 (등록/수정) | `save()`, `saveAll()` |
| delete | 데이터 삭제 | `deleteById()`, `deleteByStatus()` |
| update | 데이터 수정 | `updateStatus()`, `updatePassword()` |
```java
public User findById(Long id);
public List<User> findAllByStatus(String status);
public boolean existsByEmail(String email);
public long countByStatus(String status);
public User save(User user);
public void deleteById(Long id);
public int updatePassword(Long id, String password);
```

#### 3.4.2 Service 계층 메소드 명명 규칙

| 접두어 | 용도 | 예시 |
|--------|------|------|
| get | 단일 데이터 조회 | `getUserInfo()`, `getOrderDetail()` |
| getList | 목록 데이터 조회 | `getOrderList()`, `getUserList()` |
| search | 검색 조건에 따른 조회 | `searchOrders()`, `searchUsers()` |
| create | 데이터 생성 | `createUser()`, `createOrder()` |
| modify | 데이터 수정 | `modifyUserInfo()`, `modifyOrderStatus()` |
| remove | 데이터 삭제 | `removeUser()`, `removeOrder()` |
| process | 복잡한 비즈니스 처리 | `processPayment()`, `processRefund()` |
| validate | 유효성 검증 | `validateUserInput()`, `validateOrder()` |
| check | 상태/조건 확인 | `checkUserStatus()`, `checkOrderAvailability()` |
| calculate | 계산 처리 | `calculateTotal()`, `calculateDiscount()` |
| convert | 데이터 변환 | `convertToDto()`, `convertToEntity()` |

```java
public UserDto getUserInfo(Long id);
public List<OrderDto> getOrderList(String status);
public List<UserDto> searchUsers(UserSearchDto searchDto);
public UserDto createUser(UserCreateDto userDto);
public void modifyUserInfo(Long id, UserUpdateDto userDto);
public void removeUser(Long id);
public PaymentDto processPayment(PaymentRequestDto paymentDto);
public boolean validateUserInput(UserCreateDto userDto);
public void checkOrderAvailability(Long orderId);
public BigDecimal calculateOrderTotal(Long orderId);
public UserDto convertToDto(User user);
```

### 3.5 변수 (Variable)

- lowerCamelCase 사용
- 명확한 의미를 가진 이름 사용
- 단일 문자 변수명 지양 (루프 제외)
- 임시 변수 외에는 약어 사용 자제

```java
// Good
int userCount;
String firstName;
boolean isActive;
for(int i=0;i<limit; i++){...}

// Bad
int cnt;  // 약어
String n;  // 불명확
boolean flag;  // 의미없는 이름
```

### 3.6 상수 (Constant)

- static final 키워드 필수
- UPPER_SNAKE_CASE 사용 (전체 대문자, 단어 사이 밑줄)
- 의미를 명확하게 전달하는 이름 사용

```java
public static final int MAX_RETRY_COUNT=3;
public static final String API_BASE_URL="https://api.example.com";
public static final long CACHE_DURATION_MS=24*60*60*1000;
```

### 3.7 제네릭 타입 매개변수 약어 사용

- 단일 대문자 사용
- 일반적인 명명 규칙:
    - T: 타입
    - E: 요소
    - K: 키
    - V: 값
    - N: 숫자

```java
public interface Map<K, V> { ...
}

public class List<E> { ...
}

public class Calculator<N extends Number> { ...
}
```

### 3.8 열거형 (Enum)

- 클래스와 동일하게 PascalCase 사용
- 각 상수는 UPPER_SNAKE_CASE 사용

```java
public enum OrderStatus {
    PAYMENT_PENDING,
    PAYMENT_COMPLETED,
    SHIPPING_PENDING,
    SHIPPING_COMPLETED
}
```

## 4. 주석

### 4.1 클래스 주석

다음의 경우 반드시 작성:

- 공개 API의 모든 public 클래스
- 공개 API의 모든 public/protected 멤버
- 표준 javadoc 형식을 준수하여 문서화

#### 클래스 주석 형식

```java
/**
 * <pre>
 * 사용자 계정을 처리하는 비즈니스 구현 클래스
 * </pre>
 *
 * @author AI웹프레임워크실 홍길동
 * @since 2024.03.20
 * @version 1.0
 * @see SomeRelatedClass
 *
 * <pre>
 * == 개정이력(Modification Information) ==
 * 수정일      수정자           수정내용
 * -------    --------    ---------------------------
 * 2024.03.20  홍길동       최초 생성
 * 2024.07.15  이순신       보안 기능 추가
 * </pre>
 *
 * Copyright (c) 2024 Your Company. All rights reserved.
 */
public class UserServiceImpl implements UserService {
    // 클래스 구현
}
```

#### 필수 포함 항목
- 클래스/인터페이스 설명
- 작성자 (@author)
- 작성일 (@since)
- 버전 (@version)
- 수정이력 (변경사항이 있는 경우)

### 4.2 메소드 주석

- 인터페이스의 메소드는 주석 생략 (구현 클래스에서 상세 기술)

#### 메소드 주석 형식

```java
/**
 * 사용자ID에 해당하는 사용자명을 조회
 *
 * @param userID 검사 항목에 대한 구분자
 * @param role 사용자 권한정보 구분
 * @return 사용자명
 * @throws MyException 사용자 정보 조회 실패시 발생
 */
public String getUserInfo(String userID,String role) throws MyException{
    // 메소드 구현
}
```

#### 필수 포함 항목

- 메소드 기능 설명
- 파라미터 설명 (@param)
- 반환값 설명 (@return)
- 예외 설명 (@throws)

### 4.3 변수 주석

#### 클래스 변수 주석

```java
/** 사용자 이름 */
private String name;

/** 임시 저장 객체 */
private Object tempValue=null;

/** 최대 재시도 횟수 */
private static final int MAX_RETRY_COUNT=3;
```

#### 메소드 내 변수 주석

```java
public void processData() {
    int indentLevel;     // 들여쓰기 레벨
    int tableSize;       // 테이블 크기
    Object currentItem;  // 현재 선택된 항목

    // 구현 코드
}
```

### 4.4 주석 작성 규칙

- 주석은 코드의 '무엇'이 아닌 '왜'를 설명
- 불필요한 주석 지양
- 주석은 코드와 동일한 들여쓰기 수준 유지
- TODO 주석은 임시적인 해결책이나 미래의 개선사항 표시에 사용

#### 블록 주석

```java
/*
 * 이 블록은 매우 중요한 처리를 수행합니다.
 * 주의: 이 로직을 변경할 때는 연관된 비즈니스 로직도
 * 함께 검토해야 합니다.
 */
```

#### 한 줄 주석

```java
// 사용자 권한 검증
if (!userHasPermission(user,"ADMIN")) {
    throw new SecurityException("권한이 없습니다.");
}
```

#### TODO 주석

```java
// TODO(홍길동): 성능 최적화 필요 (2024-01-15)
// TODO(JIRA-123): 캐시 처리 로직 추가 예정
```

## 참고 문헌

### Style Guides
[1] Google Java Style Guide  
https://google.github.io/styleguide/javaguide.html