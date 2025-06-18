# 선형 자료 구조

## 1. 배열 (Array)

### 정적 배열 vs 동적 배열

**정적 배열 (Static Array)**

- 컴파일 시간에 크기가 결정됨
- 스택 메모리에 할당
- 크기 변경 불가
- 메모리 효율적

```java
int[] arr = new int[10]; // 크기 10으로 고정
```

**동적 배열 (Dynamic Array)**

- 런타임에 크기 결정/변경 가능
- 힙 메모리에 할당
- 크기 확장 시 새로운 메모리 할당 후 복사
- ArrayList, Vector 등

```java
ArrayList<Integer> list = new ArrayList<>(); // 크기 가변
Vector<String> vector = new Vector<>();
```

**동적 배열 확장 메커니즘:**

- 용량 초과 시 보통 2배 또는 1.5배로 확장
- 기존 데이터를 새 배열로 복사 (O(n))
- Amortized O(1) 삽입 시간 복잡도

### 다차원 배열

**2차원 배열:**

```java
int[][] arr = new int[3][4]; // 3행 4열
int[][] jaggedArray = {{1, 2}, {3, 4, 5}, {6}}; // 가변 길이
```

### 시간/공간 복잡도 분석

| 연산            | 시간 복잡도 | 설명                   |
| --------------- | ----------- | ---------------------- |
| 접근 (Access)   | O(1)        | 인덱스로 직접 접근     |
| 탐색 (Search)   | O(n)        | 순차 탐색 필요         |
| 삽입 (Insert)   | O(n)        | 중간 삽입 시 요소 이동 |
| 삭제 (Delete)   | O(n)        | 중간 삭제 시 요소 이동 |
| 맨 끝 삽입/삭제 | O(1)        | 동적 배열의 경우       |

**공간 복잡도:** O(n)

## 2. 연결 리스트 (Linked List)

### 단일 연결 리스트 (Singly Linked List)

```java
class ListNode {
    int data;
    ListNode next;

    public ListNode(int data) {
        this.data = data;
        this.next = null;
    }
}

class SinglyLinkedList {
    private ListNode head;

    // 삽입
    public void insert(int data) {
        ListNode newNode = new ListNode(data);
        if (head == null) {
            head = newNode;
        } else {
            ListNode current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
    }

    // 삭제
    public boolean delete(int data) {
        if (head == null) return false;

        if (head.data == data) {
            head = head.next;
            return true;
        }

        ListNode current = head;
        while (current.next != null && current.next.data != data) {
            current = current.next;
        }

        if (current.next != null) {
            current.next = current.next.next;
            return true;
        }
        return false;
    }

    // 탐색
    public ListNode search(int data) {
        ListNode current = head;
        while (current != null) {
            if (current.data == data) {
                return current;
            }
            current = current.next;
        }
        return null;
    }
}
```

### 이중 연결 리스트 (Doubly Linked List)

```java
class DoublyListNode {
    int data;
    DoublyListNode next;
    DoublyListNode prev;

    public DoublyListNode(int data) {
        this.data = data;
        this.next = null;
        this.prev = null;
    }
}

class DoublyLinkedList {
    private DoublyListNode head;
    private DoublyListNode tail;

    public void insert(int data) {
        DoublyListNode newNode = new DoublyListNode(data);

        if (head == null) {
            head = tail = newNode;
        } else {
            tail.next = newNode;
            newNode.prev = tail;
            tail = newNode;
        }
    }

    public boolean delete(int data) {
        DoublyListNode current = head;

        while (current != null) {
            if (current.data == data) {
                if (current.prev != null) {
                    current.prev.next = current.next;
                } else {
                    head = current.next;
                }

                if (current.next != null) {
                    current.next.prev = current.prev;
                } else {
                    tail = current.prev;
                }
                return true;
            }
            current = current.next;
        }
        return false;
    }
}
```

### 원형 연결 리스트 (Circular Linked List)

```java
class CircularLinkedList {
    private ListNode head;

    public void insert(int data) {
        ListNode newNode = new ListNode(data);

        if (head == null) {
            head = newNode;
            newNode.next = head; // 자기 자신을 가리킴
        } else {
            ListNode current = head;
            while (current.next != head) {
                current = current.next;
            }
            current.next = newNode;
            newNode.next = head;
        }
    }
}
```

### 배열 vs 연결 리스트 비교

| 특성            | 배열          | 연결 리스트       |
| --------------- | ------------- | ----------------- |
| 메모리          | 연속적        | 비연속적          |
| 접근 방식       | Random Access | Sequential Access |
| 접근 시간       | O(1)          | O(n)              |
| 삽입/삭제       | O(n)          | O(1)              |
| 메모리 오버헤드 | 없음          | 포인터 저장 공간  |
| 캐시 효율성     | 좋음          | 나쁨              |

## 3. 스택 (Stack)

### LIFO 원리

- Last In First Out
- 가장 나중에 들어간 데이터가 가장 먼저 나옴

### 스택 구현

**배열 기반 구현:**

```java
class ArrayStack {
    private int[] stack;
    private int top;
    private int maxSize;

    public ArrayStack(int size) {
        maxSize = size;
        stack = new int[maxSize];
        top = -1;
    }

    public void push(int data) {
        if (top < maxSize - 1) {
            stack[++top] = data;
        } else {
            throw new RuntimeException("Stack Overflow");
        }
    }

    public int pop() {
        if (top >= 0) {
            return stack[top--];
        } else {
            throw new RuntimeException("Stack Underflow");
        }
    }

    public int peek() {
        if (top >= 0) {
            return stack[top];
        } else {
            throw new RuntimeException("Stack is Empty");
        }
    }

    public boolean isEmpty() {
        return top == -1;
    }
}
```

**연결 리스트 기반 구현:**

```java
class LinkedStack {
    private ListNode top;

    public void push(int data) {
        ListNode newNode = new ListNode(data);
        newNode.next = top;
        top = newNode;
    }

    public int pop() {
        if (top == null) {
            throw new RuntimeException("Stack Underflow");
        }
        int data = top.data;
        top = top.next;
        return data;
    }

    public int peek() {
        if (top == null) {
            throw new RuntimeException("Stack is Empty");
        }
        return top.data;
    }

    public boolean isEmpty() {
        return top == null;
    }
}
```

### 활용 예시

**1. 함수 호출 스택 (Call Stack)**

- 함수 호출 시 스택에 push
- 함수 종료 시 스택에서 pop
- 지역 변수와 매개변수 저장

**2. 괄호 검사**

```java
public boolean isValidParentheses(String s) {
    Stack<Character> stack = new Stack<>();
    Map<Character, Character> pairs = new HashMap<>();
    pairs.put('(', ')');
    pairs.put('[', ']');
    pairs.put('{', '}');

    for (char c : s.toCharArray()) {
        if (pairs.containsKey(c)) {
            stack.push(c);
        } else if (pairs.containsValue(c)) {
            if (stack.isEmpty() || pairs.get(stack.pop()) != c) {
                return false;
            }
        }
    }

    return stack.isEmpty();
}
```

**3. 후위 표기법 계산**

```java
public int evaluatePostfix(String expression) {
    Stack<Integer> stack = new Stack<>();
    String[] tokens = expression.split(" ");

    for (String token : tokens) {
        if (isOperator(token)) {
            int b = stack.pop();
            int a = stack.pop();
            stack.push(calculate(a, b, token));
        } else {
            stack.push(Integer.parseInt(token));
        }
    }

    return stack.pop();
}

private boolean isOperator(String token) {
    return token.equals("+") || token.equals("-") ||
           token.equals("*") || token.equals("/");
}

private int calculate(int a, int b, String operator) {
    switch (operator) {
        case "+": return a + b;
        case "-": return a - b;
        case "*": return a * b;
        case "/": return a / b;
        default: throw new IllegalArgumentException("Invalid operator");
    }
}
```

## 4. 큐 (Queue)

### FIFO 원리

- First In First Out
- 가장 먼저 들어간 데이터가 가장 먼저 나옴
- 대기줄과 같은 개념

### 선형 큐 vs 원형 큐

**선형 큐 (배열 기반):**

```java
class LinearQueue {
    private int[] queue;
    private int front, rear;
    private int maxSize;

    public LinearQueue(int size) {
        maxSize = size;
        queue = new int[maxSize];
        front = rear = -1;
    }

    public void enqueue(int data) {
        if (rear == maxSize - 1) {
            throw new RuntimeException("Queue is Full");
        }
        if (front == -1) front = 0;
        queue[++rear] = data;
    }

    public int dequeue() {
        if (front == -1 || front > rear) {
            throw new RuntimeException("Queue is Empty");
        }
        return queue[front++];
    }
}
```

**원형 큐:**

```java
class CircularQueue {
    private int[] queue;
    private int front, rear;
    private int maxSize;
    private int size;

    public CircularQueue(int capacity) {
        maxSize = capacity;
        queue = new int[maxSize];
        front = rear = 0;
        size = 0;
    }

    public void enqueue(int data) {
        if (size == maxSize) {
            throw new RuntimeException("Queue is Full");
        }
        queue[rear] = data;
        rear = (rear + 1) % maxSize;
        size++;
    }

    public int dequeue() {
        if (size == 0) {
            throw new RuntimeException("Queue is Empty");
        }
        int data = queue[front];
        front = (front + 1) % maxSize;
        size--;
        return data;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == maxSize;
    }
}
```

### 우선순위 큐 (Priority Queue)

```java
import java.util.PriorityQueue;
import java.util.Comparator;

// 기본 사용 (최소 힙)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(5);
minHeap.offer(2);
minHeap.offer(8);
System.out.println(minHeap.poll()); // 2

// 최대 힙
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Comparator.reverseOrder());

// 커스텀 객체 우선순위 큐
class Task {
    String name;
    int priority;

    public Task(String name, int priority) {
        this.name = name;
        this.priority = priority;
    }
}

PriorityQueue<Task> taskQueue = new PriorityQueue<>(
    (a, b) -> Integer.compare(b.priority, a.priority) // 높은 우선순위 먼저
);
```

### 덱 (Deque - Double Ended Queue)

```java
import java.util.ArrayDeque;
import java.util.Deque;

Deque<Integer> deque = new ArrayDeque<>();

// 앞쪽 삽입/삭제
deque.addFirst(1);
deque.addFirst(2);
int first = deque.removeFirst();

// 뒤쪽 삽입/삭제
deque.addLast(3);
deque.addLast(4);
int last = deque.removeLast();

// 스택처럼 사용
deque.push(5);    // addFirst와 동일
int top = deque.pop();  // removeFirst와 동일

// 큐처럼 사용
deque.offer(6);   // addLast와 동일
int head = deque.poll(); // removeFirst와 동일
```

---

# 면접 단골 질문 & 답안

## Q1. 배열과 연결리스트의 차이점을 설명하고, 각각 언제 사용해야 하나요?

**A:**
배열은 메모리에 연속적으로 저장되어 인덱스로 O(1) 접근이 가능하지만, 중간 삽입/삭제 시 O(n)이 걸립니다. 연결리스트는 노드들이 포인터로 연결되어 있어 순차 접근만 가능하지만(O(n)), 위치를 알고 있다면 삽입/삭제가 O(1)입니다.

**사용 시기:**

- **배열**: 데이터에 자주 접근하고 크기가 고정적일 때
- **연결리스트**: 데이터 삽입/삭제가 빈번하고 크기가 가변적일 때

## Q2. 동적 배열이 크기를 확장할 때의 시간 복잡도는?

**A:**
크기 확장 시에는 O(n) 시간이 걸립니다. 새로운 메모리를 할당하고 기존 데이터를 모두 복사해야 하기 때문입니다. 하지만 확장 빈도가 낮기 때문에 평균적으로(amortized) O(1)의 삽입 시간을 가집니다. ArrayList는 보통 1.5배로 확장합니다.

## Q3. 스택 오버플로우가 발생하는 이유는?

**A:**
함수가 재귀적으로 너무 많이 호출되거나 지역 변수가 과도하게 선언될 때, 제한된 스택 메모리 공간을 초과하여 발생합니다. 특히 무한 재귀나 깊은 재귀 호출이 주된 원인입니다.

```java
// 스택 오버플로우 예시
public int factorial(int n) {
    return n * factorial(n - 1); // 종료 조건 없음
}
```

## Q4. 원형 큐를 사용하는 이유는?

**A:**
선형 큐에서는 dequeue 연산 후 front가 이동하면서 앞쪽 공간이 낭비되는 문제가 있습니다. 원형 큐는 배열의 끝과 시작을 연결하여 공간을 효율적으로 재사용할 수 있습니다. 모듈로 연산 `(index + 1) % size`을 사용하여 인덱스를 순환시킵니다.

## Q5. 스택과 큐의 실제 사용 사례를 설명해주세요.

**A:**
**스택:**

- 함수 호출 관리 (Call Stack)
- 웹 브라우저 뒤로가기 기능
- 수식 계산 및 괄호 매칭
- Undo/Redo 기능
- DFS 알고리즘

**큐:**

- 프로세스 스케줄링
- BFS 알고리즘
- 프린터 스풀링
- 네트워크 패킷 버퍼링
- 캐시 구현 (LRU)

## Q6. 배열의 캐시 효율성이 좋은 이유는?

**A:**
배열은 메모리에 연속적으로 저장되어 있어 공간 지역성(spatial locality)이 좋습니다. CPU가 한 번에 여러 요소를 캐시로 가져올 수 있어, 인접한 데이터에 접근할 때 캐시 히트율이 높아집니다. 반면 연결리스트는 노드들이 메모리에 흩어져 있어 캐시 미스가 빈번하게 발생합니다.

## Q7. 이중 연결리스트의 장단점은?

**A:**
**장점:**

- 양방향 순회 가능
- 특정 노드 삭제 시 이전 노드 탐색 불필요
- 역순 탐색 가능

**단점:**

- 추가 메모리 필요 (prev 포인터)
- 구현 복잡도 증가
- 포인터 관리 부담 증가

## Q8. 우선순위 큐를 배열로 구현할 때와 힙으로 구현할 때의 차이는?

**A:**
**배열 구현:**

- 삽입: O(1) - 맨 뒤에 추가
- 삭제: O(n) - 최우선순위 요소 탐색 필요

**힙 구현:**

- 삽입: O(log n) - 힙 성질 유지
- 삭제: O(log n) - 루트 제거 후 재정렬

대용량 데이터에서는 힙 구현이 더 효율적입니다. Java의 PriorityQueue는 힙으로 구현되어 있습니다.

## Q9. ArrayList와 LinkedList의 성능 차이는?

**A:**

```java
// ArrayList
- get(index): O(1)
- add(element): O(1) amortized
- add(index, element): O(n)
- remove(index): O(n)

// LinkedList
- get(index): O(n)
- add(element): O(1)
- add(index, element): O(n) - 인덱스 탐색 때문
- remove(index): O(n) - 인덱스 탐색 때문
```

빈번한 인덱스 접근이 필요하면 ArrayList, 빈번한 삽입/삭제가 필요하면 LinkedList를 사용합니다.

## Q10. Java의 Stack 클래스 대신 Deque를 권장하는 이유는?

**A:**
Java의 Stack 클래스는 Vector를 상속받아 동기화 오버헤드가 있고, LIFO가 아닌 다른 방법으로도 접근할 수 있어 진정한 스택이 아닙니다. ArrayDeque는 더 빠르고 메모리 효율적이며, 스택과 큐 기능을 모두 제공합니다.

```java
// 권장하지 않음
Stack<Integer> stack = new Stack<>();

// 권장
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);
int top = stack.pop();
```
