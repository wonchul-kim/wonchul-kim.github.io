---
layout: post
title: Smart Pointers
category: C++
tag: [smart pointer]
---

# Smart Pointers




### `unique_ptr` vs. `shared_ptr`

| 기준        | `unique_ptr`     | `shared_ptr`             |
| --------- | ---------------- | ------------------------ |
| **소유권**   | 하나의 소유자만 존재      | 여러 객체가 공유 가능             |
| **복사**    | ❌ 복사 불가 (이동만 가능) | ✅ 복사 가능 (참조 카운트 증가)      |
| **오버헤드**  | 매우 적음 (가벼움)      | 참조 카운트 때문에 무거움           |
| **삭제 시점** | 소유자가 사라지면 즉시 삭제  | 마지막 `shared_ptr` 소멸 시 삭제 |
| **용도**    | 리소스를 단독 소유할 때    | 여러 곳에서 객체를 공유할 때         |
| **선호도**   | **기본값처럼 사용**     | 반드시 공유가 필요할 때만 사용        |


#### 정리된 사용 가이드라인
* 가능하면 unique_ptr를 기본값처럼 사용하세요.

* 객체가 한 곳에서만 관리된다면 unique_ptr로 충분합니다.

    * 예: 컴포넌트, 리소스 관리, factory return 등.

* **shared_ptr는 실제로 **“공유된 생명 주기”가 필요할 때만 사용하세요.

    * 예: Observer, Graph, Tree 구조에서 여러 참조자 필요 시.
    * 예: 이벤트 시스템, 콜백 큐에서 다수가 소유할 때.

* **raw pointer**와 섞어서 사용하지 말것

    ```cpp
    std::shared_ptr<int> p1 = std::make_shared<int>(10);
    int* raw = p1.get();         // raw pointer 추출
    delete raw;                  // ❌ 문제 발생
    ```

    * shared_ptr가 소유권을 가지고 있는데, raw 포인터로 delete 해버리면. 더블 삭제 (shared_ptr가 소멸될 때 다시 delete)
    * Undefined Behavior (UB) 발생 → 크래시 or 메모리 손상

