# TypeScript 코딩 스타일 가이드

## 목차
1. [소스 파일 기본사항](#1-소스-파일-기본사항)
2. [포맷팅](#2-포맷팅)
3. [타입 시스템](#3-타입-시스템)
4. [네이밍 규칙](#4-네이밍-규칙)
5. [클래스](#5-클래스)
6. [함수](#6-함수)
7. [비동기 처리](#7-비동기-처리)
8. [주석](#8-주석)

## 1. 소스 파일 기본사항

### 1.1 파일명
- 소문자 사용
- 단어 구분은 하이픈(-) 사용
- `.ts` 확장자 사용 (React 컴포넌트의 경우 `.tsx`)
```typescript
// Good
my-component.ts
user-service.ts
order-processor.ts

// Bad
MyComponent.ts
userService.ts
```

### 1.2 파일 인코딩
- UTF-8 사용
- BOM 사용하지 않음

### 1.3 모듈
- ES6 모듈 구문 사용 (`import`/`export`)
- 와일드카드 import는 지양
```typescript
// Good
import {Foo} from './foo';
import {Bar} from './bar';

// Bad
import * as Foo from './foo';
```

## 2. 포맷팅

### 2.1 들여쓰기
- 2 spaces 사용
- 탭 문자 사용 금지

### 2.2 중괄호
- 모든 제어문에 중괄호 필수 사용
- 한 줄 if문도 중괄호 사용
```typescript
// Good
if (someCondition) {
  doSomething();
}

// Bad
if (someCondition) doSomething();
```

### 2.3 줄 길이
- 최대 80자
- URL, 긴 문자열 등은 예외

### 2.4 세미콜론
- 문장 끝에 세미콜론(;) 필수

### 2.5 공백
- 연산자 전후에 공백
- 콤마 뒤에 공백
```typescript
// Good
let x = a + b;
function foo(a, b) { }

// Bad
let x=a+b;
function foo(a,b) { }
```

## 3. 타입 시스템

### 3.1 타입 선언
- 가능한 모든 변수와 반환값에 타입 명시
```typescript
// Good
function getUser(id: string): User {
  return new User(id);
}

// Bad
function getUser(id) {
  return new User(id);
}
```

### 3.2 인터페이스와 타입
- 인터페이스 이름은 `I` 접두어 사용하지 않음
- 일반적인 객체 타입에는 interface 사용
- 유니온 타입이나 튜플에는 type 사용
```typescript
// Good
interface User {
  name: string;
  age: number;
}

type StringOrNumber = string | number;

// Bad
interface IUser {
  name: string;
  age: number;
}
```

### 3.3 any 타입
- `any` 사용 최소화
- 불가피한 경우 주석으로 사유 설명
```typescript
// 외부 라이브러리 타입 정의가 없어 불가피하게 any 사용
const externalLibResult: any = externalLib.doSomething();
```

## 4. 네이밍 규칙

### 4.1 변수와 함수
- camelCase 사용
```typescript
const firstName: string;
function calculateTotal(): number;
```

### 4.2 클래스와 인터페이스
- PascalCase 사용
```typescript
class UserService {}
interface RequestOptions {}
```

### 4.3 타입과 열거형
- PascalCase 사용
```typescript
type StringArray = Array<string>;
enum Color { Red, Green, Blue }
```

### 4.4 상수
- UPPER_SNAKE_CASE 사용
```typescript
const MAX_RETRY_COUNT = 3;
const DEFAULT_TIMEOUT = 1000;
```

## 5. 클래스

### 5.1 멤버 접근성
- 가능한 한 private 사용
- public은 필요한 경우만 명시적 선언
```typescript
class User {
  private name: string;
  public getProfile(): UserProfile { }
}
```

### 5.2 메서드 순서
1. static members
2. instance fields
3. constructor
4. instance methods

```typescript
class Example {
  static staticField: string;
  static staticMethod() {}
  
  private field1: string;
  protected field2: number;
  
  constructor() {}
  
  public method1() {}
  private method2() {}
}
```

## 6. 함수

### 6.1 화살표 함수
- 콜백에는 화살표 함수 사용
- 메서드 정의에는 일반 함수 사용
```typescript
// Good
array.map((item) => doSomething(item));

class Example {
  method() { }  // 메서드 정의
}

// Bad
class Example {
  method = () => { }  // 불필요한 화살표 함수
}
```

### 6.2 비구조화 할당
- 매개변수 3개 이상일 경우 객체 비구조화 사용
```typescript
// Good
interface Options {
  timeout: number;
  retries: number;
  callback: () => void;
}
function request({timeout, retries, callback}: Options) {}

// Bad
function request(timeout: number, retries: number, callback: () => void) {}
```

## 7. 비동기 처리

### 7.1 Promise
- async/await 사용 권장
- Promise 체이닝은 지양
```typescript
// Good
async function fetchData() {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return data;
  } catch (error) {
    handleError(error);
  }
}

// Bad
function fetchData() {
  return fetch(url)
    .then(response => response.json())
    .then(data => data)
    .catch(handleError);
}
```

### 7.2 콜백
- 콜백보다는 Promise나 async/await 사용
- 콜백 사용시에는 화살표 함수로 표현
```typescript
// Good
someApi.request(async () => {
  await doSomething();
});

// Bad
someApi.request(function(error, result) {
  if (error) handleError(error);
  processResult(result);
});
```

## 8. 주석

### 8.1 단일 행 주석
- 코드 위에 작성
- 해당 코드와 같은 들여쓰기 수준 유지
- 주석은 코드의 '무엇'이 아닌 '왜'를 설명

```typescript
// 사용자 권한 검증
if (!hasPermission(user, 'ADMIN')) {
  throw new Error('권한 없음');
}

// 주문 금액 총계 계산 (배송비 포함)
const total = calculateOrderTotal(order);
```

### 8.2 JSDoc 주석
- 모든 public API와 주요 파일에 JSDoc 주석 작성
- 필요한 정보만 간단명료하게 작성

#### 파일/모듈 레벨 주석
```typescript
/**
 * 사용자 인증 관련 유틸리티
 * 
 * @description
 * JWT 토큰 생성, 검증 및 사용자 인증 상태 관리를 담당
 * 
 * @module Authentication
 */
```

#### 클래스 주석
```typescript
/**
 * 주문 처리 비즈니스 로직을 담당하는 클래스
 * 
 * @description
 * 주문 생성, 결제, 배송 처리의 전체 플로우를 관리한다.
 * Payment 시스템과 Delivery 시스템과 연동된다.
 */
class OrderService {
  // ...
}
```

#### 메서드 주석
```typescript
/**
 * 사용자 정보를 조회한다.
 * 
 * @param id 사용자 ID
 * @returns 사용자 정보 객체
 * @throws {UserNotFoundError} 사용자를 찾을 수 없는 경우
 */
function getUser(id: string): User {
  // 구현
}
```

### 8.3 주석 작성 기준

#### 필수 포함 사항
- 간단한 설명
- 주요 기능 또는 책임
- 특별한 주의사항 (있는 경우)

#### 선택적 포함 사항
- `@description` - 상세 설명이 필요한 경우
- `@example` - 사용 예시가 필요한 경우
- `@throws` - 예외 발생 가능한 경우
- `@see` - 관련 항목 참조 필요한 경우

#### 생략 권장 사항
- 작성자, 수정일 등 Git으로 관리되는 정보
- 자명한 매개변수나 반환값 설명
- 당연한 내용의 중복 설명

### 8.4 TODO 주석
- 임시 코드나 미래의 개선사항을 표시
- 담당자와 예정일을 명시
- 가능한 빨리 해결하고 주석 제거

```typescript
// TODO(홍길동): 성능 최적화 필요 (2024-Q1)
// TODO(JIRA-123): 캐시 처리 로직 추가 예정
```

### 8.5 주석 예시

#### 좋은 주석
```typescript
// 외부 API의 응답시간이 3초를 초과하면 기본값 사용
if (response.time > 3000) {
  return DEFAULT_VALUE;
}

/**
 * 주문의 총 결제금액을 계산한다.
 * 상품금액 + 배송비 - 할인금액으로 계산되며,
 * 마이너스 금액은 0원으로 처리된다.
 */
function calculateOrderTotal(order: Order): number
```

#### 피해야 할 주석
```typescript
// 나쁜 예: 코드를 그대로 설명하는 주석
// user 변수가 null이면 에러를 발생시킨다
if (!user) throw new Error();

// 나쁜 예: 불필요한 주석
// i를 1 증가시킨다
i++;
```

## 참고 문헌

### Style Guides
[1] Google TypeScript Style Guide  
https://google.github.io/styleguide/tsguide.html