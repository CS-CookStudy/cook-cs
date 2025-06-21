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

### 2-1. 단일 연결 리스트 (Singly Linked List)

#### 개념

- 각 노드가 **데이터**와 **다음 노드를 가리키는 포인터** 하나만 가지는 연결 리스트
- 한 방향으로만 순회 가능 (head → tail)
- 가장 기본적이고 단순한 연결 리스트 형태

#### 특징

- **단방향 순회**: head에서 시작해서 tail까지만 이동 가능
- **메모리 효율성**: 포인터 하나만 저장하므로 메모리 사용량이 적음
- **역방향 접근 불가**: 이전 노드로 돌아가려면 처음부터 다시 순회해야 함

#### 구현

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
    private int size;

    public SinglyLinkedList() {
        this.head = null;
        this.size = 0;
    }

    // 맨 앞에 삽입
    public void insertFirst(int data) {
        ListNode newNode = new ListNode(data);
        newNode.next = head;
        head = newNode;
        size++;
    }

    // 맨 뒤에 삽입
    public void insertLast(int data) {
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
        size++;
    }

    // 특정 위치에 삽입
    public void insertAt(int index, int data) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }

        if (index == 0) {
            insertFirst(data);
            return;
        }

        ListNode newNode = new ListNode(data);
        ListNode current = head;
        for (int i = 0; i < index - 1; i++) {
            current = current.next;
        }
        newNode.next = current.next;
        current.next = newNode;
        size++;
    }

    // 값으로 삭제
    public boolean delete(int data) {
        if (head == null) return false;

        if (head.data == data) {
            head = head.next;
            size--;
            return true;
        }

        ListNode current = head;
        while (current.next != null && current.next.data != data) {
            current = current.next;
        }

        if (current.next != null) {
            current.next = current.next.next;
            size--;
            return true;
        }
        return false;
    }

    // 인덱스로 삭제
    public boolean deleteAt(int index) {
        if (index < 0 || index >= size) {
            return false;
        }

        if (index == 0) {
            head = head.next;
            size--;
            return true;
        }

        ListNode current = head;
        for (int i = 0; i < index - 1; i++) {
            current = current.next;
        }
        current.next = current.next.next;
        size--;
        return true;
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

    // 특정 인덱스의 값 반환
    public int get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }

        ListNode current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.data;
    }

    // 크기 반환
    public int size() {
        return size;
    }

    // 비어있는지 확인
    public boolean isEmpty() {
        return head == null;
    }

    // 출력
    public void display() {
        ListNode current = head;
        while (current != null) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
}
```

#### 장단점

**장점:**

- 구현이 간단하고 직관적
- 메모리 사용량이 적음
- 삽입/삭제가 O(1) (위치를 알고 있을 때)

**단점:**

- 역방향 순회 불가능
- 특정 노드 삭제 시 이전 노드를 찾기 위해 처음부터 순회 필요
- 임의 위치 접근이 O(n)

### 2-2. 이중 연결 리스트 (Doubly Linked List)

#### 개념

- 각 노드가 **데이터**, **다음 노드 포인터(next)**, **이전 노드 포인터(prev)** 를 가지는 연결 리스트
- 양방향 순회 가능 (head ↔ tail)
- 더 유연한 탐색과 조작이 가능

#### 특징

- **양방향 순회**: 앞뒤로 자유롭게 이동 가능
- **효율적인 삭제**: 특정 노드 삭제 시 이전 노드 탐색 불필요
- **head와 tail 포인터**: 양쪽 끝에서의 삽입/삭제가 효율적

#### 구현

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
    private int size;

    public DoublyLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    // 맨 앞에 삽입
    public void insertFirst(int data) {
        DoublyListNode newNode = new DoublyListNode(data);

        if (head == null) {
            head = tail = newNode;
        } else {
            newNode.next = head;
            head.prev = newNode;
            head = newNode;
        }
        size++;
    }

    // 맨 뒤에 삽입
    public void insertLast(int data) {
        DoublyListNode newNode = new DoublyListNode(data);

        if (head == null) {
            head = tail = newNode;
        } else {
            tail.next = newNode;
            newNode.prev = tail;
            tail = newNode;
        }
        size++;
    }

    // 특정 위치에 삽입
    public void insertAt(int index, int data) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }

        if (index == 0) {
            insertFirst(data);
            return;
        }
        if (index == size) {
            insertLast(data);
            return;
        }

        DoublyListNode newNode = new DoublyListNode(data);
        DoublyListNode current = getNodeAt(index);

        newNode.next = current;
        newNode.prev = current.prev;
        current.prev.next = newNode;
        current.prev = newNode;
        size++;
    }

    // 값으로 삭제
    public boolean delete(int data) {
        DoublyListNode current = head;

        while (current != null) {
            if (current.data == data) {
                deleteNode(current);
                return true;
            }
            current = current.next;
        }
        return false;
    }

    // 인덱스로 삭제
    public boolean deleteAt(int index) {
        if (index < 0 || index >= size) {
            return false;
        }

        DoublyListNode nodeToDelete = getNodeAt(index);
        deleteNode(nodeToDelete);
        return true;
    }

    // 특정 노드 삭제 (내부 메서드)
    private void deleteNode(DoublyListNode node) {
        if (node.prev != null) {
            node.prev.next = node.next;
        } else {
            head = node.next;
        }

        if (node.next != null) {
            node.next.prev = node.prev;
        } else {
            tail = node.prev;
        }

        // 메모리 누수 방지
        node.prev = null;
        node.next = null;
        size--;
    }

    // 특정 인덱스의 노드 반환 (내부 메서드)
    private DoublyListNode getNodeAt(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }

        DoublyListNode current;

        // 중간점을 기준으로 앞/뒤에서 탐색 시작 (최적화)
        if (index < size / 2) {
            current = head;
            for (int i = 0; i < index; i++) {
                current = current.next;
            }
        } else {
            current = tail;
            for (int i = size - 1; i > index; i--) {
                current = current.prev;
            }
        }
        return current;
    }

    // 특정 인덱스의 값 반환
    public int get(int index) {
        return getNodeAt(index).data;
    }

    // 탐색
    public DoublyListNode search(int data) {
        DoublyListNode current = head;
        while (current != null) {
            if (current.data == data) {
                return current;
            }
            current = current.next;
        }
        return null;
    }

    // 크기 반환
    public int size() {
        return size;
    }

    // 비어있는지 확인
    public boolean isEmpty() {
        return head == null;
    }

    // 정방향 출력
    public void displayForward() {
        DoublyListNode current = head;
        while (current != null) {
            System.out.print(current.data + " <-> ");
            current = current.next;
        }
        System.out.println("null");
    }

    // 역방향 출력
    public void displayBackward() {
        DoublyListNode current = tail;
        while (current != null) {
            System.out.print(current.data + " <-> ");
            current = current.prev;
        }
        System.out.println("null");
    }
}
```

#### 장단점

**장점:**

- 양방향 순회 가능
- 특정 노드 삭제가 효율적 (이전 노드 탐색 불필요)
- 역순 정렬이나 역방향 작업에 유리
- 양쪽 끝에서의 삽입/삭제가 O(1)

**단점:**

- 추가 메모리 필요 (prev 포인터)
- 구현 복잡도 증가
- 포인터 연결 관리가 복잡 (실수하기 쉬움)

### 2-3. 원형 연결 리스트 (Circular Linked List)

#### 개념

- 마지막 노드가 첫 번째 노드를 가리켜서 **원형 구조**를 만드는 연결 리스트
- 끝이 없는 순환 구조
- 단일 원형과 이중 원형 두 가지 형태 존재

#### 특징

- **순환 구조**: 마지막 노드의 next가 head를 가리킴
- **끝이 없음**: 계속해서 순회 가능
- **시작점이 임의적**: 어느 노드에서든 시작 가능

#### 구현

```java
class CircularLinkedList {
    private ListNode head;
    private int size;

    public CircularLinkedList() {
        this.head = null;
        this.size = 0;
    }

    // 맨 앞에 삽입
    public void insertFirst(int data) {
        ListNode newNode = new ListNode(data);

        if (head == null) {
            head = newNode;
            newNode.next = head; // 자기 자신을 가리킴
        } else {
            // 마지막 노드를 찾아서 연결 업데이트
            ListNode last = getLastNode();
            newNode.next = head;
            last.next = newNode;
            head = newNode;
        }
        size++;
    }

    // 맨 뒤에 삽입
    public void insertLast(int data) {
        ListNode newNode = new ListNode(data);

        if (head == null) {
            head = newNode;
            newNode.next = head;
        } else {
            ListNode last = getLastNode();
            last.next = newNode;
            newNode.next = head;
        }
        size++;
    }

    // 특정 위치에 삽입
    public void insertAt(int index, int data) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }

        if (index == 0) {
            insertFirst(data);
            return;
        }

        ListNode newNode = new ListNode(data);
        ListNode current = head;

        for (int i = 0; i < index - 1; i++) {
            current = current.next;
        }

        newNode.next = current.next;
        current.next = newNode;
        size++;
    }

    // 값으로 삭제
    public boolean delete(int data) {
        if (head == null) return false;

        // 노드가 하나뿐인 경우
        if (size == 1 && head.data == data) {
            head = null;
            size--;
            return true;
        }

        // head 노드를 삭제하는 경우
        if (head.data == data) {
            ListNode last = getLastNode();
            head = head.next;
            last.next = head;
            size--;
            return true;
        }

        // 다른 노드를 삭제하는 경우
        ListNode current = head;
        do {
            if (current.next.data == data) {
                current.next = current.next.next;
                size--;
                return true;
            }
            current = current.next;
        } while (current != head);

        return false;
    }

    // 인덱스로 삭제
    public boolean deleteAt(int index) {
        if (index < 0 || index >= size) {
            return false;
        }

        if (index == 0) {
            if (size == 1) {
                head = null;
            } else {
                ListNode last = getLastNode();
                head = head.next;
                last.next = head;
            }
            size--;
            return true;
        }

        ListNode current = head;
        for (int i = 0; i < index - 1; i++) {
            current = current.next;
        }
        current.next = current.next.next;
        size--;
        return true;
    }

    // 탐색
    public ListNode search(int data) {
        if (head == null) return null;

        ListNode current = head;
        do {
            if (current.data == data) {
                return current;
            }
            current = current.next;
        } while (current != head);

        return null;
    }

    // 특정 인덱스의 값 반환
    public int get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index out of bounds");
        }

        ListNode current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.data;
    }

    // 마지막 노드 반환 (내부 메서드)
    private ListNode getLastNode() {
        if (head == null) return null;

        ListNode current = head;
        while (current.next != head) {
            current = current.next;
        }
        return current;
    }

    // 크기 반환
    public int size() {
        return size;
    }

    // 비어있는지 확인
    public boolean isEmpty() {
        return head == null;
    }

    // 출력 (무한 루프 방지)
    public void display() {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }

        ListNode current = head;
        do {
            System.out.print(current.data + " -> ");
            current = current.next;
        } while (current != head);
        System.out.println("(back to " + head.data + ")");
    }

    // n번 순회하며 출력
    public void displayNTimes(int n) {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }

        ListNode current = head;
        for (int i = 0; i < n; i++) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("...");
    }
}
```

#### 장단점

**장점:**

- 모든 노드에서 모든 다른 노드 접근 가능
- 라운드 로빈 스케줄링에 적합
- 큐와 스택을 하나의 구조로 구현 가능
- NULL 포인터 처리 불필요

**단점:**

- 무한 루프 위험 (종료 조건 관리 중요)
- 구현이 복잡
- 디버깅이 어려움
- 메모리 누수 위험 (순환 참조)

### 배열 vs 연결 리스트 비교

| 특성            | 배열          | 연결 리스트       |
| --------------- | ------------- | ----------------- |
| 메모리          | 연속적        | 비연속적          |
| 접근 방식       | Random Access | Sequential Access |
| 접근 시간       | O(1)          | O(n)              |
| 삽입/삭제       | O(n)          | O(1)              |
| 메모리 오버헤드 | 없음          | 포인터 저장 공간  |
| 캐시 효율성     | 좋음          | 나쁨              |

### 연결 리스트 종류별 비교표

| 특성               | 단일 연결 리스트     | 이중 연결 리스트 | 원형 연결 리스트          |
| ------------------ | -------------------- | ---------------- | ------------------------- |
| **포인터 수**      | 1개 (next)           | 2개 (next, prev) | 1개 (next, 마지막→첫번째) |
| **순회 방향**      | 단방향               | 양방향           | 단방향 (순환)             |
| **메모리 사용**    | 적음                 | 많음             | 적음                      |
| **구현 복잡도**    | 낮음                 | 높음             | 중간                      |
| **역방향 접근**    | 불가능               | 가능             | 불가능 (전체 순회 필요)   |
| **양끝 삽입/삭제** | head O(1), tail O(n) | 양쪽 모두 O(1)   | head O(1), tail O(n)      |

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

    public boolean isFull() {
        return top == maxSize - 1;
    }

    public int size() {
        return top + 1;
    }
}
```

**연결 리스트 기반 구현:**

```java
class LinkedStack {
    private ListNode top;
    private int size;

    public LinkedStack() {
        this.top = null;
        this.size = 0;
    }

    public void push(int data) {
        ListNode newNode = new ListNode(data);
        newNode.next = top;
        top = newNode;
        size++;
    }

    public int pop() {
        if (top == null) {
            throw new RuntimeException("Stack Underflow");
        }
        int data = top.data;
        top = top.next;
        size--;
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

    public int size() {
        return size;
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

    public boolean isEmpty() {
        return front == -1 || front > rear;
    }

    public boolean isFull() {
        return rear == maxSize - 1;
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

    public int size() {
        return size;
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

### 연결 리스트 기반 큐 구현

```java
class LinkedQueue {
    private ListNode front;
    private ListNode rear;
    private int size;

    public LinkedQueue() {
        this.front = null;
        this.rear = null;
        this.size = 0;
    }

    public void enqueue(int data) {
        ListNode newNode = new ListNode(data);

        if (rear == null) {
            front = rear = newNode;
        } else {
            rear.next = newNode;
            rear = newNode;
        }
        size++;
    }

    public int dequeue() {
        if (front == null) {
            throw new RuntimeException("Queue is Empty");
        }

        int data = front.data;
        front = front.next;

        if (front == null) {
            rear = null;
        }

        size--;
        return data;
    }

    public int peek() {
        if (front == null) {
            throw new RuntimeException("Queue is Empty");
        }
        return front.data;
    }

    public boolean isEmpty() {
        return front == null;
    }

    public int size() {
        return size;
    }

    public void display() {
        ListNode current = front;
        System.out.print("Front -> ");
        while (current != null) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("Rear");
    }
}
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

## Q3. 원형 큐를 사용하는 이유는?

**A:**
선형 큐에서는 dequeue 연산 후 front가 이동하면서 앞쪽 공간이 낭비되는 문제가 있습니다. 원형 큐는 배열의 끝과 시작을 연결하여 공간을 효율적으로 재사용할 수 있습니다. 모듈로 연산 `(index + 1) % size`을 사용하여 인덱스를 순환시킵니다.

## Q4. 스택과 큐의 실제 사용 사례를 설명해주세요.

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

## Q5. 배열의 캐시 효율성이 좋은 이유는?

**A:**
배열은 메모리에 연속적으로 저장되어 있어 공간 지역성(spatial locality)이 좋습니다. CPU가 한 번에 여러 요소를 캐시로 가져올 수 있어, 인접한 데이터에 접근할 때 캐시 히트율이 높아집니다. 반면 연결리스트는 노드들이 메모리에 흩어져 있어 캐시 미스가 빈번하게 발생합니다.

## Q6. 이중 연결리스트의 장단점은?

**A:**
**장점:**

- 양방향 순회 가능
- 특정 노드 삭제 시 이전 노드 탐색 불필요
- 역순 탐색 가능

**단점:**

- 추가 메모리 필요 (prev 포인터)
- 구현 복잡도 증가
- 포인터 관리 부담 증가

## Q7. 우선순위 큐를 배열로 구현할 때와 힙으로 구현할 때의 차이는?

**A:**
**배열 구현:**

- 삽입: O(1) - 맨 뒤에 추가
- 삭제: O(n) - 최우선순위 요소 탐색 필요

**힙 구현:**

- 삽입: O(log n) - 힙 성질 유지
- 삭제: O(log n) - 루트 제거 후 재정렬

대용량 데이터에서는 힙 구현이 더 효율적입니다. Java의 PriorityQueue는 힙으로 구현되어 있습니다.

## Q8. ArrayList와 LinkedList의 성능 차이는?

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

## Q9. Java의 Stack 클래스 대신 Deque를 권장하는 이유는?

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

## Q10. 원형 연결 리스트에서 무한 루프를 방지하는 방법은?

**A:**
원형 연결 리스트에서는 NULL 포인터가 없으므로 종료 조건을 명확히 설정해야 합니다:

```java
// ❌ 무한 루프 위험
public void printAll() {
    ListNode current = head;
    while (current != null) { // null이 없으므로 무한 루프!
        System.out.println(current.data);
        current = current.next;
    }
}

// ✅ 올바른 구현
public void printAll() {
    if (head == null) return;

    ListNode current = head;
    do {
        System.out.println(current.data);
        current = current.next;
    } while (current != head); // head로 돌아올 때까지
}
```

## Q11. 언제 단일/이중/원형 연결 리스트를 각각 사용해야 하나요?

**A:**

**단일 연결 리스트:**

- 메모리 사용량을 최소화하고 싶을 때
- 스택이나 간단한 큐 구현
- 단순한 순차 처리만 필요할 때

**이중 연결 리스트:**

- 양방향 순회가 자주 필요할 때 (브라우저 히스토리)
- 임의 위치에서의 삭제가 빈번할 때
- 덱(Deque) 기능이 필요할 때

**원형 연결 리스트:**

- 라운드 로빈 알고리즘 구현
- 게임에서 플레이어 턴 관리
- 순환적인 처리가 필요할 때

## Q12. 메모리 누수를 방지하는 방법은?

**A:**
연결 리스트에서 노드 삭제 시 참조를 명시적으로 해제해야 합니다:

```java
// 이중 연결 리스트 노드 삭제 시
public void deleteNode(DoublyListNode node) {
    // 연결 해제
    if (node.prev != null) node.prev.next = node.next;
    if (node.next != null) node.next.prev = node.prev;

    // 메모리 누수 방지를 위한 참조 해제
    node.prev = null;
    node.next = null;
}
```

특히 원형 연결 리스트에서는 순환 참조로 인한 메모리 누수에 주의해야 합니다.

# 시간 복잡도 요약표

| 자료구조            | 접근 | 탐색 | 삽입     | 삭제     | 공간복잡도 |
| ------------------- | ---- | ---- | -------- | -------- | ---------- |
| **배열**            | O(1) | O(n) | O(n)     | O(n)     | O(n)       |
| **동적배열**        | O(1) | O(n) | O(1)\*   | O(n)     | O(n)       |
| **단일 연결리스트** | O(n) | O(n) | O(1)\*\* | O(1)\*\* | O(n)       |
| **이중 연결리스트** | O(n) | O(n) | O(1)\*\* | O(1)\*\* | O(n)       |
| **스택**            | O(n) | O(n) | O(1)     | O(1)     | O(n)       |
| **큐**              | O(n) | O(n) | O(1)     | O(1)     | O(n)       |

\* Amortized (평균적으로)  
\*\* 위치를 알고 있을 때

---
