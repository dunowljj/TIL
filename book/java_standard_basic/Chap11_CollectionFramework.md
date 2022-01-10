컬렉션 프레임워크
========
# 1. Collection
## 1.1.Collection 이란?
- 데이터 군을 저장하는 클래스들을 표준화한 설계
- JDK1.2 이전까지는 Vector, Hashtable, Properties와 같은 컬렉션 클래스들을 서로 다른 방식으로 처리, 이후부터 컬렉션 프레임웍 등장
- 생산성, 유지보수
- JDK 1.8에 이으러서 람다와 스트림에 의해 이루지 못한 표준화를 이룸 (다양한 종류의 데이터를 동일한 방식으로 다루는 것)
***

## 1.2. 핵심 인터페이스
- 모든 컬렉션 클래스들은 List, Set, Map 중 하나를 구현하고 이름에 표시됨
### 1.2.3. List
-  데이터의 집합 순서o, 중복 허용
### 1.2.3. Set
-  데이터의 집합 순서x, 중복 허용x
### 1.2.3. Map
- 키와 값의 쌍(key,value,pair)로 이루어진 데이터의 집합
- 순서 x, 키 중복x , 값 중복o
- 중복된 키와 값을 저장하면 기존 값 없어지고 마지막 저장된 값이 남음
***

## 1.3. 인터페이스 메서드
### 1.3.1. Collection
- iterator iterator() : iterator를 얻어서 반환
- boolean removeAll(Collection c)
- boolean ratainAll(Collection c) : 지정된 Collection에 포함된 객체만 남기고 다른 객체들은 Collection에서 삭제. 변화가 있으면 true, 없으면 false
- Object[] toArray()
- Object[] toArray(Object[] a)
### 1.3.2. List
- boolean addAll(int index, Collection c)
- ListIterator listIterator()/(int index) : List 접근 가능한 ListIterator 반환
- Object remove(int index) : 해당 인덱스 객체 지우고 반환
- void sort(Comparator c)
- List subList(int fromIndex, int toIndex)
### 1.3.3. Set
- clear()
- Object[] toArray()/(Object[] a)
### 1.3.4. Map
- boolean containsKey(Object key)
- boolean containValue(Object value) : 일치하는 객체 있는지 확인
- Set entrySet() : Map.Entry 타입의 객체로 저장한 Set을 반환
- Map에 저장된 모든 key 객체를 반환
- Object put(Object key, Object value): 키와 연결해 저장
- void putAll(Map t) 
### 1.3.5. ArrayList
- Object set(int index, Object element) : 해당 위치에 객체 저장하고 이전에 있던놈 반환
- void add(int index, Object element)
- void ensureCapacibility(int minCapacity)
- void trimToSize()
- void sort(Comparator)
- boolean addAll(int index, Collection c)/(Collection c)
- boolean retainAll(Collection c) : Collection과 겹치는 부분 제외하고 모두 없어짐
***

## 1.4. ArrayList vs LinkedList
### 1.4.1. ArrayList 요소 삭제 과정
    1. 중간에 껴있는 요소 삭제
    2. 아래에 있는 데이터들을 복사해서 한 칸 위에 덮어씀
    3. 이동하고 남은 맨아래 데이터 -> null 상태
    4. size 1 감소
- 삽입, 삭제가 자주 있으면 불리함. 비효율적인 메모리 사용
- 랜덤 엑세스 가능
### 1.3.2. LinkedList 추가와 삭제
- 삭제 : 참조 요소만 변경하면 삭제 끝
- 추가 : 새로운 요소 생성, 추가하고자 하는 위치의 이전요소의 참조를 새로운 요소로 변경, 새로운 요소가 다음 요소를 참조하도록 변경
- 삽입, 삭제가 빠름 -> 참조만 바꾸면 끝
- 순차적 접근 구조라 접근시간이 길어지는 문제. 데이터가 많을수록 접근성이 떨어진다.
###
- clear() 싹 비움
- Set, Collection은 remove시 boolean 반환/ List, Map은 Object 반환
***

## 1.5. Stack, Queue
### 1.5.1. Stack
#### 1.5.1.1. 설명
- FILO(First In Last Out)
- LinkedList 구현하는게 유리
#### 1.5.1.2. Stack 메서드
- boolean empty()
- Object peek() : 맨 위에 저장된 객체 반환, 비어있으면 EmptyStackException 발생
- Object pop() : 맨 위에 저장되 객체 꺼내기
- Object push(Object item)
- int search(Object o) : (1부터 시작해서 맨 위부터 1) 주어진 객체 찾아서 그 위치를 반환
### 1.5.2. Queue
#### 1.5.2.1. 설명
- FIFO(First In First Out)
- ArrayList로 구현하는게 유리
#### 1.5.2.2 Queue 메서드
- boolean add(Object o)
- Object remove() : 꺼내서 반환, 비어있으면 NoSuchElementException
- Obejct element()
- boolean offer(Object o)
- Object poll() : 꺼내서 반환, 비어있으면 null반환
- Object peek() : 삭제 없이 요소 읽어옴, 비어있으면 null반환
+ java API문서에 보면 All Known Implementing Calsses라는 항목이 있는데 해당 클래스들은 Queue인터페이스에 정의된 메서드를 모두 작성해 놓았다.
### 1.5.3. 메서드 차이
- 값이 비어있을때
    - Stack : 
        - peek() : EmptyStackException
        - pop() : EmptyStackException
    - Queue :
        - peek() : null
        - poll() : null
        - element() : NoSuchElementException
        - remove() : NoSuchElementException
- 이유는 모르겠으나 스택은 기본적으로 꺼내기와 반환 호출 후 스택이 비어있으면 오류를 뿌리지만, 큐는 없으면 null 주고 말지라는 마인드에 오류를 주는 메서드들이 따로 있다. 
### 1.5.4. 활용
- Stack : 수식계산, 수식괄호검사, 워드프로세서의 undo/redo, 웹브라우저의 뒤로/앞으로
- Queue : 최근사용문서, 인쇄작업 대기목록, 버퍼(buffer)
***

## 1.6. Iterator, ListIterator, Enumeration 
### 1.6.1. Iterator
- 컬렉션에 저장된 요소에 접근하는데 사용하는 인터페이스 
- Enumeration은 Iterator 구버전
- List나 Set인터페이스를 구현하는 컬렉션은 iterator()가 특징에 맞게 작성 -> Iterator를 구현한 클래스의 인스턴스를 반환
- List클래스들은 저장순서를 유지하기떄문에 iterator로 뽑아도 순서유지, Set은 안그럼
### 1.6.2. ListIterator
- Iterator에 양방향 조회기능추가
### 1.6.3. Map과 iterator
- 키 값이라 iterator 직접 호출 불가
- keySet(), entrySet()과 같은 메서드를 통해 Set 형태로 얻어서 iterator 호출해야함
```
Map map = new HashMap();
...
Iterator it = map.entrySet().iterator();
```
***

### 1.7. Arrays
- toString([] a)
## 1.7.1. 복사
- copyOf(배열, 길이) : 배열 처음부터 길이만큼 복사
- copyOfRange(배열, 시작, 끝) : 끝 불포함
- 끝 범위나 길이 배열보다 길면 0값으로 생성
## 1.7.2. 채우기, 정렬, 검색
- fill(배열, 값) : 지정된 값으로 채우기
- setAll(배열 ,함수형 인터페이스) : 배열을 채우는데 사용할 함수형 인터페이스를 매개변수로 받는다.
```
Arrays.setAll(arr, (i) -> (int)(Math.random()*5)+1);
```
## 1.7.3. 배열의 정렬과 검색
- sort(arr) : 배열 정렬할 때
- bianrySearch(arr, index) : 정렬된 요소를 검색할 때 사용, 정렬 되어 있어야 올바른 결과 나옴 -> 일치하는 요소가 여러개면 어떤 것의 위치가 반활될지 알 수 없음
### 검색
- 이진 검색(bianry search) : 검색 범위를 반씩 줄여 매우 빠름, 정렬이 돼 있어야 사용 가능
- 순차 검색(linear search) : 첫 요소부터 순차적으로 검색. 요소를 하나씩 비교해서 많이 걸림, 정렬이 안돼 있어도 됨
## 1.7.4. 비교와 출력
- toString(arr)
- equals() : Arrays의 equals를 사용 시 모든 요소를 비교해서 t/f -> **이차원 이상 비교** 시 **배열에 저장된 배열의 주소**를 비교하게 된다.
- deepToString(arr) : 다차원 배열에서 사용가능. 재귀적 접근을 통하기에 3차원 이상에서도 동작
- deepEquals() : 이차원 이상 비교시 사용
## 1.7.5. 변환
- asList(Object... a) : 배열을 List에 담아서 반환 
    - 매개변수가 가변인수라 값만 나열해도 상관 없음
    - asList()가 반환한 List의 크기를 변경할 수 없다.
```
List list = new ArrayList(Arrays.asList(1,2,3,4,5));
```
- 위와같이 사용 시 크기 변경 가능
### parallelXXX(), spliterator(), stream()
- parallel : 해당 키워드로 시작하는 이름의 메서드들은 빠른 결과를 위해 여러 쓰레드가 나눠서 작업하도록 함
- spliterator() : 여러 쓰레드가 처리할 수 있게 하나의 작업을 여러 작업으로 나누는 Spliterator 반환
- stream() : 컬렉션을 스트림으로 변환
- 14장 람다와 스트림에서 배움
***

## 1.8. Comparator와 Comparable
### 1.8.1. Comparator
- 기본 정렬기준 외에 다른 기준으로 정렬하고자 할 때 사용
- int compare()
### 1.8.2. Comparable
- 기본 정렬기준을 구현하는데 사용
- 구현 시 정렬이 가능하다는 의미. 주로 래퍼클래스들이 오름차순 정렬로 구현되어 있음
- int compareTo(Object o);
- String의 Comparable구현은 유니코드순으로 오름차순 정렬된다.**(공백, 숫자, 대문자, 소문자)**
### 1.8.3. Arrays.sort
- Comparator를 지정해주지 않으면 저장하는 객체에 구현된 내용에 따라 정렬된다.
- String.CASE_INSENSITIVE_ORDER); 사용 시 대소문자 구분없이 정렬된다.
### 1.8.4. Integer와 Comparable
- 간단한 뺄셈으로 compareTo() 구현 가능 -> 성능은 삼항이 더 좋음
- Arrays.sort와 같은 메서드는 이미 정렬 알고리즘이 잘 되어있으므로 compareTo()에 정렬방식만 구현해주면 됨
- -1만 곱하면 반대 정렬된 결과 얻을 수 있음
```
//이미 정렬 기준이 있는 매개변수 제공
Arrays.sort(arr);
//정렬 기준을 제공
Arrays.sort(arr, new DescComp()); 
```
- 위처럼 이미 기준이 있거나 기준을 제공하지 않으면 예외 발생
***

## 1.9. HashSet
### 1.9.1. 설명
- Set을 구현한 대표적인 컬렉션
- 중복 방지 -> add나 addAll로 추가 시 이미 저장되어 있는 요소와 중복된 요소를 추가하고자 한다면 false를 반환하며 추가에 실패한 것을 알림
- 중복 제거와 동시에 순서를 유지하려면 LinkedHashSet을 사용해야함
### 1.9.2. 메서드(424p)
- HashSet()/(Collection c)/(int initialCapacity)(int initialCapacity, float loadFactor)
- boolean removeAll(Collection c) : 겹치는거 다 삭제(차집합)
- boolean containsAll(Colleciont c) : 컬렉션에 저장된 모든 객체를 포함하고 있는지 알려줌(교집합)
- Object[] toArray() : 저장된 객체들 객체배열 형태로 변환
- Object[] toArray(Object[] a) : 저장된 객체를 주어진 객체 배열에 담는다
### 예제
- HashSet에 Integer 1과 String "1" 넣을 시 구분되어 값이 저장됨
- set.size()로 for문 실행조건 넣어서 로또번호 만들기
+ HashSet에 내용이 같은 다른 객체를 집어넣으면 중복처리가 되지 않는다
-> equals()와 hashCode()를 오버라이딩 하면 내용으로 비교 가능; 두개 다 호출하므로 두개 다 오버라이딩 해야한다.
***

## 1.10. TreeSet
### 1.10.1. 설명
- 정렬, 검색, 범위검색에 높은 성능
- 이진 탐색 트리의 성능을 향상시킨 레드-블랙 트리로 구현
- (Set구현)중복 허용x, 정렬된 위치에 저장(저장 순서x)
- 각 노드에 최대 2개의 노드, '루트(root)'라 불리는 노드에서 시작해서 확장
### 1.10.2. 이진 탐색 트리
- 부모노드의 왼쪽에는 부모노드의 값보다 작은 값의 자식노드를, 오른쪽에는 큰 값의 자식노드를 저장하는 이진트리
- 저장된 값에 비례하여 검색시간 증가
- 값의 수가 10배가 되어도 3~4번 밖에 비교횟수가 증가하지 않음
- 링크드 리스트보다 저장(저장위치 찾아야)이나 삭제(트리일부 재구성)는 더 걸리나 검색, 정렬은 더 뒤어남
### 1.10.3. 저장과정
- 첫 번째 저장 값은 루트가 됨
- 왼쪽 마지막 레벨이 제일 작은 값, 오른쪽 마지막 레벨이 값이 가장 큼
- 저장되는 객체가 Comparable 구현 또는 Comparator 제공하지 않으면 예외 발생
### 1.10.4. TreeSet 메서드(432p)
- TreeSet()/(Collection c)/(Comparator comp)/(SortedSet s)
- Object ceiling/floor(Object o) : 지정된 객체와 같은 객체 반환. 없으면 해당 객체보다 큰/작은 값을 찾아 반환. 없으면 null
- Object higher/lower(Object o) : 지정된 객체보다 큰/작은 값 중 가장 가까운 값의 객체 반환. 없으면 null
- SortedSet headSet/tailSet(Object toElement) : 이게 머리다/꼬리다 정해주는 느낌. 해당 객체보다 작은/큰 값의 객체들을 반환
- NavigableSet headSet(Object toElement, boolean inclusive)
- Object pollFirst()/pollLast() : 첫번째/마지막 요소(제일 작은/큰)를 반환 (제거까지)
- Object first()/last() : 정렬된 순서에서 첫/마지막 객체를 반환(반환만)
- SortedSet subSet(Object fromElement, Object toElement) : 범위 검색, 끝 범위 불포함
- NavigableSet subSet(Object fromElement, boolean fromInclusive, ObjecttoElement, boolean to incluseive)
***

## 1.11. HashSet과 Hashtable
### 1.11.1. 설명
- Hashtable은 Vector와 같은 맥락
- 해싱을 사용하기때문에 많은 양의 데이터를 검색하는데 있어서 뛰어난 성능
- HashMap은 Entry라는 내부 클래스를 정의(안에 key,value)하고, 다시 Entry타입의 배열을 선언하고 있음.
- key-value 모두 Object 형태로 저장할 수 있지만, 키는 주로 String 대문자 또는 소문자로 통일해서 사용함
- key값 겹치면 데이터 덮어써짐
### 1.11.2. 메서드
- boolean containsKey/Value(Object key/value) : 객체에 HashMap에 저장되어 있는 키/값이 있는지 알려준다.
- Set entrySet()/keySet() : 저장된 키와 값을 엔트리(키와 값의 결합)의 형태로 Set에 저장해서 반환 / 모든 키가 저장된 Set반환
- void putAll(Map m) : Map에 저장된 모든 요소 저장
- Object get(Object key)
- Object getOrDefault(Object key, Object defaultValue)
- Collection values() : HashMap에 저장된 모든 값을 컬렉션의 형태로 변환
***

## 1.12. Collections의 메서드
- 배열엔 Arrays, 컬렉션엔 Collections
- fill(), copy(), sort(), binarySearch)
### 1.12.1. 컬렉션의 동기화
- 멀티쓰레드에서 하나의 객체에 여러 쓰레드가 동시에 접근 할 수 있기 때문에 데이터의 무결성을 유지해야 한다. -> 동기화 필요
- Vector, Hashtable : **자체적 동기화**가 되어 있어 멀티쓰레드 프로그래밍이 아닌 경우 불필요한 기능으로 성능 저하
- ArrayList, HashMap : 자체적 동기화 대신 **동기화 메서드**를 제공한다.
```
//메서드 형식
static Collection synchronizedCollection(Collection c)
static List synchronizedList(List list)
...
//사용
Lisy synList = Collections.synchronizedList(new ArrayList(...));
```
### 1.12.2 변경불가 컬렉션
- 컬렉션에 저장된 데이터를 보호하기 위해 변경 불가(읽기 전용)으로 만들기
- 주로 멀티 쓰레드 프로그래밍에서 여러 쓰레드가 하나의 컬렉션 공유 시 손상 가능성
```
static Colleciont unmodifiableCollection(Collection c)
...
```
### 1.12.3. 싱글톤 컬렉션
- 단 하나의 객체만을 저장하는 컬렉션을 만들어야 하는 경우
```
static List singletonList(Object o)
static List singleton(Object o) 
static List singletonMap(Object key, Object value)
```
- 반환된 컬렉션 변경 불가
### 1.12.4. 단일 컬렉션
- 한 종류의 객체만 저장하는 컬렉션
```
//메서드 형식
static Collection checkedCollection(Collection c, Class type)
...
//사용 - 두번째 매개변수에 저장할 객체의 클래스 지정
List list = new ArrayList();
List checkedList = checkedList(list, String.class) //String만 저장 가능
```

### 448p 그림