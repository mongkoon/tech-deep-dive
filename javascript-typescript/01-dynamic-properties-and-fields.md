# 동적 프로퍼티 생성과 클래스 필드 선언 비교

> 키워드: `#JavaScript` `#TypeScript` `#ClassFields` `#PrivateField` `#StrictNullChecks`

---

## Q1. 클래스 변수 선언 없이 `this.countText`를 바로 사용할 수 있는 이유는?

JavaScript는 객체에 프로퍼티를 동적으로 추가할 수 있는 언어이기 때문입니다. 클래스 상단에 멤버 변수를 미리 선언하지 않아도, `constructor` 내부에서 `this.countText = ...` 형태로 값을 할당하는 순간 해당 인스턴스에 `countText` 프로퍼티가 자동으로 생성됩니다.

* **동적 프로퍼티 생성**: Java, C++, C# 같은 정적 타입 언어는 클래스 내부에 필드를 미리 선언해야 하지만, JavaScript의 클래스 인스턴스도 결국 일반 객체이므로 런타임에 `this.프로퍼티명 = 값` 형태로 언제든 새 속성을 추가할 수 있습니다.
* **최신 문법과의 비교**: ES2022 이후부터는 가독성을 위해 클래스 최상단에 필드를 미리 선언하는 문법(Class Fields)을 공식 지원합니다. 일반 공개(public) 프로퍼티는 생략해도 동작하지만, `#countText` 같은 private 필드나 TypeScript 환경에서는 반드시 미리 선언해야 합니다.

---

## Q2. JS 공개 필드, `#` 비공개 필드, TypeScript 선언 예제 비교

### 1) JavaScript 공개(Public) 필드 미리 선언 (ES2022+)
```javascript
class CounterView {
  viewModel;
  countText;
  incButton;

  constructor(viewModel) {
    this.viewModel = viewModel;
    this.countText = document.getElementById("count");
    this.incButton = document.getElementById("inc-btn");
  }
}
```

### 2) JavaScript 비공개(Private) 필드 (`#`)
변수명 앞에 `#`을 붙이면 클래스 외부에서 접근할 수 없는 진짜 비공개 필드가 됩니다. **`#` 필드는 동적 생성이 불가능하므로 반드시 클래스 상단에 먼저 선언해야 합니다.**

```javascript
class CounterView {
  #viewModel;
  #countText;
  #incButton;

  constructor(viewModel) {
    this.#viewModel = viewModel;
    this.#countText = document.getElementById("count");
    this.#incButton = document.getElementById("inc-btn");

    this.#viewModel.subscribe((newCount) => {
      this.#countText.textContent = newCount;
    });
  }
}

const app = new CounterView(viewModel);
// console.log(app.#countText); // SyntaxError: 외부 접근 불가
```

### 3) TypeScript 환경에서 미리 선언
```typescript
class CounterView {
  private viewModel: CounterViewModel;
  private countText: HTMLElement | null;
  private incButton: HTMLElement | null;

  constructor(viewModel: CounterViewModel) {
    this.viewModel = viewModel;
    this.countText = document.getElementById("count");
    this.incButton = document.getElementById("inc-btn");

    this.incButton?.addEventListener("click", () => {
      this.viewModel.onIncrementClicked();
    });

    this.viewModel.subscribe((newCount: number) => {
      if (this.countText) {
        this.countText.textContent = String(newCount);
      }
    });
  }
}
```

---

## Q3. `CounterViewModel`과 `HTMLElement | null`의 차이는?

TypeScript의 엄격한 null 검사(`strictNullChecks: true`) 기준으로 Non-nullable과 Nullable의 차이를 나타냅니다.

* **`private viewModel: CounterViewModel;`**: `null`이나 `undefined`를 허용하지 않는 Non-nullable 변수입니다.
* **`private countText: HTMLElement | null;`**: 유니온 타입(`|`)을 사용해 `HTMLElement` 또는 `null`을 가질 수 있는 **nullable** 변수이며, 사용 시 `if (this.countText)`나 옵셔널 체이닝(`?.`) 같은 `null` 체크가 강제됩니다.
