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

---

## 🔮 개선점 및 향후 계획

- 공통 기능 재활용을 위한 Context 구조 개선 및 컴포넌트 재사용성 강화

- 폼과 리스트 구조 개선으로 유지보수성과 코드 가독성 향상

- 달력 UI 확장, 반복 일정 관리, 우선순위 설정, 알림 기능 등 추가 기능 구현 검토

- 향후 다양한 화면 크기 대응과 반응형 설계 최적화

---

## 📸 프로젝트 데모 및 기타

📸 프로젝트 데모
👉 [https://nonamehj.github.io/todo-calendar-app](https://nonamehj.github.io/todo-calendar-app)

💻 GitHub 코드
👉 [https://github.com/nonamehj/todo-calendar-app](https://github.com/nonamehj/todo-calendar-app)
