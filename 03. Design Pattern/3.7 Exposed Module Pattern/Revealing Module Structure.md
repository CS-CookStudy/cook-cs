# 📘 디자인 패턴

# 공개 모듈 패턴 (Revealing Module Pattern)

> 또는: 노출 모듈 패턴

## 1. 정의

공개 모듈 패턴은 **모듈의 내부 구현을 숨기고 필요한 부분만 선택적으로 외부에 노출**하는 디자인 패턴이다. 클로저를 활용하여 **진정한 private 멤버를 구현**하고, 공개할 메서드와 속성만을 명시적으로 반환함으로써 **캡슐화와 정보 은닉**을 실현한다.

전역 네임스페이스 오염을 방지하고, 모듈 간의 충돌을 예방하며, **안전하고 재사용 가능한 코드 구조**를 만들 수 있다.

> 참고: 이 패턴은 GoF의 23개 디자인 패턴에는 포함되지 않으며, JavaScript 커뮤니티에서 발전된 모듈 패턴의 변형입니다.

> ⚠️ 용어 주의: 일부 문서나 강의에서는 `Exposed Module Pattern`이라는 비공식 명칭을 사용하는 경우가 있으나, 이는 대부분 `Revealing Module Pattern`을 지칭합니다. 본 문서에서는 정식 명칭인 Revealing Module Pattern으로 통일합니다.

### 언제 사용하면 좋은가?

**개발 관련 예시로 이해하기:**

* **API 클라이언트 라이브러리**: 내부 HTTP 요청 로직은 숨기고 `get()`, `post()` 같은 메서드만 노출
* **데이터베이스 커넥션 풀**: 복잡한 커넥션 관리 로직은 숨기고 `getConnection()`, `releaseConnection()` 만 제공
* **암호화 라이브러리**: 복잡한 암호화 알고리즘은 내부에 숨기고 `encrypt()`, `decrypt()` 인터페이스만 노출
* **로깅 시스템**: 파일 I/O, 포맷팅 로직은 숨기고 `log()`, `error()`, `debug()` 메서드만 제공

**이런 상황에서 Revealing Module Pattern을 사용하면 좋습니다:**

* **민감한 설정값이나 토큰**을 외부에서 직접 접근하지 못하게 하고 싶을 때
* **복잡한 내부 로직**을 숨기고 간단한 API만 제공하고 싶을 때
* **전역 네임스페이스 오염을 방지**하고 모듈 간 충돌을 피하고 싶을 때
* **라이브러리나 SDK**를 개발할 때 내부 구현 변경이 외부에 영향을 주지 않게 하고 싶을 때
* **레거시 JavaScript 환경**에서 진정한 private 멤버가 필요할 때

## 2. 구조도

![Revealing Module Pattern](./images/Revealing%20Module%20Structure.png)

* 출처: JavaScript 디자인 패턴 가이드

> - `IIFE (즉시실행함수)`: 모듈을 감싸는 함수로 private 스코프 생성
> - `Private Members`: 외부에서 접근할 수 없는 변수와 함수들
> - `Public API`: return문을 통해 노출되는 공개 인터페이스
> - `Closure`: private 멤버에 대한 접근을 유지하는 메커니즘

## 3. 예시

### 예: 은행 계좌 관리 시스템

```javascript
var BankAccount = (function() {
    // Private 멤버들 - 외부에서 절대 접근 불가
    var balance = 1000000;
    var accountNumber = "123-456-789";
    var transactionHistory = [];
    
    function validatePassword(inputPassword) {
        return inputPassword === "secret123";
    }
    
    function addTransaction(type, amount) {
        transactionHistory.push({
            type: type,
            amount: amount,
            date: new Date(),
            balance: balance
        });
    }
    
    function formatCurrency(amount) {
        return amount.toLocaleString() + "원";
    }
    
    // Public API만 선택적으로 노출
    return {
        // 잔액 조회 (비밀번호 필요)
        getBalance: function(password) {
            if (!validatePassword(password)) {
                return "비밀번호가 틀렸습니다.";
            }
            return formatCurrency(balance);
        },
        
        // 입금
        deposit: function(amount, password) {
            if (!validatePassword(password)) {
                return false;
            }
            if (amount <= 0) {
                console.log("입금액은 0보다 커야 합니다.");
                return false;
            }
            balance += amount;
            addTransaction("입금", amount);
            console.log(formatCurrency(amount) + " 입금 완료");
            return true;
        },
        
        // 출금
        withdraw: function(amount, password) {
            if (!validatePassword(password)) {
                return false;
            }
            if (amount <= 0) {
                console.log("출금액은 0보다 커야 합니다.");
                return false;
            }
            if (amount > balance) {
                console.log("잔액이 부족합니다.");
                return false;
            }
            balance -= amount;
            addTransaction("출금", amount);
            console.log(formatCurrency(amount) + " 출금 완료");
            return true;
        },
        
        // 거래 내역 조회 (최근 5건만)
        getRecentTransactions: function(password) {
            if (!validatePassword(password)) {
                return "접근 거부";
            }
            return transactionHistory.slice(-5);
        }
    };
})();

// 사용
console.log(BankAccount.getBalance("secret123")); // "1,000,000원"
BankAccount.deposit(50000, "secret123");          // "50,000원 입금 완료"
BankAccount.withdraw(20000, "secret123");         // "20,000원 출금 완료"

// Private 멤버는 접근 불가
console.log(BankAccount.balance);                 // undefined
console.log(BankAccount.accountNumber);           // undefined
```

### 예: 게임 캐릭터 관리 시스템


```javascript
var GameCharacter = (function() {
    // Private 속성들
    var health = 100;
    var mana = 50;
    var experience = 0;
    var level = 1;
    var inventory = [];
    
    function calculateLevel() {
        return Math.floor(experience / 100) + 1;
    }
    
    function updateLevel() {
        var newLevel = calculateLevel();
        if (newLevel > level) {
            level = newLevel;
            health = level * 100; // 레벨업 시 체력 회복
            console.log("레벨 업! 현재 레벨: " + level);
        }
    }
    
    function isValidItem(item) {
        return item && typeof item === 'string' && item.length > 0;
    }
    
    // Public API
    return {
        // 캐릭터 정보 조회
        getStats: function() {
            return {
                level: level,
                health: health,
                mana: mana,
                experience: experience
            };
        },
        
        // 경험치 획득
        gainExperience: function(amount) {
            if (amount > 0) {
                experience += amount;
                console.log(amount + " 경험치 획득!");
                updateLevel();
            }
        },
        
        // 아이템 사용
        useHealthPotion: function() {
            var potionIndex = inventory.indexOf("체력 포션");
            if (potionIndex === -1) {
                console.log("체력 포션이 없습니다.");
                return false;
            }
            
            inventory.splice(potionIndex, 1);
            health = Math.min(health + 30, level * 100);
            console.log("체력 포션 사용! 현재 체력: " + health);
            return true;
        },
        
        // 아이템 추가
        addItem: function(item) {
            if (isValidItem(item)) {
                inventory.push(item);
                console.log(item + "을(를) 획득했습니다!");
                return true;
            }
            return false;
        },
        
        // 인벤토리 조회
        getInventory: function() {
            return inventory.slice(); // 복사본 반환으로 원본 보호
        },
        
        // 전투
        attack: function(damage) {
            if (mana >= 10) {
                mana -= 10;
                console.log(damage + " 데미지로 공격! 남은 마나: " + mana);
                return damage;
            } else {
                console.log("마나가 부족합니다.");
                return 0;
            }
        }
    };
})();

// 사용
console.log(GameCharacter.getStats());        // {level: 1, health: 100, mana: 50, experience: 0}
GameCharacter.gainExperience(150);            // "150 경험치 획득!" "레벨 업! 현재 레벨: 2"
GameCharacter.addItem("체력 포션");            // "체력 포션을(를) 획득했습니다!"
GameCharacter.useHealthPotion();              // "체력 포션 사용! 현재 체력: 200"

// Private 멤버 접근 시도 (실패)
console.log(GameCharacter.health);            // undefined
GameCharacter.inventory.push("치트 아이템");   // 에러! inventory는 접근 불가
```


### 예: 설정 관리 모듈 (Java 버전 - 정보 은닉 예시)

```java
// Java 버전 예시
public class ConfigurationManager {
    // Private 멤버들
    private static Map<String, String> settings = new HashMap<>();
    private static String encryptionKey = "secret_key_2024";
    private static boolean isInitialized = false;
    
    // Private 메서드들
    private static boolean validateKey(String key) {
        return key != null && !key.trim().isEmpty();
    }
    
    private static String encrypt(String value) {
        // 간단한 암호화 로직 (실제로는 더 복잡함)
        return Base64.getEncoder().encodeToString(value.getBytes());
    }
    
    private static String decrypt(String encryptedValue) {
        return new String(Base64.getDecoder().decode(encryptedValue));
    }
    
    // Public API만 노출
    public static void initialize() {
        if (!isInitialized) {
            settings.put("app_name", encrypt("MyApplication"));
            settings.put("version", encrypt("1.0.0"));
            settings.put("debug_mode", encrypt("false"));
            isInitialized = true;
            System.out.println("설정 초기화 완료");
        }
    }
    
    public static String getSetting(String key) {
        if (!isInitialized) {
            throw new IllegalStateException("설정이 초기화되지 않았습니다.");
        }
        if (!validateKey(key)) {
            return null;
        }
        String encryptedValue = settings.get(key);
        return encryptedValue != null ? decrypt(encryptedValue) : null;
    }
    
    public static boolean setSetting(String key, String value) {
        if (!isInitialized) {
            throw new IllegalStateException("설정이 초기화되지 않았습니다.");
        }
        if (!validateKey(key) || value == null) {
            return false;
        }
        settings.put(key, encrypt(value));
        return true;
    }
    
    public static Set<String> getAvailableKeys() {
        return new HashSet<>(settings.keySet()); // 복사본 반환
    }
}

// 사용
ConfigurationManager.initialize();
System.out.println(ConfigurationManager.getSetting("app_name")); // "MyApplication"
ConfigurationManager.setSetting("theme", "dark");
```


## 4. 다양한 구현 방식

### 즉시실행함수 (IIFE) 방식


```javascript
var MyModule = (function() {
    var privateVar = "숨겨진 값";
    
    return {
        publicMethod: function() {
            return privateVar;
        }
    };
})();
```

### 네임스페이스 방식

```javascript
var MyApp = MyApp || {};

MyApp.UserModule = (function() {
    var currentUser = null;
    
    return {
        login: function(username) {
            currentUser = username;
        },
        getCurrentUser: function() {
            return currentUser;
        }
    };
})();
```

### 매개변수를 받는 모듈

```javascript
var Calculator = (function(window, document) {
    var history = [];

    return {
        add: function(a, b) {
            var result = a + b;
            history.push(a + " + " + b + " = " + result);
            return result;
        },

        getHistory: function() {
            return history.slice();
        }
    };
})(window, document);
```

## 5. 장단점

| 장점        | 설명                        |
| --------- | ------------------------- |
| 캡슐화       | 내부 구현을 완전히 숨기고 인터페이스만 노출  |
| 네임스페이스 보호 | 전역 변수 오염 방지 및 모듈 간 충돌 예방  |
| 정보 은닉     | 중요한 데이터와 로직을 외부로부터 보호     |
| 재사용성      | 독립적인 모듈로 다른 프로젝트에서 재사용 가능 |

| 단점      | 설명                         |
| ------- | -------------------------- |
| 메모리 사용량 | 클로저로 인한 메모리 점유량 증가         |
| 복잡성 증가  | 간단한 기능에 비해 구조가 복잡할 수 있음    |
| 디버깅 어려움 | Private 멤버는 개발자 도구에서 접근 불가 |
| 상속의 어려움 | 전통적인 객체 상속 패턴 적용이 복잡함      |

## 6. 활용 예시

* JavaScript 라이브러리 개발 (jQuery, Lodash 등)
* Node.js 모듈 시스템
* 웹 애플리케이션의 컴포넌트 모듈화
* API 클라이언트 라이브러리
* 게임 엔진의 시스템 모듈

## Revealing Module Pattern을 사용하지 말아야 할 경우

* 모든 메서드와 속성이 public이어야 하는 단순한 유틸리티 함수들
* 성능이 매우 중요하고 메모리 사용량을 최소화해야 하는 경우
* 객체 지향 상속이 주요 설계 패턴인 애플리케이션
* ES6+ 환경에서 class와 private 필드를 사용할 수 있는 경우

> 예: 간단한 수학 계산 함수들이나 상수값들만 모아둔 모듈에서는 굳이 복잡한 클로저 구조가 필요 없음

## 유사 패턴과 비교

| 항목    | Revealing Module Pattern | Module Pattern   | Singleton Pattern |
| ----- | ------------------------ | ---------------- | ----------------- |
| 목적    | 캡슐화와 선택적 노출              | 기본적인 모듈화         | 인스턴스 개수 제어        |
| 구현 방식 | private 함수 → return에 매핑  | return 객체에 직접 정의 | 생성자 제어            |
| 사용 시점 | 복잡한 내부 로직 정리             | 간단한 모듈 분리        | 전역 상태 관리          |
| 예시    | 라이브러리 API, 게임 로직         | 유틸리티 함수 모음       | 설정 관리자 등          |

> 핵심 차이: Revealing Module은 "무엇을 보여줄 것인가"에 집중, Module은 "어떻게 묶을 것인가"에 집중, Singleton은 "몇 개를 만들 것인가"에 집중

## 핵심 요약

* Revealing Module Pattern은 **클로저를 활용하여 진정한 private 멤버를 구현**하고 필요한 부분만 선택적으로 노출하는 패턴이다.
* **전역 네임스페이스 오염을 방지**하고 모듈 간 충돌을 예방한다.
* JavaScript에서 **ES6 이전에 캡슐화를 구현**하는 주요 방법이었다.
* **즉시실행함수(IIFE)와 클로저**를 핵심 메커니즘으로 사용한다.

## 용어 설명

* **IIFE (Immediately Invoked Function Expression)**: 정의와 동시에 실행되는 함수 표현식으로, 독립적인 스코프를 생성
* **클로저 (Closure)**: 함수가 정의된 스코프의 변수에 접근할 수 있는 메커니즘
* **캡슐화 (Encapsulation)**: 데이터와 메서드를 하나로 묶고 외부 접근을 제한하는 객체지향 원칙
* **정보 은닉 (Information Hiding)**: 객체의 내부 구현을 외부로부터 숨기는 설계 원칙
* **네임스페이스 (Namespace)**: 이름 충돌을 방지하기 위해 식별자들을 그룹화하는 메커니즘
* **전역 오염 (Global Pollution)**: 전역 스코프에 너무 많은 변수나 함수가 선언되어 발생하는 문제
* **스코프 (Scope)**: 변수나 함수의 유효 범위
* **Private 멤버**: 외부에서 직접 접근할 수 없는 객체의 속성이나 메서드
* **Public API**: 외부에서 사용할 수 있도록 공개된 인터페이스
