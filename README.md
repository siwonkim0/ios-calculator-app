# STEP 1 README

## STEP 1 UML
![](https://i.imgur.com/2PtFHlT.png)

## STEP 1 고민했던 점
### 1. 제네릭의 사용

제네릭 타입을 구현하면 사용자 정의 타입을 정의할 때 타입을 정해주는 것이 아니라, 실제로 사용할 때 타입을 지정할 수 있습니다. 따라서 숫자와 연산자 타입 둘다 담을 수 있도록 제네릭을 사용하였습니다.

처음에는 CalculatorItemQueue에 연산자와 피연산자를 함께 담으려고 했으나, 제네릭 타입의 인스턴스를 생성할 때 한가지 타입을 지정하면 calculatorItems 배열에 하나의 타입만 담을 수 있습니다. 그래서 고민하던 중에 성격이 다른 둘을 하나의 자료구조안에 넣어 관리하는 것은 맞지 않다고 생각하여 연산자와 피연산자를 분리하여 저장하기로 했습니다. 


#### 제네릭 타입 제약 
`class CalculatorItemQueue<T: CalculateItem>`
`class CalculatorItemQueue<T>: CalculateItem`

전자는 제네릭 타입으로 구현된 클래스의 타입 매개변수에 타입 제약을 준 것이고,
후자는 제네릭 타입으로 구현된 클래스가 CalculateItem 프로토콜을 채택한 것입니다.

두가지 방식이 헷갈려서 어떤 방식을 선택할지 고민하다가, 전자를 선택했습니다.
그 이유는 제네릭 타입을 특정 프로토콜을 따르는 타입만 사용할 수 있도록 제약을 두고 싶었고, 구체적으로는 calculatorItems 프로퍼티의 배열에 숫자와 연산자가 같이 들어와야하기 때문에 숫자와 연산자 타입 모두 CalculateItem 프로토콜을 채택하여서 두 타입만 사용가능하도록 하기 위해서입니다.

후자는 제네릭 타입에 타입 제약을 걸어주지 않고 클래스에서 프로토콜을 채택한 것인데 
CalculatorItemQueue의 역할은 계산에 필요하는 요소를 관리하는 것으로 계산에 참여하는 요소의 타입을 묶어주는 CalculateItem 프로토콜을 채택하는 것은 맞지 않다고 생각했습니다.


### 2. 시간 복잡도

처음 구현시에는 큐에서 요소를 하나씩 뺄 때 removeFirst()로 제일 처음 들어온 요소를 하나씩 뺐었는데, removeFirst()의 시간 복잡도가 O(n)으로 제일 처음 요소를 빼면 그 다음 요소들의 인덱스를 모두 한칸씩 앞으로 옮겨야 해서 비효율적입니다.

따라서, calculatorItems 배열에 담긴 요소들을 reversed()로 뒤집은 후 거기서 removeLast()를 통해 첫번째 요소를 빼고 다시 뒤집어서 담는 방식을 사용했습니다.
removeLast()와 reversed()의 시간 복잡도는 각각 O(1)로 removeFirst()보다 빠릅니다.

#### **[#](https://camp.yagom-academy.kr/board/614c48258a9fd23b353b0734/articles/618aa6957c82755a83c68b47#on-%EC%8B%9C%EA%B0%84%EB%B3%B5%EC%9E%A1%EB%8F%84%EB%A5%BC-%EA%B0%96%EB%8A%94-%EB%A9%94%EC%84%9C%EB%93%9C) O(n) 시간복잡도를 갖는 메서드**

- [removeFirst()](https://developer.apple.com/documentation/swift/contiguousarray/2885850-removefirst) -> “Removes and returns the first element of the collection.”
- [reverse()](https://developer.apple.com/documentation/swift/array/2943836-reverse) -> “Reverses the elements of the collection in place.”
- removeAll() -> “Removes all elements from the array.”
- insert()

#### **[#](https://camp.yagom-academy.kr/board/614c48258a9fd23b353b0734/articles/618aa6957c82755a83c68b47#o1-%EC%8B%9C%EA%B0%84%EB%B3%B5%EC%9E%A1%EB%8F%84%EB%A5%BC-%EA%B0%96%EB%8A%94-%EB%A9%94%EC%84%9C%EB%93%9C) O(1) 시간복잡도를 갖는 메서드**

- isEmpty() -> “A Boolean value indicating whether the collection is empty.”
- append() -> “Adds a new element at the end of the array.”
- [removeLast()](https://developer.apple.com/documentation/swift/array/2885764-removelast) -> “Removes and returns the last element of the collection.”
- [reversed()](https://developer.apple.com/documentation/swift/array/1690025-reversed) -> “Returns a view presenting the elements of the collection in reverse order.”

### 3. 유닛 테스트 적용 
- setUpWithError(), tearDownWithError()
유닛테스트 코드를 작성할 때 테스트를 하기위한 타입 sut을 테스트 케이스마다 생성하고 초기화 하는데 사용하는 메서드들입니다.테스트 메서드가 하나 실행되기 전 그리고 후에 한번씩 호출되는 메서드로 테스트를 하는데, 하나의 테스트 메서드의 결과가 다른 테스트 메서드의 결과에 영향을 주지 못하도록 설정해주는 메서드입니다.

    이 메서드들을 테스트 클래스내에 정의해놓으면 각각의 테스트 케이스 메서드가 호출이 될 때 자동으로 실행됩니다. 이 부분은 뷰 라이프사이클에서 뷰가 로드될 때 viewDidLoad(), viewWillAppear()등의 매서드들이 자동으로 호출되는 것과 비슷한 패턴으로 진행됩니다. 





## STEP 1 구현 내용 
- CalculateItem 프로토콜
- CalculateItem을 채택한 Character 익스텐션
- 제네릭 타입 CalculatorItemQueue 구조체
    1. var calculatorItems : 피연산자나 연산자를 담고 있는 배열
    2. func enqueue(_ item: T): 요소를 큐에 넣는 메서드
    3. func dequeue() -> T?: 큐의 제일 처음 요소를 빼는 메서드
    4. removeAll(): 큐의 모든 요소를 제거하는 메서드
