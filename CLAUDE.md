# TMB

TMB(Tour du Mont Blanc, 2027.07.10~07.20, 10박 12일) 백패킹 준비용 개인 앱 (PWA). 사용자 본인 휴대폰에만 홈 화면 앱으로 설치해서 씀.
배포: GitHub Pages https://sieun87.github.io/tmb-gear/ (저장소 `Sieun87/tmb-gear`, 브랜치 `main`, `gh` CLI 로그인돼 있음).

## 사용자와 일하는 방식
- 사용자는 한국어로 말하고 개발자가 아님. 답은 한국어로, 쉬운 말로, 짧게. 폰에서 해야 할 일은 단계로 적어 줄 것.
- 사용자의 실제 데이터(장비 77개 이상 등록)는 휴대폰에만 있음. **데이터가 날아가면 안 됨**이 최우선 조건.
  - 데이터 구조를 바꾸면 기존 데이터와 예전 백업 파일을 그대로 읽을 수 있어야 함 (`valid()` / `upgrade()`에서 처리).
  - 위험한 변경 전에는 Main 맨 아래 [내보내기]로 백업하라고 먼저 안내.
- "올려줘" = 커밋하고 `main`에 push. push 뒤 `curl -s https://sieun87.github.io/tmb-gear/sw.js`로 새 `CACHE` 버전이 보이는지 확인하고 알려 줌 (보통 1분 안).
- 폰 반영 안내: 홈 화면 앱을 완전히 닫았다가 다시 열기 (안 바뀌면 한 번 더).
- 화면을 바꿀 땐 미리보기로 직접 확인한 뒤 결과를 보여 줄 것.

## 파일
- `index.html` — 앱 전체 (HTML/CSS/JS 한 파일, 빌드 도구 없음).
- `sw.js` — 네트워크 우선 캐시(오프라인용). **앱 파일을 바꾸면 `CACHE` 버전(`tmb-app-vN`)을 꼭 올릴 것.**
- `manifest.webmanifest`, `icon.svg`, `icon-180/192/512.png` — 앱 이름 "TMB", 파스텔 노을 산 아이콘. PNG는 svg에서 `qlmanage`/`sips`로 만듦.
- `design/preview.html` — 화면 설계 시안 (무드, 컴포넌트, 모든 화면, "흐름 규칙"). 앱을 바꿀 때 흐름 규칙을 따를 것. 일부 예전 이름(준비 등)이 남아 있음.

## 미리보기
- `.claude/launch.json`의 `tmb-app` (python http.server, 8766). preview_start가 안 되면 Bash 백그라운드로 `python3 -m http.server 8766 --directory <프로젝트>` 실행 후 브라우저 창에서 `http://localhost:8766/?nocache=N` 열기 (캐시 때문에 쿼리 붙임).
- 시안은 `/design/preview.html`.

## 디자인 원칙
- 파스텔(블러시/라벤더), Noto Sans KR, 둥근 카드, 산 일러스트. 작은 글씨.
- 한 항목 = 한 줄. 등록은 화면 아래 고정 입력줄 + "상세 입력"으로 펼침. 버튼은 아이콘(등록은 +).
- 저장 버튼 없음: 바꾸면 바로 저장. 삭제는 확인 창.
- 순서 변경은 줄을 길게 눌러 끌기(손잡이 아이콘).

## 화면 구성
- Main: 위 작은 글씨 "2027.07.10~2027.07.20 (10박 12일)", 큰 글씨 "TMB", 검색 아이콘. 카드 가로 스와이프(scroll-snap) — 순서 **Plan · Gear · Journey · Stay · Hut**, 카드에 따라 아래 패널이 바뀜. 맨 아래 백업 [내보내기]/[불러오기].
- Plan: 체크리스트. 항목마다 링크 여러 개 + 메모. 기한 없음. 다른 페이지로 연결 안 함.
- Gear: 카테고리 목록(무게 비율 막대, 순서 변경, 16색). 카테고리 상세(장비 줄, 수량 스테퍼, 길게 눌러 순서 변경, 보유 장비에서 추가). 카테고리 편집. 장비현황 탭 **보유 장비 / 위시리스트**(가격·판매처·링크, [구매 완료] 하면 보유로 이동). 장비 하나는 카테고리 하나에만.
- Journey: 일정 지도. 줄의 +로 그 뒤에 끼워 넣기. 길게 눌러 순서 바꿀 때 일차(D1, D2…)는 자리에 남음 (D5를 D2 자리로 옮기면 그게 D2가 됨). 상세/편집 화면에서 수정·삭제. 국가는 도착지 기준. 묵는 곳은 Stay 목록에서 선택(모든 숙소 보이고 연결된 일차 표시). 들르는 산장은 여러 줄, 아직 연결 안 된 산장만 선택 가능.
- Stay(숙박, 잠자는 곳): 정보만 — 이름, 국가(스위스/프랑스/이탈리아), 종류(캠핑장/숙소/산장/야영), 체크인, 위치, 가격, 연락처, 시설, 메모. 일차 없음.
- Hut(산장, 물·전기·식사 들르는 곳): 이름, 국가, 가는 길, 결제, 연락처, 시설, 메모.
- 검색: 모든 목록에서 찾기 (한글로 라틴 이름은 못 찾음 — 알려진 한계).

## 데이터
- localStorage 키 `tmb-app-v2`, 형태 `{v:2, cats, gear, wish, trips, stays, huts, todos}`.
  - `todos`는 나중에 추가돼서 없으면 `upgrade()`가 빈 목록으로 채움.
  - 카테고리 안 장비 순서는 선택 필드 `gear[].pos`(없으면 등록 순서 `at`). 카테고리를 바꾸면 `pos`를 지움.
  - `trips[]`는 `stayId`, `hutIds`(배열) 등을 가짐.
- 처음 실행할 때만 목록마다 예시 1개(`seed()`). 다시 넣지 않음.

## 코드 구조 (index.html 스크립트)
- 상태 `D`(데이터), `U`(화면 상태: `stack` 라우터, `draft`, `sheet`, `dialog` 등). `commit(fn)` = 바꾸고 저장하고 다시 그림.
- 화면 함수는 `S.<이름>`, Main 패널은 `PANELS = [mainPlan, mainGear, mainTrips, mainStays, mainHuts]`.
- 이벤트는 위임: `data-act` → `A` 액션, `data-form` → `F` 제출 처리.
- 뒤로가기: `go()`/`back()`이 history와 연결. 검색·시트·확인창은 `openLayer`/`closeLayer`. 확인창 OK 동작은 `U.afterClose`로 popstate 뒤 실행.
- 입력 보존: `data-keep` 입력값은 다시 그릴 때 `U.draft`에 보관. 값 바꿀 땐 `setD(id,v)`, 비울 땐 `clearDraft(prefix)`.
- 길게 눌러 끌기: `data-drag-list`/`data-drag-id`, 380ms. 끝난 뒤 350ms 동안 탭 무시(`justDropped`) — 테스트할 땐 400ms 이상 기다릴 것.
- 링크는 `safeUrl()`로 https 붙여 정리 (입력칸은 `type=url` 대신 `inputmode="url"`).
