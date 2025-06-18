---
layout: post
title: SemDeDup
category: Deep Learning
tag: [reduce dataset]
---

# Modern C++

C++98, C++03까지의 전통적인 스타일 이후,

C++11/14/17/20/23의 새로운 기능과 스타일이 적용된 C++를 **Modern C++**라고 한다.

| 기능       | 이전 C++          | 모던 C++                                          |
| -------- | --------------- | ----------------------------------------------- |
| 타입 추론    | 없음              | `auto`, `decltype`                              |
| 메모리 관리   | `new/delete`    | 스마트 포인터 (`unique_ptr`, `shared_ptr`)            |
| 반복문      | 반복자 중심          | 범위 기반 반복문                                       |
| 함수 표현    | 함수 객체 또는 함수 포인터 | 람다 함수                                           |
| 복사/이동    | 복사 중심           | move semantics (`std::move`)                    |
| 템플릿      | SFINAE          | Concepts, cleaner syntax                        |
| 동시성      | pthread 등 외부 의존 | `std::thread`, `std::async`                     |
| 컨테이너 초기화 | 복잡함             | `{}` 일관된 초기화 방식                                 |
| 기타       | 제한적 도구          | `optional`, `variant`, `filesystem`, `ranges` 등 |


#### `auto` 

변수에 대해 자동으로 타입 추론이 가능

> 성능(속도?) 면에서 단점은 없나, 가독성 및 디버깅할 때 안좋으려나?


#### Smart Pointers - `unique_ptr`, `shared_ptr`, `lock_guard`, ...

* Before
    ```cpp
    MyClass* obj = new MyClass();
    ...
    ...
    delete obj;
    ```

* After
    ```cpp
    std:unique_ptr<MyClass> obj = std::make_unique<MyClass>();
    ...
    ...
    // 자동으로 소멸됨 (RAII)
    ```

메모리 누수를 방지하고, 자원 관리를 자동화한다.


> RAII: 동적인 프로그래밍을 위해서 `new`를 사용하여 힙 메모리에서 메모리를 할당받으면, 할당 받는 순간 해당 메모리의 resource는 개발자가 직접 관리하게 된다. 이 때, 수동으로 관리하기 때문에 메모리 누수가 발생할 수 있음. 이러한 단점을 보완하기 위해서 **smart pointers**가 개발되었고, 해당 클래스들은 함수가 끝나면 (`{}`에서 벗어나면), finally처럼 무조건 실행해준다. 이렇게 함수가 끝나면 무조건 실행을 보장해주는 클래스들의 기능적인 부분들을 통틀어 **RAII**라고 한다. 


#### Range_based for loop

* Before
    ```cpp
    for (std::vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
        std::cout << *it << "\n";
    }
    ```
* After
    ```cpp
    for (auto x : vec) {
        std::cout << x << "\n";
    }

반복문이 더 간결하고 읽기 쉬운 형태로 개선됨

#### Lambda Expressions

* Before
    ```cpp
    struct Print {
        void operator()(int x) const { std::cout << x << "\n"; }
    };

    std::for_each(vec.begin(), vec.end(), Print());
    ```

* After
    ```cpp
    std::for_each(vec.begin(), vec.end(), [](int x) {
        std::cout << x << "\n";
    });

간단한 동작을 inline 함수 형태로 작성 가능

#### Move Semantics

* Before
    ```cpp
    std::string a = "hello";
    std::string b = a; // deepcopy
    ```

* After
    ```cpp
    std::string a = "hello";
    std::string b = std::move(a); // 이동 발생 (a는 빈 문자열)
    ```

불필요한 복사 오버헤드 감소, 성능 향상

> 빈 문자열????? 어떻게 동작함?


#### 초기화 방식 개선 - Uniform Initialization

* Before
    ```cpp
    std::vector<int> v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(3);
    ```

* After
    ```cpp
    std::vector<int> v = {1, 2, 3};
    ```

일관된 `{}` 초기화로 가동성 향상 및 버그 감소

#### Concurrency 내장

* Before 
    ```cpp
    pthread_create(...);
    ```

    * OS API 사용
    * 복잡하고 플랫폼 의존

    > ?????????

* After
    ```cpp
    std::thread t([]() { std::cout << "Running in thread\n"; });
    t.join();
    ```

C++ 표준 라이브러리에서 thread, future, async 등을 직접 지원함

#### `template` 개선 

* Before
    ```cpp 
    template<typename T>
    typename std::enable_if<std::is_integral<T>::value>::type
    foo(T x) { ... }
    ```
    
    * 복잡한 SFINAF, 에러 메시지 장황

* After
    ```cpp
    template<std::integral T>
    void foo(T x) { ... }
    ```

제약된 template으로 에러 메세지 명확히함


#### 유용한 새로운 타입들

* `std::optional<T>`: 값이 있을 수도, 없을 수도 있음 (nullable 변수)
* `std::variant<T1, T2>`: 여러 타입 중 하나를 저장 가능
* `std::any`: 어떤 타입이든 저장 가능
* `std::filesystem`: 파일/디렉토리 탐색 표준화
* `std::span`: 포인터/배열을 안전하게 다룸


