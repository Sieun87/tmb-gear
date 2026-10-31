# TMB 기어 리스트

TMB 백패킹 준비용 개인 장비 목록 앱 (PWA). GitHub Pages로 배포: https://sieun87.github.io/tmb-gear/

- 빌드 도구 없음. `index.html` 하나에 HTML/CSS/JS가 모두 들어 있음.
- 데이터는 휴대폰의 localStorage(`tmb-gear-v1`)에만 저장. 데이터 구조를 바꿀 때는 기존 데이터를 읽을 수 있게 유지할 것.
- `sw.js`가 앱 파일을 캐시해 오프라인에서도 열림. 앱 파일을 바꾸면 `sw.js`의 `CACHE` 버전도 올릴 것.
- 미리보기: `.claude/launch.json`의 `tmb-gear` (python http.server, 8765 포트).
- `main`에 push하면 1~2분 뒤 Pages에 반영됨.
