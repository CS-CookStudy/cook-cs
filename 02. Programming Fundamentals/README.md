# 프로그래밍 패러다임

> 코드를 어떻게 구조화하고 사고화할 것인가

- 절차적 프로그래밍(PP, Procedure Programming)
- 객체지향 프로그래밍(OOP, Object Oriented Programming)
- 함수형 프로그래밍(FP, Functional Programming)


---
# 절차적 프로그래밍
> 프로시저(함수)를 호출함으로써 추상화와 코드의 재사용성을 목표로 하는 패러다임

**개념** : 프로그램을 순차적인 명령(절차)들의 집합으로 보고, 각 절차(프로시저, 함수 등)를 호출하여 문제를 해결.

### 주요 특징
1. **순차적 실행**:
프로그램은 위에서 아래로, 또는 지정된 흐름에 따라 순차적으로 실행. 각 단계가 명확한 순서를 가지며, 조건문이나 반복문을 통해 분기와 반복을 제어.

2. **함수와 서브루틴**:
코드의 재사용과 모듈화를 위해 기능별로 함수를 정의. 각 함수는 특정 작업을 수행하며, 필요에 따라 호출되어 실행.

3. **데이터와 변수**:
데이터를 저장하는 변수는 전역 변수 또는 지역 변수로 구분되며, 함수 간에 데이터를 주고받는 방식으로 프로그램이 동작.

### 장점
1. **단순하고 직관적**: 간단한 문제에 매우 효과적이며 프로그램의 흐름이 명확함. 다만, 기능 수행을 위해 해당 프로시저를 직접 호출해야 함.

2. **세밀한 성능 제어**: 단순한 스크립트나 시스템 프로그래밍, 임베디드 시스템 등에서는 절차적 프로그래밍 방식이 여전히 널리 사용

### 단점
1. **확장성의 한계**: 프로그램이 커지면 함수 간의 의존성이 복잡해지고, 코드의 재사용성과 유지보수가 어려워짐.

2. **모듈화, 구조화의 어려움**: 큰 규모의 시스템에서 코드가 하나의 흐름에 얽매여 모듈 간의 역할 분담이 어렵고, 기능 구현을 위해 함수를 만들고 인자를 전달하는 등 과정이 복잡

3. **의도치 않은 오류**: 순차적으로 실행되어야 하는데, 순서가 바뀌거나 코드 상 오류가 있을 경우 큰 문제 발생.

### 예시 (C)
```
  #include <stdio.h>

  // 두 수의 합을 구하는 함수
  int add(int a, int b) {
      return a + b;
  }

  int main() {
      int num1, num2, sum;

      // 사용자로부터 두 개의 숫자 입력 받기
      printf("첫 번째 숫자를 입력하세요: ");
      scanf("%d", &num1);
      printf("두 번째 숫자를 입력하세요: ");
      scanf("%d", &num2);

      // 함수 호출을 통해 합 계산
      sum = add(num1, num2);

      // 결과 출력
      printf("두 수의 합은 %d 입니다.\n", sum);

      return 0;
  }
```
---


# 객체지향 프로그래밍
- 객체(Object)란?


특정 실체를 '객관화'하여 인식하거나 이해하는 대상, 프로그래밍에서는 속성(변수, variable)와 행위(매서드, method)를 가진 실체.

> **데이터의 구조화**, 객체간의 상호작용을 통한 문제해결 제시

**개념** : 현실 세계의 개체(객체)를 소프트웨어적으로 모델링하여, 데이터와 그 데이터를 처리하는 기능을 하나의 단위로 결합하는 패러다임.

### 주요 특징
1. **절차적 프로그래밍 한계점 보완**:
절차적 프로그래밍은 프로시저(함수)를 구성함으로써 구조적 방식을 제안하고자 하였으나, 겉보기에만 그럴듯 하고 데이터 그 자체로서는 구조화가 되지 못 함.

2. **객체의 상호작용 중시**:
문제 해결을 위해 객체를 생성한 후, 그들 간의 조합으로 해결법을 찾음.

3. **현실 세계 모델링**:
실제 세계의 사물과 개념을 객체라는 단위로 추상화하여, 문제 영역을 보다 직관적으로 표현.

4. **모듈화**:
기능별로 클래스를 분리함으로써, 프로그램을 여러 개의 독립적인 모듈로 구성하여 유지보수와 확장이 용이.

5. **재사용성**:
상속과 다형성을 통해 기존 코드를 재사용하고, 새로운 기능을 확장할 수 있어 개발 효율이 높음.

### 핵심원리
1. **캡슐화(Encapsulation)**: 데이터와 메서드를 하나로 묶고, 외부에 필요한 부분만 공개하여 내부 구현을 숨김.

2. **추상화(Abstraction)**: 객체의 공통된 속성과 동작을 추출하여 모델링함.

3. **상속(Inheritance)**: 기존 클래스의 속성과 메서드를 새로운 클래스가 물려받아 재사용 및 확장.

4. **다형성(Polymorphism)**: 동일한 인터페이스에 대해 다양한 구현이 가능하게 하여, 코드의 유연성과 확장성을 높임.


### 장점
1. **유지보수 용이**: 내부 구현이 캡슐화되어 있어, 코드 변경 시 다른 부분에 미치는 영향을 최소화할 수 있음.

2. **확장성**:
새로운 객체와 클래스 추가가 용이하여, 복잡한 시스템에서도 유연한 확장이 가능.

3. **코드 재사용**:
상속과 인터페이스를 활용해 중복 코드를 줄이고, 모듈화된 코드를 재사용할 수 있음.

### 단점
1. **초기 설계의 복잡성**:
객체지향 설계를 위해서는 사전에 시스템의 구조와 클래스 간의 관계를 신중하게 계획해야 하므로, 초기 학습 곡선이 존재.

2. **성능 오버헤드**:
객체 생성 및 다형성 구현 등에서 객체 간의 상호작용이 많아지면, 프로그램 성능이 절차적 접근 방식보다 저하되는 약간의 오버헤드가 발생할 수 있음.

### 예시(Python)
```
  # 클래스 정의: Person
  class Person:
      def __init__(self, name, age):
          self.name = name  # 객체의 상태
          self.age = age

      # 객체의 행동을 정의하는 메서드
      def introduce(self):
          print(f"안녕하세요, 저는 {self.name}이고, {self.age}살입니다.")

  # 객체 생성
  person1 = Person("Alice", 30)
  person2 = Person("Bob", 25)

  # 메서드 호출
  person1.introduce()  # 출력: 안녕하세요, 저는 Alice이고, 30살입니다.
  person2.introduce()  # 출력: 안녕하세요, 저는 Bob이고, 25살입니다.
```

- 클래스(Class)란?


객체를 생성하기 위한 설계도(템플릿)로, 객체가 가져야할 속성과 기능 정의


## SOLID (객체 지향 설계 원칙)
- **S : 단일 책임 원칙** (SRP, Single Responsibility Principle)
    - `하나의 클래스`는 오직 `하나의 책임만` 가져야 하며, 변경의 이유도 하나뿐이다.
    - 여러 책임이 한 클래스에 섞여 있으면, 한 책임의 변경이 다른 책임의 코드에 영향을 줄 수 있음. 책임을 분리하면 응집도가 높아지고 결합도는 낮아져 유지보수가 쉬워짐.
    - EX) Calculator 라는 클래스는 사칙연산만 한다. 계산기에 알람 모드가 있어도 이 클래스에 기능을 추가하지 않고 따로 관리해줘야 한다.

- **O : 개방-폐쇠의 원칙** (OCP, Open Close Principle)
    - 확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다.
    - 기존 코드에 기능을 추가하되, 기존 코드는 변경하지 않는 설계가 되어야 함.
    - 여러 객체에서 사용하는 동일한 기능을 **인터페이스**에 정의하는 방법이 존재. 👉🏻 **캡슐화**
    - <details>
        <summary>OCP 예시 (Java)</summary>
        <pre><code class="language-java">
        // 동물의 공통 인터페이스
        public interface Animal {
            void cry();
        }


        // Dog 클래스
        public class Dog implements Animal {
            @Override
            public void cry() {
                System.out.println("멍멍");
            }
        }

        // Cat 클래스
        public class Cat implements Animal {
            @Override
            public void cry() {
                System.out.println("야옹");
            }
        }

        // 새로운 동물을 추가하고 싶다면, 예를 들어 Bird
        public class Bird implements Animal {
            @Override
            public void cry() {
                System.out.println("짹짹");
            }
        }

        // 클라이언트 코드
        public class Client {
            public static void main(String[] args) {
                Animal dog = new Dog();
                Animal cat = new Cat();
                Animal bird = new Bird(); // 새로 추가된 동물

                dog.cry();  // 멍멍
                cat.cry();  // 야옹
                bird.cry(); // 짹짹
          }
        }
        </code></pre>
    </details>

- **L : 리스코프 치환 원칙** (LSP, Liskov Subsitution Principle)
    - 자식 클래스는 최소한 부모 클래스에서 가능한 행위를 수행할 수 있어야 하며, 부모 객체로 대체해도 시스템이 정상 동작 해야 한다.
    - 자식 클래스는 부모 클래스의 책임을 무시하거나 재정의하지 않고, 확장만 수행한다. 오버라이드는 가급적 피한다.

- **I : 인터페이스 분리 원칙** (ISP, Interface Segregation Principle)
    - 자신이 사용하지 않을 인터페이스는 구현하지 않는다.
    - 변경에 따른 위험도를 낮추기 위해 하나의 거대한 인터페이스보단 여러 개의 구체적인 인터페이스가 낫다.
    - EX) 휴대폰 인터페이스에 전화, 문자, 알람, 계산 등 기능을 모두 넣지 말고, 각 기능별로 인터페이스를 분리하여 필요한 기능만 구현하도록 함.
    - <details>
        <summary>ISP 예시 (C++)</summary>
        <pre><code class="language-cpp">
          #include <iostream>
          using namespace std;

          // 각 기능별 인터페이스
          struct ICall {
              virtual void call() = 0;
          };
          struct IMessage {
              virtual void message() = 0;
          };

          // 전화만 되는 폰
          class BasicPhone : public ICall {
          public:
              void call() override {
                  cout << "전화 걸기" << endl;
              }
          };

          // 문자만 되는 폰
          class MessagePhone : public IMessage {
          public:
              void message() override {
                  cout << "문자 보내기" << endl;
              }
          };

          // 전화와 문자 모두 되는 폰
          class SmartPhone : public ICall, public IMessage {
          public:
              void call() override {
                  cout << "스마트폰: 전화 걸기" << endl;
              }
              void message() override {
                  cout << "스마트폰: 문자 보내기" << endl;
              }
          };

          int main() {
              BasicPhone bp;
              bp.call();

              MessagePhone mp;
              mp.message();

              SmartPhone sp;
              sp.call();
              sp.message();
          }

        </code></pre>
    </details>

- **D : 의존관계 역전 원칙** (DIP, Dependency Inversion Principle)
    - 하위 클래스에서 구체화가 아닌, 상위 클래스 자체가 고수준의 모듈이 되어야 한다.
    - 의존 관계는 변화하기 어렵거나 변화가 거의 없는 것과 맺어야 한다.

---


# 함수형 프로그래밍