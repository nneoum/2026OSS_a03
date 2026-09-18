# 2026 OSS 과제 03 - 프론트엔드 CRUD UI 및 폼 유효성 검사 (assign03-c01-22300272)

서버나 DB 없이 HTML/CSS/JavaScript만으로 음악 플레이리스트의 목록·추가·상세·수정 화면을 구현하고, 폼 필수 입력 지정과 JS 유효성 검사, Bootstrap을 병행한 반응형 디자인까지 적용한 과제입니다.

- **학번**: 22300272
- **이름**: 박동민

## Pages

| 페이지 | 설명 | URL |
|---|---|---|
| index.html | 음악 목록 (표에서 곡 선택 시 상세로 이동) | https://2026-oss-a03.vercel.app/ |
| add.html | 음악 추가 폼 (6개 필드, required + JS validation) | https://2026-oss-a03.vercel.app/add.html |
| view.html | 상세보기 (URL의 `?id=`로 클릭한 곡의 전체 필드 표시) | https://2026-oss-a03.vercel.app/view.html?id=1 |
| edit.html | 수정 폼 (기존 값 자동 채움, `?id=`로 원래 항목 유지) | https://2026-oss-a03.vercel.app/edit.html?id=1 |

## Vercel Deploy URL
- https://2026-oss-a03.vercel.app/

## GitHub Repository
- **Repo URL**: https://github.com/2026-2-OSS/assign03-c01-22300272
- **Commit History**: https://github.com/2026-2-OSS/assign03-c01-22300272/commits/main/

## Development Flow

VS Code → Git → GitHub → Vercel

1. VS Code에서 코드 작성
2. Git으로 버전 관리 (add, commit)
3. GitHub에 push
4. Vercel에서 자동 배포

---

## Weekly Review – Week 3

### example.html (Bootstrap Album 참고 예제)

Bootstrap 공식 Album 예제(`getbootstrap.com/docs/5.3/examples/album`)를 로컬로 가져와, 카드 3개의 `<svg>` placeholder 이미지를 `<img src="cj1.jpg">` / `cj2.jpg` / `cj3.jpg` 실제 이미지로 교체해 실습용으로 활용함. 레이아웃 구조(navbar, hero 섹션, `row row-cols-*` 카드 그리드, footer)와 다크 테마 토글(`data-bs-theme`) 등은 원본 예제를 그대로 유지하고, 이미지 삽입 방식만 커스터마이징함.

- **목적**: Bootstrap 그리드 시스템(`row-cols-1 row-cols-sm-2 row-cols-md-3`)과 카드 컴포넌트 구조를 익히기 위한 참고/연습용 예제

- **수정 내용**: `<svg class="bd-placeholder-img">` placeholder → `<img src="cjN.jpg" class="bd-placeholder-img card-img-top" style="height:225px; object-fit:cover; width:100%;">`로 교체
- **위치**: 실제 과제 페이지(index/add/view/edit)와는 별개로, 디자인 참고용 예제 파일로 보관

### Service Topic
음악 플레이리스트 CRUD 프론트엔드 서비스. 곡 목록 조회, 추가, 상세보기, 수정 기능을 서버/DB 없이 JS 배열(`data.js`)과 URL 쿼리스트링만으로 구현함.

### Data Fields
| 필드 | 설명 |
|---|---|
| title | 곡 제목 |
| artist | 아티스트명 |
| album | 앨범명 |
| releaseDate | 발매일 |
| duration | 재생시간 |
| playCount | 재생횟수 |
| genre | 장르 (select 옵션 11종: K-Pop, Pop, Hip-Hop, R&B, Rock, Jazz, Classical, EDM, Indie, OST, Ballad) |

### List Page
index.html 표에서 번호, 제목, 아티스트, 장르, 발매일 5개 필드를 표시. 제목 또는 상세보기 버튼 클릭 시 `view.html?id=`로 이동해 해당 곡의 상세 정보 확인 가능.

### Validation
add.html, edit.html 공통 적용 (`checkAddForm()` / `checkEditForm()`):
1. 필수 입력 검사 — title/artist/album/duration/playCount 공백 여부 확인
2. 제목 길이 검사 — 2자 이상 50자 이하
3. 재생횟수 형식·범위 검사 — 숫자 형식(`isNaN` 체크) + 0~100,000,000 범위
4. 발매일 필수 여부 검사
5. 장르 선택 필수 검사 (미선택 시 빈 값 차단)

HTML `required` 속성(브라우저 레벨 검증)과 JS 함수(스크립트 레벨 검증)를 이중으로 적용해, 단순 공백 체크를 넘어서는 세부 규칙까지 검사함.

### RWD
Bootstrap 그리드 시스템(`row g-3`, `col-md-6`, `col-12`)을 사용해 반응형 폼 레이아웃 구성. 데스크탑(md 이상, 768px 이상)에서는 입력 필드가 2열로 배치되고, 모바일(md 미만)에서는 `col-md-6`이 100% 너비로 전환되어 1열로 쌓이는 구조. `.container` 클래스로 좌우 여백과 최대 너비를 화면 크기별로 자동 조정함.

### Bootstrap
- `.navbar`, `.navbar-brand`, `.navbar-nav`, `.nav-link` — 헤더 네비게이션
- `.container` — 레이아웃 폭 제한 및 반응형 여백
- `.row`, `.col-md-6`, `.col-12`, `g-3` — 반응형 그리드 시스템
- `.form-control`, `.form-select` — 입력 필드 및 드롭다운 스타일
- `.btn`, `.btn-primary`, `.btn-secondary` — 버튼
- `.alert`, `.alert-info` — 필수 입력 안내 문구
- `.table` — 음악 목록 테이블

### Problem & Solution
실습 중 발생한 문제와 해결 과정

- **문제**: view.html이 목록의 어떤 곡을 클릭해서 들어가도 항상 첫 번째 곡(Dynamite) 정보만 하드코딩되어 보여서, "상세보기"가 사실상 의미가 없었음
- **해결**: index.html의 각 링크를 `view.html?id=1`처럼 쿼리스트링으로 바꾸고, `data.js`에 곡 데이터를 배열로 모아둔 뒤 `URLSearchParams`로 `id`를 읽어 해당 곡 정보만 DOM에 채우도록 구조를 바꿈. edit.html도 동일한 방식으로 연결해서 수정 후 원래 곡의 상세 페이지로 돌아가게 함

### Key Learning
이번 주 배운 핵심 내용 3가지

1. `required` 속성(HTML5 레벨 검증)과 JS `if`문 검증(스크립트 레벨 검증)은 서로 다른 층위라서, "필수 입력 지정"을 제대로 하려면 둘 다 챙겨야 한다는 걸 알게 됨
2. 서버/DB 없이도 URL 쿼리스트링(`?id=`)과 JS 배열 조회를 조합하면, 목록에서 클릭한 항목에 맞는 상세 데이터를 동적으로 보여줄 수 있다는 걸 배움
3. 기존 커스텀 CSS 위에 Bootstrap을 얹을 때는 `.container`, `.card`, `.btn-primary`처럼 클래스 이름이 겹치는 부분의 로드 순서(내 CSS를 나중에 불러야 내 스타일이 이김)를 신경 써야 한다는 걸 확인함

### AI Usage
AI(Claude)를 활용해 과제 요구사항 대조 점검과 오류 찾기, README 작성을 진행함.

**요구사항 대조 점검**
- 입력 필드 개수, Form Element 적합성, 필수 입력 지정, JS Validation 개수 등 과제 체크리스트를 코드와 하나씩 대조받음
- 이 과정에서 `required` 속성이 라벨의 `*` 표시만 있고 실제 input에는 빠져있었던 걸 발견해서 추가함

**동적 상세 페이지 리팩터링**
- view.html/edit.html이 클릭한 항목과 무관하게 고정 데이터만 보여주는 구조적 한계를 짚어준 뒤, `data.js` + 쿼리스트링 기반으로 바꾸는 작업을 같이 진행함

### 새롭게 알게 된 점 또는 궁금한 점
- 코드 외적으로 알게된 점으로, C 언어나 자바처럼 문법을 외워두고 여러 실습을 거치면서 하지 않고도, 코드 문법을 거의 모르더라도, 실습 (웹 서비스 개발 과목에서 하는 실습 포함) 몇 번으로도 충분히 익힐 수 있다는 점이
프론트엔드 학습의 장점임을 몸소 느꼈다.