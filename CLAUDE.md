# TMB

TMB(Tour du Mont Blanc) 백패킹 준비용 개인 앱 (PWA). GitHub Pages로 배포: https://sieun87.github.io/tmb-gear/

- 빌드 도구 없음. `index.html` 하나에 HTML/CSS/JS가 모두 들어 있음.
- 화면 설계 시안: `design/preview.html` (무드, 컴포넌트, 모든 화면, 흐름 규칙). 앱을 바꿀 때 시안의 "흐름 규칙"을 따를 것.
- 데이터는 휴대폰의 localStorage(`tmb-app-v2`)에만 저장. 구조: `cats, gear, wish, trips, stays, huts`. 데이터 구조를 바꿀 때는 기존 데이터와 백업 파일을 읽을 수 있게 유지할 것.
- 처음 실행할 때만 목록마다 예시 1개를 넣음(`seed()`). 다시 넣지 않음.
- `sw.js`가 앱 파일을 캐시해 오프라인에서도 열림. 앱 파일을 바꾸면 `sw.js`의 `CACHE` 버전도 올릴 것.
- 미리보기: `.claude/launch.json`의 `tmb-app` (python http.server, 8766 포트). 앱은 `/`, 시안은 `/design/preview.html`.
- `main`에 push하면 1~2분 뒤 Pages에 반영됨.
