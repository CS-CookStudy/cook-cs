# 단순 정렬 알고리즘 (Simple Sorting Algorithms)

## 1. 버블 정렬 (Bubble Sort)

### 알고리즘 원리
인접한 두 원소를 비교하여 크기 순서대로 교환하는 과정을 반복한다. 마치 거품이 물 위로 올라오듯이 큰 값들이 배열의 뒤쪽으로 이동하는 모습에서 버블 정렬이라는 이름이 붙었다.

**동작 과정:**
1. 배열의 첫 번째 원소부터 인접한 원소와 비교
2. 앞의 원소가 뒤의 원소보다 크면 교환
3. 배열 끝까지 이 과정을 반복 (한 번의 패스로 가장 큰 값이 맨 뒤로 이동)
4. 정렬될 때까지 패스를 반복

### 시간/공간 복잡도
- **시간 복잡도**:
    - 최악의 경우: O(n²) - **역순으로 정렬된 경우 [5,4,3,2,1]**
      > 이유: n번의 패스 × 평균 n/2번의 비교 = n²/2 ≈ O(n²)
    - 평균의 경우: O(n²)
    - 최선의 경우: O(n) - **이미 정렬된 경우 [1,2,3,4,5]** (개선된 버전)
      > 이유: 첫 패스에서 교환이 없으면 즉시 종료
- **공간 복잡도**: O(1) - **제자리 정렬**
  > 이유: 임시 변수 몇 개만 사용, 입력 크기와 무관하게 일정한 메모리 사용

### 구현

#### Python
```python
def bubble_sort(arr):
    n = len(arr)
    
    for i in range(n):
        # 마지막 i개 원소는 이미 정렬됨
        swapped = False
        
        for j in range(0, n - i - 1):
            # 인접한 원소 비교
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        
        # 교환이 일어나지 않으면 정렬 완료
        if not swapped:
            break
    
    return arr

# 사용 예시
numbers = [64, 34, 25, 12, 22, 11, 90]
print("정렬 전:", numbers)
bubble_sort(numbers)
print("정렬 후:", numbers)
```

#### C++
```cpp
#include <iostream>
#include <vector>

void bubbleSort(std::vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 0; i < n; i++) {
        bool swapped = false;
        
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // 원소 교환
                std::swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        
        // 교환이 일어나지 않으면 정렬 완료
        if (!swapped) break;
    }
}

// 사용 예시
int main() {
    std::vector<int> numbers = {64, 34, 25, 12, 22, 11, 90};
    
    std::cout << "정렬 전: ";
    for (int num : numbers) std::cout << num << " ";
    
    bubbleSort(numbers);
    
    std::cout << "\n정렬 후: ";
    for (int num : numbers) std::cout << num << " ";
    
    return 0;
}
```

#### Java
```java
import java.util.Arrays;

public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        
        for (int i = 0; i < n; i++) {
            boolean swapped = false;
            
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // 원소 교환
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            
            // 교환이 일어나지 않으면 정렬 완료
            if (!swapped) break;
        }
    }
    
    // 사용 예시
    public static void main(String[] args) {
        int[] numbers = {64, 34, 25, 12, 22, 11, 90};
        
        System.out.println("정렬 전: " + Arrays.toString(numbers));
        bubbleSort(numbers);
        System.out.println("정렬 후: " + Arrays.toString(numbers));
    }
}
```

### 특징 및 쓰면 좋은 경우

**장점:**
- 구현이 매우 간단하고 이해하기 쉬움
- 안정 정렬 (같은 값의 원소들의 상대적 순서가 유지됨)
- 제자리 정렬 (추가 메모리 공간이 거의 필요 없음)
- 정렬 과정을 시각적으로 이해하기 좋음

**단점:**
- 시간 복잡도가 O(n²)로 비효율적
- 원소 교환 횟수가 많아 실제 성능이 느림

**쓰면 좋은 경우:**
- 교육 목적으로 정렬 알고리즘의 기본 개념을 설명할 때
- 데이터 크기가 매우 작을 때 (10개 이하)
- 이미 거의 정렬된 데이터에서 간단한 정리가 필요할 때
- 메모리가 극도로 제한적인 환경에서

---

## 2. 선택 정렬 (Selection Sort)

### 알고리즘 원리
전체 배열에서 가장 작은(또는 큰) 원소를 찾아 첫 번째 위치에 놓고, 나머지 배열에서 다시 가장 작은 원소를 찾아 두 번째 위치에 놓는 과정을 반복한다.

**동작 과정:**
1. 배열에서 최솟값을 찾아 첫 번째 원소와 교환
2. 두 번째 원소부터 끝까지에서 최솟값을 찾아 두 번째 원소와 교환
3. 이 과정을 배열이 정렬될 때까지 반복

### 시간/공간 복잡도
- **시간 복잡도**:
    - 모든 경우: O(n²) - **최선/평균/최악 모두 동일**
      > 이유: 항상 전체 배열을 탐색하여 최솟값을 찾아야 함 (n + (n-1) + ... + 1 = n(n+1)/2)
        - 최악의 경우: **역순으로 정렬된 경우 [5,4,3,2,1]**
        - 최선의 경우: **이미 정렬된 경우 [1,2,3,4,5]** (성능 차이 없음)
- **공간 복잡도**: O(1) - **제자리 정렬**
  > 이유: 최솟값 인덱스를 저장할 변수 하나만 추가로 사용

### 구현

#### Python
```python
def selection_sort(arr):
    n = len(arr)
    
    for i in range(n):
        # i번째부터 끝까지에서 최솟값의 인덱스 찾기
        min_idx = i
        
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        
        # 최솟값을 현재 위치로 교환
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    
    return arr

# 사용 예시
numbers = [64, 34, 25, 12, 22, 11, 90]
print("정렬 전:", numbers)
selection_sort(numbers)
print("정렬 후:", numbers)
```

#### C++
```cpp
#include <iostream>
#include <vector>

void selectionSort(std::vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 0; i < n; i++) {
        int min_idx = i;
        
        // 최솟값 찾기
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        
        // 최솟값을 현재 위치로 교환
        if (min_idx != i) {
            std::swap(arr[i], arr[min_idx]);
        }
    }
}

// 사용 예시
int main() {
    std::vector<int> numbers = {64, 34, 25, 12, 22, 11, 90};
    
    std::cout << "정렬 전: ";
    for (int num : numbers) std::cout << num << " ";
    
    selectionSort(numbers);
    
    std::cout << "\n정렬 후: ";
    for (int num : numbers) std::cout << num << " ";
    
    return 0;
}
```

#### Java
```java
import java.util.Arrays;

public class SelectionSort {
    public static void selectionSort(int[] arr) {
        int n = arr.length;
        
        for (int i = 0; i < n; i++) {
            int min_idx = i;
            
            // 최솟값 찾기
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[min_idx]) {
                    min_idx = j;
                }
            }
            
            // 최솟값을 현재 위치로 교환
            if (min_idx != i) {
                int temp = arr[i];
                arr[i] = arr[min_idx];
                arr[min_idx] = temp;
            }
        }
    }
    
    // 사용 예시
    public static void main(String[] args) {
        int[] numbers = {64, 34, 25, 12, 22, 11, 90};
        
        System.out.println("정렬 전: " + Arrays.toString(numbers));
        selectionSort(numbers);
        System.out.println("정렬 후: " + Arrays.toString(numbers));
    }
}
```

### 특징 및 쓰면 좋은 경우

**장점:**
- 구현이 간단하고 직관적
- 교환 횟수가 적음 (최대 n-1번)
- 제자리 정렬로 메모리 효율적
- 선택 과정이 명확해 이해하기 쉬움

**단점:**
- 시간 복잡도가 항상 O(n²)
- 불안정 정렬 (같은 값의 상대적 순서가 바뀔 수 있음)
- 이미 정렬된 배열에도 동일한 시간이 소요됨

**쓰면 좋은 경우:**
- 교환 비용이 비싼 상황에서 (교환 횟수를 최소화하고 싶을 때)
- 메모리가 제한적이고 단순한 정렬이 필요할 때
- 작은 크기의 배열을 정렬할 때
- 교육 목적으로 정렬의 기본 개념을 설명할 때

---

## 3. 삽입 정렬 (Insertion Sort)

### 알고리즘 원리
배열을 정렬된 부분과 정렬되지 않은 부분으로 나누고, 정렬되지 않은 부분의 원소를 하나씩 정렬된 부분의 적절한 위치에 삽입한다. 카드를 정렬하는 방식과 유사하다.

**동작 과정:**
1. 두 번째 원소부터 시작 (첫 번째 원소는 이미 정렬된 것으로 간주)
2. 현재 원소를 정렬된 부분과 비교하여 적절한 위치를 찾음
3. 해당 위치에 현재 원소를 삽입 (필요시 다른 원소들을 이동)
4. 모든 원소에 대해 반복

### 시간/공간 복잡도
- **시간 복잡도**:
    - 최악의 경우: O(n²) - **역순으로 정렬된 경우 [5,4,3,2,1]**
      > 이유: 각 원소를 삽입할 때마다 이전 모든 원소와 비교 (1+2+...+(n-1) = n(n-1)/2)
    - 평균의 경우: O(n²)
    - 최선의 경우: O(n) - **이미 정렬된 경우 [1,2,3,4,5]**
      > 이유: 각 원소당 1번의 비교만 필요 (바로 다음 원소가 제자리)
- **공간 복잡도**: O(1) - **제자리 정렬**
  > 이유: 삽입할 원소를 저장할 key 변수와 인덱스 변수만 사용

### 구현

#### Python
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]  # 삽입할 원소
        j = i - 1     # 정렬된 부분의 마지막 인덱스
        
        # key보다 큰 원소들을 한 칸씩 뒤로 이동
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        
        # key를 적절한 위치에 삽입
        arr[j + 1] = key
    
    return arr

# 사용 예시
numbers = [64, 34, 25, 12, 22, 11, 90]
print("정렬 전:", numbers)
insertion_sort(numbers)
print("정렬 후:", numbers)
```

#### C++
```cpp
#include <iostream>
#include <vector>

void insertionSort(std::vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        
        // key보다 큰 원소들을 한 칸씩 뒤로 이동
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        
        // key를 적절한 위치에 삽입
        arr[j + 1] = key;
    }
}

// 사용 예시
int main() {
    std::vector<int> numbers = {64, 34, 25, 12, 22, 11, 90};
    
    std::cout << "정렬 전: ";
    for (int num : numbers) std::cout << num << " ";
    
    insertionSort(numbers);
    
    std::cout << "\n정렬 후: ";
    for (int num : numbers) std::cout << num << " ";
    
    return 0;
}
```

#### Java
```java
import java.util.Arrays;

public class InsertionSort {
    public static void insertionSort(int[] arr) {
        int n = arr.length;
        
        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;
            
            // key보다 큰 원소들을 한 칸씩 뒤로 이동
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            
            // key를 적절한 위치에 삽입
            arr[j + 1] = key;
        }
    }
    
    // 사용 예시
    public static void main(String[] args) {
        int[] numbers = {64, 34, 25, 12, 22, 11, 90};
        
        System.out.println("정렬 전: " + Arrays.toString(numbers));
        insertionSort(numbers);
        System.out.println("정렬 후: " + Arrays.toString(numbers));
    }
}
```

### 특징 및 쓰면 좋은 경우

**장점:**
- 구현이 간단하고 직관적
- 안정 정렬 (같은 값의 상대적 순서 유지)
- 제자리 정렬로 메모리 효율적
- 이미 정렬된 데이터에 대해서는 O(n)의 성능
- 온라인 알고리즘 (데이터가 들어오는 대로 정렬 가능)

**단점:**
- 평균적으로 O(n²)의 시간 복잡도
- 큰 데이터셋에서는 비효율적

**쓰면 좋은 경우:**
- 작은 크기의 배열을 정렬할 때 (보통 10-50개 이하)
- 이미 대부분 정렬된 데이터를 정리할 때
- 온라인으로 데이터가 들어오면서 실시간 정렬이 필요할 때
- 다른 고급 정렬 알고리즘의 하위 루틴으로 사용할 때
- 메모리 사용량을 최소화해야 할 때

---

## 4. 단순 정렬 비교표

| 알고리즘 | 시간복잡도 (평균) | 시간복잡도 (최악) | 시간복잡도 (최선) | 공간복잡도 | 안정성 | 특징 |
|---------|-----------------|-----------------|-----------------|------------|--------|------|
| 버블 정렬 | O(n²) | O(n²) | O(n) | O(1) | 안정 | 인접 원소 교환, 구현 쉬움 |
| 선택 정렬 | O(n²) | O(n²) | O(n²) | O(1) | 불안정 | 최솟값 선택, 교환 횟수 적음 |
| 삽입 정렬 | O(n²) | O(n²) | O(n) | O(1) | 안정 | 부분적으로 정렬된 데이터에 효율적 |

---

## 5. 핵심 정리

**단순 정렬의 공통 특징:**
- 모두 O(n²)의 시간 복잡도를 가짐 (삽입 정렬은 최선의 경우 O(n))
- 제자리 정렬로 추가 메모리가 거의 필요 없음
- 구현이 간단하고 이해하기 쉬움
- 작은 데이터셋에서는 충분히 실용적

**선택 기준:**
- **교육/학습 목적**: 버블 정렬 (가장 직관적)
- **교환 비용이 클 때**: 선택 정렬 (교환 횟수 최소)
- **부분 정렬된 데이터**: 삽입 정렬 (적응적 성능)
- **실시간 정렬**: 삽입 정렬 (온라인 알고리즘)

**실무에서의 활용:**
단순 정렬들은 대용량 데이터에는 부적합하지만, 고급 정렬 알고리즘(퀵 정렬, 병합 정렬)의 기초가 되며, 작은 부분 배열을 정렬하는 하위 루틴으로 자주 사용된다.