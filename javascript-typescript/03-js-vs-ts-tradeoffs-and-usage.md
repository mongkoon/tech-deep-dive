# TypeScript의 트레이드오프와 JS/TS 실무 생태계

> 키워드: `#JavaScript` `#TypeScript` `#StaticTyping` `#WebFrontend` `#Runtime`

---

## Q1. 동적 객체 생성의 강점을 가진 JS에 정적 타입을 적용한 TS는 JS의 장점을 버린 것인가?

자유로운 동적 유연성의 일부를 포기하는 대신 **대규모 프로젝트의 안정성과 유지보수성을 얻는 트레이드오프**를 선택한 것입니다. 다만 TypeScript는 **구조적 타이핑(Duck Typing)**, **타입 추론**, 그리고 `Record<string, any>`나 `unknown` 같은 탈출구를 통해 JavaScript 특유의 유연함을 최대한 살리는 방향으로 설계되었습니다.

---

## Q2. 현재 JavaScript와 TypeScript의 실무 사용 비율은?

| 기준 | TypeScript | JavaScript | 특징 |
| :--- | :---: | :---: | :--- |
| **웹·프론트엔드 전문 실무** | **약 75 ~ 80%** | **약 20 ~ 25%** | 신규 프로젝트의 사실상 표준 |
| **전체 개발자 (전 분야 통합)** | **약 40%** | **약 60%** | 레거시, 간단한 스크립트 포함 |

---

## Q3. 웹/프론트엔드에서 TypeScript가 대세가 된 이유는?

1. **프론트엔드 코드의 거대화 (SPA 시대)**: 복잡한 상태(State) 관리에서 런타임 전 오류 검출이 필수화되었습니다.
2. **컴포넌트와 API 데이터의 '계약서' 역할**: 백엔드 응답과 Props 스펙을 코드 자체로 문서화하여 협업 비용을 줄입니다.
3. **압도적인 IDE 지원과 리팩토링 안정성**: 자동 완성과 일괄 이름 변경(Rename)으로 대규모 수정의 안전성을 보장합니다.
4. **점진적 도입이 가능한 구조 (JS의 슈퍼셋)**: `.js` 파일을 하나씩 `.ts`로 전환할 수 있어 도입 장벽이 낮았습니다.
5. **프레임워크와 생태계의 기본값화**: Angular, Vue 3, Next.js, Vite 등이 TS를 기본 옵션으로 채택했습니다.

---

## Q4. 웹/프론트엔드 외에 순수 JavaScript가 여전히 강력하게 쓰이는 영역은?

* **서버리스 및 엣지 컴퓨팅**: AWS Lambda, Cloudflare Workers 등 빠른 Cold Start가 중요한 환경
* **백엔드 API 서버 및 실시간 서비스**: Node.js, Express, Fastify 기반 I/O 중심 서비스 및 빠른 MVP 구축
* **업무 자동화 및 SaaS 내부 스크립팅**: Google Apps Script, n8n, Zapier 커스텀 스크립트
* **빌드·배포 스크립트 및 CLI 도구**: 설정 파일 및 `node script.js` 즉시 실행 자동화 도구
* **크로스 플랫폼 앱 및 브라우저 확장 프로그램**: Electron, React Native, Chrome Extensions
