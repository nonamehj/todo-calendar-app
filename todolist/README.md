# 📝 To-Do & 일정 관리 웹앱

## 📌 프로젝트 소개

일상 속 할 일과 일정을 효율적으로 관리하기 위한 투두리스트 & 달력 기반 일정 관리 웹앱입니다. 오늘의 할 일과 날짜별 일정을 직관적으로 확인하고 관리할 수 있도록 구현하였습니다.

👉 데모 페이지: 배포 링크 (예: https://username.github.io/todo-agenda)👉 만든 이유: 단순한 투두리스트를 넘어서 실제 일정 중심의 생활 관리를 고려한 프로젝트

## 🔧 주요 기능

### ✅ 공통

- 로컬스토리지 기반 데이터 저장 (새로고침에도 데이터 유지)

- 작성된 리스트는 수정, 삭제, 완료 체크 가능

- 완료 시 항목에 밑줄, 완료/미완료 상태에 따라 진행률(%) 표시

### 📅 일정 관리(Agenda)

- 달력 구현: 전달/다음달로 이동 가능

- 날짜 클릭 시 모달창으로 일정 추가

- 날짜마다 색상으로 상태 표시 (마감일, 일부 완료, 전체 완료 등)

- 오늘 이후 일정 기준 6개만 메인화면에 표시

### 🗒️ 오늘 할 일(TodoList)

- 간단한 할 일 입력 후 목록 표시

- 작성순 기준으로 메인화면에 최대 6개 미리보기

### 🏠 메인 화면(Home)

- 현재 날짜와 시간 표시 (Navbar)

- 오늘 할 일 & 오늘 이후 일정 요약 미리보기

- 메모장 기능 (메모 열고 닫기 가능)

## 🗂️ 폴더 구조 (src/)

```js
src/
├── components/
│   └── agenda/                 // 달력 + 일정 관리
│       ├── Agenda.jsx
│       ├── AgendaForm.jsx
│       ├── AgendaList.jsx
│       └── CalendarDays.jsx
│   └── alert/                  // 등록/삭제 알림
│       └── Alert.jsx
│   └── home/                   // 메인 화면 + 메모장 + 미리보기
│       ├── Home.jsx
│       ├── NotePad.jsx
│       ├── PreviewAgenda.jsx
│       ├── PreviewList.jsx
│       └── useHomeData.jsx
│   └── navbar/                 // 메뉴 버튼 + 날짜 시간
│       ├── MenuBtns.jsx
│       ├── Navbar.jsx
│       └── TodayDateTime.jsx
│   └── todolist/               // 오늘 할 일 목록
│       ├── Todolist.jsx
│       └── TodoListItems.jsx
├── context.js
├── agendaContext.js
└── listContext.js

```

## 🛠️ 사용 기술

- React (CRA 기반)

- React Hooks: useState, useEffect, useMemo, useCallback

- 커스텀 훅 사용

- React-icons 사용

- 일반 CSS만 사용하여 구성

## 🧠 트러블슈팅 & 경험

1. 날짜 저장 및 비교 이슈 (LocalStorage + Date 객체)

new Date() 객체를 그대로 로컬스토리지에 저장했을 때, 다시 불러오면 동일한 객체로 반환될 줄 알았으나 실제로는 문자열로 변환되어 반환되는 문제를 경험했습니다.
이는 JSON.stringify 과정에서 Date 객체가 자동으로 문자열로 직렬화되기 때문이었고, 날짜 비교나 연산 시 오류가 발생하였습니다.
이 문제를 해결하기 위해 데이터를 불러올 때 다음과 같이 new Date()로 다시 변환하여 처리했습니다:

return JSON.parse(agendaItems).map((agenda) => ({
...agenda,
date: new Date(agenda.date),
}));

또한 날짜 비교 시에는 객체끼리 비교할 수 없기 때문에 toLocaleDateString()을 활용해 문자열 기반 비교 방식으로 처리하였습니다:

new Date(a.date).toLocaleDateString() === new Date().toLocaleDateString()

이 과정을 통해 날짜 데이터 저장과 비교에서 발생할 수 있는 타입 이슈 및 포맷 일치의 중요성을 명확히 인식하게 되었습니다.

2. toISOString() 시간 오차 문제

처음에는 toISOString()을 사용해 날짜를 저장했지만, 한국 시간(KST)과 UTC의 시간 차이로 인해 하루가 밀리는 현상이 발생했습니다.
예를 들어 오늘 날짜를 저장했음에도 불러오면 하루 전으로 인식되는 문제가 발생했고, 일정이 의도와 다르게 표시되었습니다.
이를 해결하기 위해 toLocaleDateString()을 사용하여 로컬 시간 기준으로 날짜를 처리함으로써 정확한 비교와 출력이 가능해졌습니다.

## 🔍 개선 포인트 및 회고

listContext와 agendaContext를 각각 관리했지만 공통 로직이 많아 하나로 통합하여 관리할 수 있는 구조를 설계하는 것이 더 효율적이라고 판단됨

유사한 UI 구조(입력창, 버튼 등)를 컴포넌트로 분리하지 않아 코드 중복이 발생했으며, 추후 재사용 가능한 컴포넌트로 리팩토링이 필요함

일반 CSS로만 스타일링하여 전체적인 디자인이 단순하며, 향후 TailwindCSS 또는 Styled-components 도입을 고려하고 있음

처음에는 단순한 ToDo 기능만 구현했지만, 기능이 부족하다는 판단과 피드백을 반영하여 달력 기반 일정 관리, 퍼센트 진행률, 시각화 등 실제 사용성을 고려한 방향으로 재구성함

이번 프로젝트를 통해 사용자 중심의 기능 설계, 날짜 및 시간 처리, 유지보수와 확장성을 고려한 구조 설계의 중요성을 깊이 체감함

🙌 포트폴리오 외 다른 프로젝트와 연결하거나, 면접 시 참고할 수 있도록 정리했습니다.더 많은 포트폴리오 제작 팁은 👉 https://gptonline.ai/ko 에서 확인하실 수 있어요.

```

```

aaaaaaaaaaaaaaaaaaa new1
========= 새로 작성 =================

# 📅 Todo-Calendar-App

오늘의 일정과 날짜별 할 일을 손쉽게 관리하세요

## 프로젝트 소개

**Todo-Calendar-App**는 일상 속 할 일과 일정을 효율적으로 관리하기 위한 투두리스트 & 달력 기반 일정 관리 웹앱입니다. 오늘의 할 일과 날짜별 일정을 직관적으로 확인하고 관리할 수 있도록 구현하였습니다.

---

## 🎯 주요 기능 및 흐름

### 🛠️ 주요 기능

- **전체 구조**
- 단일 페이지 애플리케이션으로 라우터 없이 모든 기능이 컴포넌트 기반으로 구성됨

- 주요 컴포넌트: Home, TodoList, Agenda(달력 일정 관리), Alert, Navbar

- 전역 상태 관리를 위한 Context 3종 사용 (global, agenda, list)

- **오늘 할 일 기능 (TodoList)**

  - 인풋에 오늘 할 일을 작성 후 버튼 클릭으로 등록 가능

  - 등록된 할 일은 리스트 형태로 표시되고, 없을 경우 안내 문구 표시

  - 체크박스를 클릭하면 완료 상태로 변경되며, ‘완료 → 삭제’ 흐름 적용

  - 수정 버튼 클릭 시 인풋창에 기존 내용이 올라와 수정 가능

- **일정 관리 기능 (Agenda)**

  - 월 단위 달력 제공, 이전/다음 달 이동 가능

  - 날짜 선택 시 모달 창 열림, 일정 작성 가능

  - 일정 작성 시 해당 날짜에 색상으로 표시됨

  - 색상은 일정의 종류나 상태에 따라 구분 가능

  - 일정 리스트 수정 및 전체 삭제 기능 제공

- **메모장 및 미리보기 (Home)**

  - 오늘 할 일과 달력 일정 중 오늘 날짜에 해당하는 항목들을 미리보기로 표시

  - 최대 6개까지 표시되며, 많을 경우 전체 보기로 이동 유도

  - 간단한 메모장 기능 포함 (열기/닫기 토글 가능)

- **알림 기능 (Alert)**

  - 할 일 추가, 수정 완료, 삭제 등의 동작 후 알림 문구 자동 표시

  - 몇 초 후 자동으로 사라지며 사용자 방해 최소화

- **내비게이션**

  - 상단에 현재 시간과 날짜가 실시간으로 표시됨

  - 카테고리 버튼을 통해 홈, 오늘 할 일, 일정 관리로 빠르게 이동 가능

- **반응형 UI**

  - 주요 화면 크기 기준 (479px 이하, 480~767px, 768px 이상)에 따라 레이아웃 및 UI 요소 배치 최적화

### 👣 사용자 흐름

1. 화면 크기에 따라 자동으로 레이아웃이 조절됨
2. Home 화면에서 메모, 오늘할일, 일정관리 미리보기 확인 가능
3. 메뉴를 통해 메인화면, 오늘할일, 일정관리 화면으로 이동
4. 오늘 할 일:

- 인풋에 작성 후 등로하면 리스트에 항목 추가됨
- 항목을 수정하거나 완료 후 삭제 기능

5. 일정 관리:

- 달력에 날짜를 선택하여 일정 입력
- 일정이 등록된 날짜는 색상으로 표시되어 한눈에 파악 가능
- 기존 일정 수정 및 삭제 가능

6. 모든 주요 액션(등록, 수정, 삭제) 후 알림 문구 자동 표시

---

## 📁 폴더 구조 (src/)

```js
src/
├── components/
│   ├── agenda/
│   │   ├── Agenda.js
│   │   ├── AgendaStyle.css
│   │   ├── AgendaForm.js
│   │   ├── AgendaFormStyle.css
│   │   ├── AgendaList.js
│   │   ├── AgendaListStyle.css
│   │   ├── CalendarDays.js
│   │   └── CalendarDaysStyle.css.css
│   ├── alert/
│   │   ├── Alert.js
│   │   └── AlertStyle.css
│   ├── home/
│   │   ├── Home.js
│   │   ├── HomeStyle.css
│   │   ├── NotePad.js
│   │   ├── NotePadStyle.css.js
│   │   ├── PreviewAgenda.js      → css 파일 분리해서 만들기
│   │   ├── PreviewList.js        → css파일 분리해서 만들기
│   │   ├── useHomeData.js
│   │   └── useHomeData.js
│   ├── navbar/
│   │   ├── MenuBtns.js
│   │   ├── MenuBtnsStyle.css
│   │   ├── Navbar.js
│   │   ├── NavbarStyle.css
│   │   ├── TodayDateTime.js
│   │   └── TodayDateTimeStyle.css
│   ├── todolist/
│   │   ├── TodoList.js
│   │   ├── TodoListStyle.css
│   │   ├── TodoListItems.js
│   │   └── TodoListItemsStyle.css
├── App.js/
├── index.css/
├── context.js
├── agendaContext.js
└── listContext.js

```

## 📁 프로젝트 구조 및 주요 기능

### 📌 Agenda (agenda/Agenda)

- 메인 컨테이너 컴포넌트
- 월 단위 달력 표시 및 좌우 월 이동 버튼
- 일정 달력 시 달력에 색상으로 표시
- 하위 컴포넌트: `CalendarDays`, `AgendaForm`

### CalendarDays

- 달력 UI 렌더링
- 날짜 클릭 시 모달 열림 → 일정 입력 → 색상으로 시각화

### AgendaForm

- 특정 날짜 클릭 후 열리는 모달 내부
- 일정 입력 및 추가 버튼 제공
- 기존 일정이 없다면 "작성된 일정이 없습니다" 문구 표시
- 일정이 있다면 `AgendaList`를 통해 목록 출력
- 전체 삭제버튼 포함
- 하위컴포넌트: `AgendaList`

### AgendaList

- 입력된 일정을 리스트 형태로 출력
- 항목별로 수정 / 완료 / 삭제 기능

### 📌 Alert (alert/Alert)

- 입력/수정/삭제 등의 액션 후 메시지를 띄우는 알림
- 일정 시간 후 자동으로 사라짐

### 📌 Home (home/Home)

- 메모장 작성 기능 및 열리/닫기 기능
- 오늘 기준 작성된 할일/일정 미리보기 제공
- 미리보기 개수 초과시 문구로 상세 이동
- 하위 컴포넌트: `NotePad`, `PreviewAgenda`, `PreviewList`

### NotePad

- 메모장 작성 기능

### PreviewList / PreviewAgenda

- 오늘 기준으로 작성된 일정/할일 중 최대 6개 미리보기 출력

### useHomeData.js

- Home 컴포넌트 전용 커스텀 훅(데이터 관리 및 상태 처리)

### 📌 Navbar (navbar/Navbar)

- 상단 네비게이션 바 컨테이너
- 하위컴포넌트 : `MenuBtns`, `TodayDateTime`

### MenuBtns

- 메인, 오늘 할 일, 일정관리 탭 이동 버튼

### TodayDateTime

- 현재 날짜 및 실시간 시계 표시

### 📌 TodoList (todolist/TodoList)

- 오늘 할 일 입력/삭제/수정 기능
- 입력된 항목이 없다면 안내 문구 출력
- 항목 존재 시 전체 삭제 버튼 활성화
- 하위컴포넌트: `TodoListItems`

### TodoListItems

- 입력된 항목 리스트 출력
- 항목별로 수정 / 완료 / 삭제 기능

### 📌 App

- 라우팅 없이 모든 컴포넌트 직접 구성 및 사용

### 📌 context

- 전역에서 사용하는 상태 및 기능 제공

### 📌 agendaContext

- Agenda 관련 상태 관리 및 기능

### 📌 listContext

- TodoList 관련 상태 관리 및 기능

---

## 🛠️ 사용 기술 및 라이브러리

### ✅ CSS

- 일반 CSS 사용
- 전역 스타일은 최상위 태그에서만 설정
- 반응형 처리는 각 컴포넌트 내부에서 media query 사용
- flex, grid 레이아웃 기반

### 🔧 React 훅 및 라이브러리 사용 목록

✅ React 기본 훅

- useState, useEffect, useMemo, useCallback

✅ Context

- createContext, useContext, useGlobalContext

✅ 아이콘

- react-icons

### 🚀 설치 및 실행 방법

- npx create-react-app todolist
- cd todolist
- npm install
- npm start

---

## 💡 개발 목적 및 계기

**Todo-Scheduler**는 단순한 ToDo 기능에서 시작해 일정 관리의 필요성과 피드백을 반영하여, 달력 기반의 일정 관리 시스템으로 확장하게 되었습니다.
사용자가 일정을 직관적으로 확인하고 진행 상황을 파악할 수 있는 실용적인 기능 중심의 프로젝트를 목표로 했습니다.

## 🧠 개발하며 느낀 점

- 날짜 저장 및 비교처리에서 예상치 못한 이슈를 경험했습니다. `Date` 객체를 로컬스토리지에 저장하면 문자열로 변환되며, 이를 비교하거나 연산할 때 문제가 발생했습니다. 이를 해결하기 위해 `new Date()`로 재변환하고 `toLocalDateString()` 을 활용해 문자열 기반 비교로 안정성을 확보했습니다.
- `toISOString()` 사용 시 한국 시간과 UTC 간 차이로 인해 하루 오차가 발생하여, 로컬 시간 기준 처리의 중요성을 알게 되었습니다.
- 프로젝트를 진행하며 사용자 중심의 기능 설계, 날짜 및 시간 처리, 구조적 설계의 중요성을 체감했고, 특히 상태 및 데이터 흐름 관리의 정확성과 명확한 타입 설계가 기능의 신뢰성과 직결됨을 느꼈습니다.

## 🌱 개선점 및 향후 계획

- 일정 관련 로직과 리스트 관련 로직을 각각 따로 관리하면서도 공통되는 부분이 많았지만, 이를 효과적으로 통합하거나 추상화하는 방법을 구현하기 어려웠습니다. 다양한 방식으로 시도했지만 원하는 구조를 만들기엔 역부족이었고, 결국 중복되는 코드와 구조를 감수한 채 분리된 형태로 유지할 수밖에 없었습니다.
  앞으로 상태 관리와 컴포넌트 구조에 대해 더 깊이 배우고 나서, 이를 하나의 통합된 구조로 개선해보고자 합니다.
- 비슷한 형태의 UI나 기능도 상황에 따라 로직을 다르게 처리해야 했기 때문에 컴포넌트로 분리하거나 일관된 방식으로 재사용하기가 쉽지 않았습니다.
  향후에는 코드의 재사용성과 유지보수성을 고려한 설계를 학습한 뒤, 불필요한 중복 없이 더 정돈된 구조로 리팩토링할 계획입니다.
- 기본적인 일정 관리 외에도, 기한 초과 알림, 필터 기능, 통계 시각화 등 실용적인 기능을 점차 확장하여 사용자 경험을 높이는 방향으로 발전시킬 예정입니다.
- **Todo-Scheduler**를 진행하면서 상태 관리와 UI 처리, 날짜 계산 등 여러 기능을 직접 구현해보았습니다. 앞으로는 이 기능들을 좀 더 다듬고, 사용자 경험을 개선하는 데 집중하려고 합니다.
  또한, 현재는 화면에서만 처리하는 데이터 흐름을 실제 서버와 연결해보는 경험도 해보고 싶어, 나중에 API 연동을 공부하며 적용해볼 계획입니다. 이렇게 하면 만든 기능들이 실제 서비스에서 어떻게 작동하는지 이해할 수 있을 것 같아 기대하고 있습니다.

## 📸 프로젝트 데모 및 기타

📸 프로젝트 데모
👉 [https://nonamehj.github.io/todo-calendar-app](\https://nonamehj.github.io/todo-calendar-app)

💻 GitHub 코드
👉 [https://github.com/nonamehj/todo-calendar-app](https://github.com/nonamehj/todo-calendar-app)

---

// 새로작성2 new2

# 📌 프로젝트 이름

Todo-Calendar-App
👉 달력과 할 일 관리를 결합한 반응형 투두리스트 프로젝트

## 📖 프로젝트 소개

- 날짜별 일정 관리와 오늘 할 일 관리를 동시에 지원
- 반응형 UI (768px 이상: 그리드 구조 / 767px 이하: 세로 배치)
- 일정 상태에 따른 색상 구분, 알람 메시지 표시

---

## ⚙️ 주요 기능 및 흐름

### 주요 기능

- 오늘 할 일 작성 및 관리 (TodoList, 달력과 무관)
- 날짜별 일정 작성 및 관리 (Agenda)
- 일정 상태(진행, 마감, 완료, 지연)를 색상으로 구분
- 작성 / 수정 / 삭제 / 완료 시 알람 메시지 표시
- 미완료 일정 표시 방식
  - 메인화면 미리보기
    - 오늘 할 일: 달력과 무관, 작성된 미완료 일정 중 최대 몇 개까지만 표시
    - 일정 관리: 오늘 기준으로 작성된 일정부터 날짜 순으로 최대 몇 개씩 표시
      - 예시: 오늘 3개, 내일 2개, 다다음날 1개 순으로 표시
  - 달력: 오늘 기준으로 완료되지 않은 일정은 색상으로 표시
    - 일정이 남아 있으면 미완료 색상
    - 모든 일정 완료 시 완료 색상
    - 달력 상단에 상태별 색상 가이드 제공
- 로컬스토리지 연동 → 새로고침/재접속 시 데이터 유지

### 사용자 흐름

1. 메인화면 접속 → 현재 날짜 및 시간 확인
2. 메뉴 버튼 클릭 → 메인화면 / 오늘 할 일 / 일정 관리 선택
3. 오늘 할 일 작성 → 리스트에 추가 → 알람 메시지 표시
4. 일정 관리 선택 → 달력 확인 → 날짜 선택 → 일정 작성
5. 작성된 일정 → 상태에 따라 색상 표시
6. 체크/수정/삭제/완료 수행 → 알람 메시지 표시 → 리스트 업데이트
7. 메인화면에서 오늘 기준 미완료 일정 일부 미리보기 가능

---

## 🗂️ 폴더 구조

```js
src/
├── components/
│   ├── agenda/
│   │   ├── Agenda.js
│   │   ├── AgendaStyle.css
│   │   ├── AgendaForm.js
│   │   ├── AgendaFormStyle.css
│   │   ├── AgendaList.js
│   │   ├── AgendaListStyle.css
│   │   ├── CalendarDays.js
│   │   └── CalendarDaysStyle.css.css
│   ├── alert/
│   │   ├── Alert.js
│   │   └── AlertStyle.css
│   ├── home/
│   │   ├── Home.js
│   │   ├── HomeStyle.css
│   │   ├── NotePad.js
│   │   ├── NotePadStyle.css.js
│   │   ├── PreviewAgenda.js      → css 파일 분리해서 만들기
│   │   ├── PreviewList.js        → css파일 분리해서 만들기
│   │   ├── useHomeData.js
│   │   └── useHomeData.js
│   ├── navbar/
│   │   ├── MenuBtns.js
│   │   ├── MenuBtnsStyle.css
│   │   ├── Navbar.js
│   │   ├── NavbarStyle.css
│   │   ├── TodayDateTime.js
│   │   └── TodayDateTimeStyle.css
│   ├── todolist/
│   │   ├── TodoList.js
│   │   ├── TodoListStyle.css
│   │   ├── TodoListItems.js
│   │   └── TodoListItemsStyle.css
├── App.js/
├── index.css/
├── context.js
├── agendaContext.js
└── listContext.js

```

## 🧩 컴포넌트 설명

- **Agenda**
  - 달력 및 일정 관리 메인 컴포넌트
  - 달력(CalendarDays)과 일정 입력 모달(AgendaForm) 포함
- **CalendarDays**
  - 달력 UI
  - 특정 날짜에 작성된 일정 상태(오늘/진행/마감/지연/완료)를 색상으로 표시
  - 오늘·미래 날짜에 미완료 일정이 있으면 해당 날짜 색상으로 표시
  - 화면 크기(미디어쿼리)에 따라 일정 미리보기가 한 줄 또는 두 줄로 표시
- **AgendaForm**
  - 특정 날짜 클릭 시 모달로 열림
  - 일정 입력 가능, 내부에 AgendaList 포함
  - 입력/수정/삭제/완료 시 Alert 메시지 표시
- **AgendaList**
  - 작성된 일정 리스트 표시
  - 체크 여부에 따라 버튼 상태 변화 (수정/완료 ↔ 수정/삭제)
  - 완료된 항목은 취소선 표시 + 삭제 버튼만 남음
- **Alert**
  - 입력/수정/삭제/완료 시 몇 초간 표시되는 메시지 컴포넌트
- **Home**
  - 메인화면
  - NotePad, PreviewAgenda, PreviewList 포함
- **NotePad**
  - 단순 메모장
  - 메인화면 전용, 달력과 무관
  - 로컬스토리지 저장
- **PreviewAgenda / PreviewList**
  - 오늘 기준 미완료 일정 일부만 표시 (전부 X)
  - 진행/완료 상태 및 진행률 확인 가능
- **useHomeData**
  - 미완료 일정 추출
  - 진행률 계산 로직 포함
- **Navbar**
  - TodayDateTime + MenuBtns 포함
- **TodayDateTime**
  - 현재 날짜와 시간 표시
- **MenuBtns**
  - 메인화면 / 일정관리 / 오늘할일 화면 이동 버튼
- **Todolist**
  - 오늘할일 작성 컴포넌트
  - Agenda 구조와 유사
  - 내부에 TodolistItems 포함
- **TodolistItems**
  - 오늘할일 리스트 출력
  - 체크 시 상태(수정/완료 ↔ 수정/삭제) 변경

## 🛠️ 사용 기술 및 라이브러리

## 🎯 개발 목적 및 계기

## 💡 개발하며 느낀점

## 🚀 개선점 및 향후 계획

---

new3

# todo-calendar-app

날짜별 일정 관리와 오늘 할 일을 통합한 달력형 투두리스트 애플리케이션

## 프로젝트 소개

`todo-calendar-app`은 날짜 기반의 일정 관리와 오늘 해야 할 일을 각각 기록할 수 있는 투두리스트 애플리케이션입니다.

- **일정 관리**: 달력에서 날짜를 선택해 해당 날짜의 할 일을 작성/관리
- **오늘 할 일**: 오늘 하루에 해야 할 일을 기록하는 단순 리스트
- **공통 기능**: 작성/수정/삭제/완료 시 알람 메시지 표시, 로컬스토리지 저장으로 데이터 유지
- **UI 특징**: 화면 크기에 따라 달라지는 반응형 레이아웃
  - 767px 이하: 세로 배치(위→아래: 현재 날짜/시간 → 메뉴 버튼 → 콘텐츠)
  - 768px 이상: 그리드 배치(좌측: 시계/버튼, 우측: 메인 콘텐츠)

---

## ⚙️ 주요 기능 및 흐름

**주요 기능**

- **달력 기반 일정 관리(Agenda)**

  - 날짜 선택 후 해당 날짜의 할 일을 작성
  - 상태(진행/마감/지연/완료)를 색상으로 표시
  - 색상 규칙(오늘 기준 상태에 따라 표시)
    - 아직 끝나지 않은 일정이 있는 날짜: 미완료 색상
    - 오늘 날짜에 미완료 일정이 있으면: 오늘용 색상
    - 앞으로 수행 예정(시간이 남아 있는) 일정: 예정 색상
    - 해당 날짜의 모든 일정이 완료된 경우: 완료 색상  
      ※ 한 날짜에 하나라도 미완료가 있으면 “미완료”로 표시
  - 달력 상단에 상태별 색상 가이드 제공

- **오늘 할 일(Todolist)**
  - 달력과 무관하게 오늘 해야 할 일만 기록/관리
- **미리보기(Preview)**

  - 메인화면에 두 영역 미리보기
    1. **오늘 할 일 미리보기**: 미완료 항목 최대 6개 순차 표시
    2. **일정 관리 미리보기**: 오늘 기준 미완료 일정만 날짜 순으로 최대 6개 순차 표시
  - 완료된 항목은 미리보기에 포함되지 않음

- **알람 메시지(Alert)**

  - 작성/수정/삭제/완료 시 잠깐 표시되는 안내 메시지

- **데이터 유지**
  - 로컬스토리지 저장으로 새로고침/재접속 시 데이터 유지

---

**사용자 흐름**

1. 메인화면 접속 → 현재 날짜/시간 확인
2. 메뉴 버튼으로 `메인화면 / 오늘 할 일 / 일정 관리` 이동
3. `오늘 할 일` 작성 → 리스트 추가 → 수정/완료/삭제
   - 완료 시 취소선 처리, 버튼 상태 변화
4. `일정 관리`에서 달력 확인 → 날짜 선택 → 해당 날짜의 할 일 작성
5. 리스트 상태 변경 규칙
   - 작성 직후: “수정/완료” 버튼
   - 체크 시: “수정/삭제” 버튼으로 전환 (알람 없음)
   - 완료 시: 텍스트 취소선 + 삭제 버튼만 유지
6. 달력 색상 규칙에 따라 즉시 반영
7. 메인화면 미리보기
   - 오늘 할 일: 미완료 항목을 최대 6개 순차 표시
   - 일정 관리: 오늘 기준 미완료만 날짜 순으로 최대 6개 순차 표시

---

## 🗂️ 폴더 구조

```js

src/
├── components/
│   ├── agenda/
│   │   ├── Agenda.js
│   │   ├── AgendaStyle.css
│   │   ├── AgendaForm.js
│   │   ├── AgendaFormStyle.css
│   │   ├── AgendaList.js
│   │   ├── AgendaListStyle.css
│   │   ├── CalendarDays.js
│   │   └── CalendarDaysStyle.css.css
│   ├── alert/
│   │   ├── Alert.js
│   │   └── AlertStyle.css
│   ├── home/
│   │   ├── Home.js
│   │   ├── HomeStyle.css
│   │   ├── NotePad.js
│   │   ├── NotePadStyle.css.js
│   │   ├── PreviewAgenda.js      → css 파일 분리해서 만들기
│   │   ├── PreviewList.js        → css파일 분리해서 만들기
│   │   ├── useHomeData.js
│   │   └── useHomeData.js
│   ├── navbar/
│   │   ├── MenuBtns.js
│   │   ├── MenuBtnsStyle.css
│   │   ├── Navbar.js
│   │   ├── NavbarStyle.css
│   │   ├── TodayDateTime.js
│   │   └── TodayDateTimeStyle.css
│   ├── todolist/
│   │   ├── TodoList.js
│   │   ├── TodoListStyle.css
│   │   ├── TodoListItems.js
│   │   └── TodoListItemsStyle.css
├── App.js/
├── index.css/
├── context.js
├── agendaContext.js
└── listContext.js

```

---

## 🧩 컴포넌트 설명

- **agenda/**

  - `Agenda`: 일정 관리 메인 컴포넌트(달력/입력/리스트 포함)
  - `CalendarDays`: 달력 UI + 날짜별 상태 색상 표시
  - `AgendaForm`: 선택 날짜에 대한 입력 모달
  - `AgendaList`: 해당 날짜의 리스트 출력

- **alert/**

  - `Alert`: 입력/수정/삭제/완료 시 잠깐 표시되는 메시지

- **home/**

  - `Home`: 메인화면 컨테이너
  - `Notepad`: 간단 메모장(로컬 저장), 달력과 무관
  - `PreviewAgenda`: 일정 관리 미리보기(미완료만)
  - `PreviewList`: 오늘 할 일 미리보기(미완료만)
  - `useHomeData`: 진행률 계산 및 미완료 데이터 추출

- **navbar/**

  - `Navbar`: 상단 바, 내부에 현재 시간 과 메뉴버튼 포함
  - `TodayDateTime`: 현재 날짜/시간 표시
  - `MenuBtns`: 메인/오늘 할 일/일정 관리 이동 버튼, 버튼 클릭 시 해당 화면 표시

- **todolist/**
  - `Todolist`: 오늘 할 일 입력
  - `TodolistItems`: 오늘 할 일 리스트

---

## 🛠️ 사용 기술 및 라이브러리

- React Hooks: `useState`, `useEffect`, `useMemo`, `useCallback`, `useRef`

- Context API: `createContext`, `useContext`, `useGlobalContext`

- UI 아이콘: `react-icons`

---

## 🎯 개발 목적 및 계기

- 프로젝트 경험을 쌓고 기본 기능 구현을 연습하기 위해 투두리스트 제작

- 리스트를 작성하고 관리할 때 사용자가 편하게 사용할 수 있는 기능 구현을 목표

- 입력한 데이터가 사라지지 않고 유지되도록 로컬 저장 기능 적용

- 데이터 처리와 UI 구성, 입력-저장-조회 흐름을 직접 체험하며 이해

- 기본적인 기능 구현과 데이터 관리 능력을 체계적으로 익히는 것

---

## 💡 개발하며 느낀 점

- 재접속 시 작성한 리스트가 사라지는 문제를 겪으며, 데이터를 안정적으로 저장하는 방법의 중요성을 깨달음

- 달력을 활용하며 new Date와 JSON을 이용한 로컬 저장/불러오기 과정에서 발생한 다양한 문제를 경험하고, 데이터 처리 과정에 대한 이해를 높임

- 날짜 비교 과정에서 저장 시점과 비교 대상의 날짜가 일치하지 않는 문제를 해결하며, 실제 문제 해결 과정에서 시행착오를 통해 배운 경험을 얻음

- 이러한 경험을 통해 기능 구현 과정에서 발생할 수 있는 문제를 직접 겪고 처리하는 경험을 쌓음

- 이러한 경험을 통해 기능 구현 과정에서 발생할 수 있는 문제를 직접 겪고 해결하며 학습하는 경험이 프로젝트에서 중요한 부분임을 깨달음

## 🔮 개선점 및 향후 계획

- 공통 기능 재활용을 위한 Context 구조 개선 및 컴포넌트 재사용성 강화

- 폼과 리스트 구조 개선으로 유지보수성과 코드 가독성 향상

- 달력 UI 확장, 반복 일정 관리, 우선순위 설정, 알림 기능 등 추가 기능 구현 검토

- 향후 다양한 화면 크기 대응과 반응형 설계 최적화

=

## 📸 프로젝트 데모 및 기타

📸 프로젝트 데모
👉 [https://nonamehj.github.io/todo-calendar-app](https://nonamehj.github.io/todo-calendar-app)

💻 GitHub 코드
👉 [https://github.com/nonamehj/todo-calendar-app](https://github.com/nonamehj/todo-calendar-app)
