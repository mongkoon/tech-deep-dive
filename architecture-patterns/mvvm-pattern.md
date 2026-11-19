# MVVM 패턴의 핵심 원리와 최소 구현 예제

> 키워드: `#MVVM` `#Architecture` `#DesignPattern` `#JavaScript` `#Android` `#DataBinding`

---

## 1. MVVM 패턴이란?

**MVVM(Model - View - ViewModel)**은 사용자 인터페이스(View)의 개발을 비즈니스 로직 및 데이터 처리(Model)로부터 분리하여, UI가 특정 데이터 모델이나 플랫폼 구현에 종속되지 않도록 만드는 소프트웨어 아키텍처 패턴입니다.

### 구성 요소의 역할
* **Model**: 앱에서 다루는 핵심 데이터, 상태, 비즈니스 규칙을 저장하고 처리합니다. UI(View)나 ViewModel의 존재를 전혀 알지 못합니다.
* **View**: 사용자가 직접 보고 상호작용하는 UI(DOM, XML, SwiftUI 등)를 담당합니다. 사용자 입력을 ViewModel에 전달하고, ViewModel의 상태 변화를 관찰(Observe)해 화면만 갱신합니다.
* **ViewModel**: View와 Model 사이의 중재자입니다. View로부터 받은 이벤트를 처리해 Model에 데이터를 요청하고, 결과를 View가 구독(Subscribe)하기 좋은 관찰 가능한 상태(Observable Data)로 가공해 노출합니다. **View를 직접 참조하지 않는 것이 핵심**입니다.

---

## 2. 동작 흐름

1. 사용자의 입력(버튼 클릭 등)이 **View**를 통해 들어옵니다.
2. **View**가 해당 동작에 맞는 처리를 **ViewModel**에 요청합니다.
3. **ViewModel**이 **Model**에게 데이터 조회나 변경을 요청합니다.
4. **Model**이 비즈니스 로직을 수행한 뒤 결과를 응답합니다.
5. **ViewModel**이 변경된 값을 관찰 가능한 필드(Observer / LiveData / State)로 갱신해 외부에 알립니다.
6. **View**가 ViewModel의 변경 알림을 받아 자신의 UI를 업데이트합니다.

---

## 3. 가장 단순한 구현 예제 (Vanilla JavaScript)

프레임워크나 라이브러리 없이 순수 JavaScript만으로 카운터 앱의 MVVM 구조를 구현한 최소 예제입니다.

```javascript
// 1. Model: 데이터와 핵심 규칙만 담당
class CounterModel {
  constructor() {
    this.count = 0;
  }

  increment() {
    this.count += 1;
    return this.count;
  }
}

// 2. ViewModel: View가 쓸 상태와 동작을 관리하고 변경을 알림
class CounterViewModel {
  constructor(model) {
    this.model = model;
    this.count = model.count;
    this.listeners = [];
  }

  subscribe(listener) {
    this.listeners.push(listener);
    listener(this.count);
  }

  notify() {
    this.listeners.forEach((fn) => fn(this.count));
  }

  onIncrementClicked() {
    this.count = this.model.increment();
    this.notify();
  }
}

// 3. View: 사용자 입력을 ViewModel에 전달하고 상태 변화만 화면에 반영
class CounterView {
  constructor(viewModel) {
    this.viewModel = viewModel;
    this.countText = document.getElementById("count");
    this.incButton = document.getElementById("inc-btn");

    this.incButton.addEventListener("click", () => {
      this.viewModel.onIncrementClicked();
    });

    this.viewModel.subscribe((newCount) => {
      this.countText.textContent = newCount;
    });
  }
}

// 실행 연결
const app = new CounterView(new CounterViewModel(new CounterModel()));
```

---

## 4. Android (Kotlin) 실무 구조와의 비교

순수 JS 예제에서 직접 구현한 `listeners` 배열과 `subscribe()` 메커니즘은 Android 생태계에서 `LiveData`나 `StateFlow`, 그리고 `DataBinding`으로 표준화되어 있습니다.

| 역할 | Vanilla JS 예제 | Android (Kotlin) |
| :--- | :--- | :--- |
| **Model** | `CounterModel` 클래스 | Repository / DataSource / Domain Model |
| **ViewModel** | `CounterViewModel` + `listeners` 배열 | `androidx.lifecycle.ViewModel` + `MutableLiveData` / `StateFlow` |
| **View** | `CounterView` + `document.getElementById` | `Activity` / `Fragment` + XML Layout (`DataBinding`) |
| **바인딩 방식** | `subscribe((newCount) => ...)` 콜백 등록 | `viewModel.data.observe(viewLifecycleOwner) { ... }` |

---

## 5. MVVM 패턴의 장단점

* **장점**
  * **의존성 최소화**: ViewModel이 View를 직접 알지 못하므로 UI 프레임워크가 바뀌어도 비즈니스 로직과 상태 관리 코드를 재사용할 수 있습니다.
  * **테스트 용이성**: UI 렌더링 환경 없이도 ViewModel과 Model의 상태 변화를 단위 테스트(Unit Test)로 검증하기 쉽습니다.
* **단점**
  * **초기 설계 복잡도**: 화면과 상태가 단순한 소규모 앱에서는 구독(Observer) 연결과 보일러플레이트 코드가 오히려 과도하게 느껴질 수 있으며, 규모가 커질수록 상태 흐름 설계 난이도가 높아집니다.
