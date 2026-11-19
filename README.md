# tech-deep-dive

프로그래밍 언어의 설계 철학, 역사적 배경, 패러다임 및 소프트웨어 아키텍처 패턴을 깊이 있게 탐구한 Q&A 아카이브입니다.

## 디렉토리 구조

```text
tech-deep-dive/
├── README.md
├── architecture-patterns/
│   └── mvvm-pattern.md
├── javascript-typescript/
│   ├── 01-dynamic-properties-and-fields.md
│   ├── 02-history-of-js-and-es6-class.md
│   └── 03-js-vs-ts-tradeoffs-and-usage.md
└── language-comparison/
    └── paradigm-and-feature-scale.md
```

## 목차

### Architecture Patterns
* [MVVM 패턴의 핵심 원리와 최소 구현 예제](./architecture-patterns/mvvm-pattern.md) — Model·View·ViewModel의 역할, 데이터 흐름, 순수 JS 및 Android(Kotlin) 구조 비교

### JavaScript & TypeScript
* [01. 동적 프로퍼티 생성과 클래스 필드 선언 비교](./javascript-typescript/01-dynamic-properties-and-fields.md) — JS의 동적 프로퍼티 생성 원리, ES2022 `#` private 필드, TS의 Nullable 타입 비교
* [02. JavaScript의 동적 설계 역사와 ES6 `class` 도입 배경](./javascript-typescript/02-history-of-js-and-es6-class.md) — 1995년 탄생 비화, ES4 좌초, 20년 만에 `class`가 도입된 이유
* [03. TypeScript의 트레이드오프와 JS/TS 실무 생태계](./javascript-typescript/03-js-vs-ts-tradeoffs-and-usage.md) — 정적 타입 트레이드오프, 실무 점유율, 프론트엔드 대세화 이유 및 순수 JS의 주력 영역

### Programming Language Comparison
* [현대 프로그래밍 언어의 패러다임 및 기능 크기 비교](./language-comparison/paradigm-and-feature-scale.md) — Swift(100%) 기준 Kotlin·TS·Python·JS 비교 및 주요 14개 언어 스펙 크기 순위
