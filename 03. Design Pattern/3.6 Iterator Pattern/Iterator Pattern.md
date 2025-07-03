# 📘 디자인 패턴

# 반복자 패턴 (Iterator Pattern)

## 1. 정의

반복자 패턴은 **컬렉션의 내부 구조를 노출하지 않고 순차적으로 요소에 접근할 수 있는 방법을 제공**하는 디자인 패턴이다. 컬렉션이 배열인지, 리스트인지, 트리인지에 관계없이 **동일한 인터페이스로 모든 요소를 순회**할 수 있게 한다.

즉, "어떻게 저장되어 있든 상관없이, 같은 방식으로 하나씩 접근하자"는 개념이다.

> GoF 정의: 집합 객체의 내부 표현을 노출하지 않고 그 객체의 원소들을 순차적으로 접근할 수 있는 방법을 제공한다.

### 언제 사용하면 좋은가?

**실생활 예시로 이해하기:**
- **TV 채널 돌리기**: 채널이 내부적으로 어떻게 저장되어 있든, "다음 채널" 버튼 하나로 모든 채널 순회
- **컨베이어 벨트**: 공장 직원이 컨베이어 내부 구조를 몰라도 "다음 상품" 요청으로 모든 상품 처리
- **음악 플레이어**: 플레이리스트 내부 구조와 무관하게 "다음 곡" 버튼으로 순차 재생
- **도서관 대출**: 책이 어떤 시스템으로 정리되어 있든 "다음 책" 요청으로 순서대로 대출

**이런 상황에서 Iterator Pattern을 사용하면 좋습니다:**
- 컬렉션의 **내부 구조를 숨기고 싶을 때**
- **다양한 데이터 구조를 동일한 방식으로 순회**하고 싶을 때
- 컬렉션의 구현이 바뀌어도 **클라이언트 코드는 그대로** 유지하고 싶을 때
- **복잡한 데이터 구조**를 간단한 인터페이스로 제공하고 싶을 때

## 2. 구조도

![Iterator Pattern](./images/iterator-structure.png)
- 출처: https://refactoring.guru/ko/design-patterns/iterator

> * `Iterator`: 순회를 위한 인터페이스 (hasNext(), next() 메서드 정의)
> * `ConcreteIterator`: 실제 순회 로직을 구현한 클래스
> * `Collection`: 컬렉션을 나타내는 인터페이스 (iterator() 메서드 제공)
> * `ConcreteCollection`: 실제 컬렉션 구현체

## 3. 예시

### 예: 음악 플레이리스트

```java
// Iterator 인터페이스
interface Iterator<T> {
    boolean hasNext();
    T next();
}

// Collection 인터페이스
interface Collection<T> {
    Iterator<T> iterator();
}

// 음악 클래스
class Song {
    private String title;
    private String artist;
    
    public Song(String title, String artist) {
        this.title = title;
        this.artist = artist;
    }
    
    public void play() {
        System.out.println("재생 중: " + artist + " - " + title);
    }
    
    @Override
    public String toString() {
        return artist + " - " + title;
    }
}

// ConcreteIterator
class PlaylistIterator implements Iterator<Song> {
    private Song[] songs;
    private int position = 0;
    
    public PlaylistIterator(Song[] songs) {
        this.songs = songs;
    }
    
    public boolean hasNext() {
        return position < songs.length && songs[position] != null;
    }
    
    public Song next() {
        return songs[position++];
    }
}

// ConcreteCollection
class MusicPlaylist implements Collection<Song> {
    private Song[] songs;
    private int count = 0;
    
    public MusicPlaylist(int maxSize) {
        this.songs = new Song[maxSize];
    }
    
    public void addSong(String title, String artist) {
        songs[count++] = new Song(title, artist);
    }
    
    public Iterator<Song> iterator() {
        return new PlaylistIterator(songs);
    }
}

// 사용
public class Main {
    public static void main(String[] args) {
        MusicPlaylist playlist = new MusicPlaylist(10);
        playlist.addSong("좋은날", "아이유");
        playlist.addSong("Dynamite", "BTS");
        playlist.addSong("롤린", "브레이브걸스");
        
        Iterator<Song> iter = playlist.iterator();
        while (iter.hasNext()) {
            Song song = iter.next();
            song.play();
        }
        // 재생 중: 아이유 - 좋은날
        // 재생 중: BTS - Dynamite
        // 재생 중: 브레이브걸스 - 롤린
    }
}
```

### 예: 도서관 도서 관리 시스템

```java
// 도서 클래스
class Book {
    private String title;
    private String author;
    
    public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }
    
    @Override
    public String toString() {
        return title + " - " + author;
    }
}

// List 기반 도서관
class ListLibrary implements Collection<Book> {
    private List<Book> books = new ArrayList<>();
    
    public void addBook(String title, String author) {
        books.add(new Book(title, author));
    }
    
    public Iterator<Book> iterator() {
        return new Iterator<Book>() {
            private int index = 0;
            
            public boolean hasNext() {
                return index < books.size();
            }
            
            public Book next() {
                return books.get(index++);
            }
        };
    }
}

// 배열 기반 도서관
class ArrayLibrary implements Collection<Book> {
    private Book[] books;
    private int count = 0;
    
    public ArrayLibrary(int capacity) {
        this.books = new Book[capacity];
    }
    
    public void addBook(String title, String author) {
        books[count++] = new Book(title, author);
    }
    
    public Iterator<Book> iterator() {
        return new Iterator<Book>() {
            private int index = 0;
            
            public boolean hasNext() {
                return index < count;
            }
            
            public Book next() {
                return books[index++];
            }
        };
    }
}

// 사용 - 내부 구조와 무관하게 동일한 방식으로 순회
public class Main {
    public static void printBooks(Collection<Book> library) {
        Iterator<Book> iter = library.iterator();
        while (iter.hasNext()) {
            System.out.println(iter.next());
        }
    }
    
    public static void main(String[] args) {
        ListLibrary listLib = new ListLibrary();
        listLib.addBook("자바의 정석", "남궁성");
        listLib.addBook("Clean Code", "로버트 마틴");
        
        ArrayLibrary arrayLib = new ArrayLibrary(10);
        arrayLib.addBook("이펙티브 자바", "조슈아 블로크");
        arrayLib.addBook("디자인 패턴", "GoF");
        
        System.out.println("=== List 기반 도서관 ===");
        printBooks(listLib);  // 동일한 방식으로 순회
        
        System.out.println("=== Array 기반 도서관 ===");
        printBooks(arrayLib); // 동일한 방식으로 순회
    }
}
```

### 예: 게임 인벤토리 시스템

```java
// 아이템 클래스
class Item {
    private String name;
    private int quantity;
    
    public Item(String name, int quantity) {
        this.name = name;
        this.quantity = quantity;
    }
    
    @Override
    public String toString() {
        return name + " x" + quantity;
    }
}

// 인벤토리 (다양한 순회 방식 제공)
class GameInventory implements Collection<Item> {
    private Map<String, Item> items = new HashMap<>();
    
    public void addItem(String name, int quantity) {
        items.put(name, new Item(name, quantity));
    }
    
    // 기본 순회 (추가된 순서)
    public Iterator<Item> iterator() {
        return items.values().iterator();
    }
    
    // 이름 순 정렬 순회
    public Iterator<Item> alphabeticalIterator() {
        List<Item> sortedItems = new ArrayList<>(items.values());
        sortedItems.sort((a, b) -> a.toString().compareTo(b.toString()));
        return sortedItems.iterator();
    }
    
    // 수량 역순 정렬 순회
    public Iterator<Item> quantityDescIterator() {
        List<Item> sortedItems = new ArrayList<>(items.values());
        sortedItems.sort((a, b) -> Integer.compare(b.quantity, a.quantity));
        return sortedItems.iterator();
    }
}

// 사용
public class Main {
    public static void main(String[] args) {
        GameInventory inventory = new GameInventory();
        inventory.addItem("체력 포션", 5);
        inventory.addItem("마나 포션", 12);
        inventory.addItem("검", 1);
        
        System.out.println("=== 기본 순회 ===");
        Iterator<Item> basicIter = inventory.iterator();
        while (basicIter.hasNext()) {
            System.out.println(basicIter.next());
        }
        
        System.out.println("=== 이름순 정렬 ===");
        Iterator<Item> alphaIter = inventory.alphabeticalIterator();
        while (alphaIter.hasNext()) {
            System.out.println(alphaIter.next());
        }
        
        System.out.println("=== 수량 많은 순 ===");
        Iterator<Item> quantityIter = inventory.quantityDescIterator();
        while (quantityIter.hasNext()) {
            System.out.println(quantityIter.next());
        }
    }
}
```

## 4. 다양한 구현 방식

### 내부 이터레이터 (Internal Iterator)

```java
// 컬렉션이 순회 로직을 직접 제공
class NumberList {
    private List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    
    public void forEach(Consumer<Integer> action) {
        for (Integer num : numbers) {
            action.accept(num);
        }
    }
}

// 사용
NumberList list = new NumberList();
list.forEach(System.out::println); // 람다로 처리 로직 전달
```

### 외부 이터레이터 (External Iterator)

```java
// 클라이언트가 순회를 직접 제어
Iterator<Integer> iter = list.iterator();
while (iter.hasNext()) {
    Integer num = iter.next();
    if (num % 2 == 0) {
        System.out.println("짝수: " + num);
    }
}
```

### Java Stream API 활용

```java
// 함수형 프로그래밍 스타일
inventory.iterator()
    .stream()
    .filter(item -> item.getQuantity() > 5)
    .forEach(System.out::println);
```

## 5. 장단점

| 장점 | 설명 |
|------|------|
| 단일 책임 원칙 | 순회 로직을 별도 클래스로 분리 |
| 일관된 인터페이스 | 다양한 컬렉션을 동일한 방식으로 순회 |
| 내부 구조 은닉 | 컬렉션의 구현 세부사항을 숨김 |
| 다양한 순회 방식 | 하나의 컬렉션에 여러 순회 방식 제공 가능 |

| 단점 | 설명 |
|------|------|
| 복잡성 증가 | 간단한 컬렉션에는 오버엔지니어링 가능성 |
| 메모리 사용량 | Iterator 객체가 추가로 생성됨 |
| 성능 오버헤드 | 직접 접근보다 느릴 수 있음 |
| 동시성 문제 | 순회 중 컬렉션 변경 시 예외 발생 가능 |

## 6. 활용 예시

* Java의 Collection Framework (ArrayList, LinkedList 등)
* 데이터베이스 결과 집합 순회 (ResultSet)
* 파일 시스템 디렉토리 탐색
* 웹 페이지네이션 처리
* 게임의 AI 경로 탐색

## Iterator Pattern을 사용하지 말아야 할 경우

* 컬렉션이 매우 단순하고 순회 방식이 하나뿐인 경우
* 성능이 매우 중요하고 직접 접근이 훨씬 빠른 경우
* 컬렉션의 크기가 작고 고정되어 있어 단순 배열 접근이 더 적합한 경우
* 순회보다는 랜덤 액세스가 주요 사용 패턴인 경우

> 예: 3-4개 원소의 고정 배열을 한 방향으로만 순회한다면 단순 for문이 더 직관적

## 유사 패턴과 비교

### Composite Pattern (복합체 패턴)
트리 구조의 객체들을 **동일한 인터페이스로 다루는** 패턴입니다. 단일 객체와 복합 객체를 구분하지 않고 처리할 수 있게 합니다.

**예시**: 파일 시스템에서 파일과 폴더를 같은 방식으로 처리
**차이점**: Composite은 "구조를 통일", Iterator는 "순회 방식을 통일"

### Visitor Pattern (방문자 패턴)
객체 구조를 변경하지 않고 **새로운 연산을 추가**하는 패턴입니다. 데이터 구조와 연산을 분리합니다.

**예시**: AST(추상 구문 트리) 노드들에 대한 다양한 연산 (코드 생성, 최적화 등)
**차이점**: Visitor는 "연산 추가", Iterator는 "순회 제공"

| 항목 | Iterator Pattern | Composite Pattern | Visitor Pattern |
|------|------------------|-------------------|-----------------|
| 목적 | 컬렉션 순회 방식 통일 | 단일/복합 객체 처리 통일 | 새로운 연산 추가 |
| 사용 시점 | 다양한 컬렉션 순회 | 트리 구조 처리 | 객체에 연산 추가 |
| 핵심 기능 | hasNext(), next() | add(), remove(), getChild() | visit(), accept() |
| 예시 | 플레이리스트 순회 | 파일/폴더 시스템 | 컴파일러 AST 처리 |

> 핵심 차이: Iterator는 "어떻게 순회할 것인가", Composite은 "어떻게 구조화할 것인가", Visitor는 "어떤 연산을 수행할 것인가"

## 핵심 요약

* Iterator Pattern은 **컬렉션의 내부 구조와 무관하게 동일한 방식으로 순회**할 수 있게 하는 패턴이다.
* `hasNext()`와 `next()` 메서드로 **표준화된 순회 인터페이스**를 제공한다.
* 컬렉션의 구현이 바뀌어도 **클라이언트 코드는 변경되지 않는다**.
* Java의 **for-each 문과 Collection Framework**의 기반이 되는 패턴이다.

## 용어 설명

* **Iterator (반복자)**: 컬렉션의 요소들을 순차적으로 접근하기 위한 인터페이스를 제공하는 객체. 현재 위치 상태를 유지하며 다음 요소로의 이동을 관리
* **Collection (컬렉션)**: 다수의 요소를 저장하고 관리하는 자료구조의 추상화. 배열, 링크드리스트, 트리 등 다양한 구현체를 가질 수 있음
* **순회 (Traversal)**: 자료구조의 모든 요소를 체계적으로 방문하는 알고리즘적 과정
* **캡슐화 (Encapsulation)**: 객체의 내부 구현을 외부로부터 숨기고 공개 인터페이스만을 통해 접근을 허용하는 객체지향 원칙
* **추상화 (Abstraction)**: 복잡한 시스템의 핵심적인 개념이나 기능만을 모델링하여 불필요한 세부사항을 제거하는 과정
* **느슨한 결합 (Loose Coupling)**: 시스템 구성 요소 간의 의존도를 최소화하여 한 부분의 변경이 다른 부분에 미치는 영향을 줄이는 설계 원칙
* **내부 이터레이터 (Internal Iterator)**: 컬렉션 객체가 순회 제어권을 가지며, 클라이언트는 콜백 함수를 제공하는 방식
* **외부 이터레이터 (External Iterator)**: 클라이언트가 순회 제어권을 가지며, 명시적으로 다음 요소를 요청하는 방식
* **Fail-Fast**: 시스템 오류나 예외 상황 발생 시 즉시 실행을 중단하여 데이터 무결성을 보장하는 메커니즘
* **지연 로딩 (Lazy Loading)**: 실제 데이터가 필요한 시점까지 로딩을 지연시켜 메모리 사용량과 초기화 시간을 최적화하는 기법
* **상태 유지 (Stateful)**: 객체가 이전 호출이나 상호작용의 결과를 내부에 저장하고 다음 동작에 영향을 주는 특성
* **ConcreteIterator**: Iterator 인터페이스의 구체적인 구현체로, 특정 컬렉션 타입에 대한 실제 순회 로직을 포함