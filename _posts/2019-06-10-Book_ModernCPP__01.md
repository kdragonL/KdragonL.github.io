---
layout: post
title: Modern_CPP_book_01
---

피처 코츠슐링 / 모던C++입문, 엔지니어, 프로그래머를 위한 C++11/14입문

### 1장 : C++기초
(~1.4는 패스, 별 내용 없음)

#### 1.5 함수
- 1.5.1 : 인수(Parameter)
  - 연산 결과와 같은 임시 변수들은 레퍼런스로 전달할 수 없다.
  ```cpp 
  increment(i-9); // 오류 발생한다. 
  ```
  - 인수 목록의 끝에서만 여러 개의 기본값을 선언할 수 있다. 즉, 기본값을 갖는 인수 뒤에는 기본값이 없는 인수를 가질 수 없다.
 
- 1.5.4 : 오버로딩
  - Overload Resolution의 작동 방식
    1. 인수 ㅌ아비과 정확히 일치하는 오버로드 있다면 취한다. 그렇지 않다면
    2. 변환후 일치하는 오버로드 있는가?
        - 0개 : 오류. 일치하는 함수 찾을 수 없다.
        - 1개 : 해당 오버로드를 취한다.
        - \>1개 : 오류, 모호한 호출
  - 함수 오버로드는 시그니처가 서로 달라야 한다. 
    - 시그니처 : 1.함수의 이름 / 2. 인자의 개수 / 3. 인자의 타입
    
#### 1.6 오류 처리
- 1.6.1 : assert의 사용
  - NDEBUG를 선언하여, 컴파일러 플래그에서 처리 

- 1.6.2 : 예외
  - try .. throw .. catch. 조건문에는 비지니스 로직만 담는 게 나을 것이다. 
  - throw로 예외로 전달하고 싶은 객체를 써준다. 아무 개체나 상관 없지만, C++ std lib로 가능.
  - throw가 발생하면 함수를 빠져나가는데, stack에서 생성되었떤 객체들도 모두 소멸시켜준다. 
    (*이부분은 좀 더 구체적인 학습이 필요할 것 같다.*)
  - throw가 발생하면 가장 가까운 catch를 찾는다. 그리고 catch는 여러개 사용 가능 
  - 참고 : \[[try-catch 개괄](https://supercoding.tistory.com/1)\] \[[Stack Unwinding](https://supercoding.tistory.com/2)\]
  
#### 1.7 I/O
- (본문 내용은 특이사항 없음)
- 정확하게 cout, cin은 무엇인가? : iostream에 선언된 std::ostream, std::istream class의 글로벌 object이다.
- 참고 : [cout-and-cin-are-not-functions-so-what-are-they](https://stackoverflow.com/questions/20070606/cout-and-cin-are-not-functions-so-what-are-they)

#### 1.8 배열, 포인터, 레퍼런스
- (본문내용과 별개) When to memory management? : [stacoverflow:When_are_Variables_removed_from..m](https://stackoverflow.com/questions/1880984/when-are-variables-removed-from-memory-in-c/1881066#1881066)
  - C++에서는, scopre를 벗어나면 자동으로 free되는데, 동적할당된 경우는 직접 해제해줘야 한다.
  - RAII참고 필요 (Resource Acquisutuin Is Initialization)
- 포인터 관련 오류를 최소화하는 전략 세 가지
  1. 표준 컨테이너를 사용 (std::vector)
  2. 캡슐화 : 위에서 언급한 RAII 
  3. 스마트 포인터의 사용. 
- 스마트 포인터 : C++11
    1. unique_ptr : 참조한 데이터의 unique한 ownership을 의미. 기본적으로 일반 포인터와 같이 사용
      - 하지만, 소유권이 있기 때문에. 다른 객체는 delete를 할 수 없음 -> double free버그 방지 가능
      - 소유권의 이전은 가능하다 : 복사 생성자는 정의되지 않았지만, 이동 생성자는 가능. 
      - ```cpp
        std::unique_ptr<A> pb = std::move(pa);
        ```
      - 위의 코드에서, 소유권은 pa에서 pb로 강제로 이전된다. 
    2. shared_ptr : 여러 파티가 공통으로 사용하는 메모리. 더 이상 데이터를 참조하지 않는 즉시 메모리 자동 해제
      - referrence couter를 가지고 있다. use_count() 
    ```cpp
    shared_ptr<int> ptr01(new int(5)); // int형 shared_ptr인 ptr01을 선언하고 초기화함.
    cout << ptr01.use_count() << endl; // 1
    auto ptr02(ptr01);                 // 복사 생성자를 이용한 초기화
    cout << ptr01.use_count() << endl; // 2
    auto ptr03 = ptr01;                // 대입을 통한 초기화
    cout << ptr01.use_count() << endl; // 3 
    ``` 
      (출처 : http://tcpschool.com/cpp/cpp_template_smartPointer)

