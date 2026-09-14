# frontend_home

모바일 브라우저 동작을 확인하기 위한 정적 웹 테스트 모음입니다.
빌드 도구 없이 HTML/CSS/JS 파일만으로 구성되며, 그대로 열거나 정적 호스팅에 올려서 사용합니다.

## 구조

```
index.html                     테스트 목록 허브
assets/base.css                공통 스타일
tests/<test-name>/index.html   개별 테스트 페이지
archive/                       이전 프로젝트 파일 보관
```

## 테스트 목록

| # | 테스트 | 대상 브라우저 | 경로 |
|---|--------|---------------|------|
| 01 | 식권대장 OneLink 딥링크 | 삼성 인터넷 | `tests/onelink-deeplink/` |

## 실행 방법

### 1. 로컬 서버 실행

```bash
python3 -m http.server 8000
```

- PC: http://localhost:8000
- 휴대폰(같은 Wi-Fi): `http://<맥 IP>:8000`

맥 IP 확인:

```bash
ipconfig getifaddr en0
```

휴대폰에서 주소를 직접 입력하는 대신, PC에서 테스트 페이지를 열고 **06 QR 코드**를 스캔하면
같은 페이지가 휴대폰에서 열립니다.

### 2. GitHub Pages

저장소 Settings → Pages → Source를 `main` 브랜치 `/ (root)`로 지정하면
`https://dohyun82.github.io/frontend_home/` 로 접근할 수 있습니다.
휴대폰에서 로컬 네트워크 없이 열어야 할 때 사용합니다.

## TEST 01 · 식권대장 OneLink 딥링크

AppsFlyer OneLink(`vendys.onelink.me/szhg`)를 통해 식권대장 앱의 OAuth 화면으로 진입하는지 확인합니다.

**기대 동작**

- 앱 설치됨 → 식권대장 앱 실행, `sikdae://screen/main/oauth` 진입
- 앱 미설치 → 스토어로 이동

**페이지 기능**

- 원본 URL 그대로 실행 / 파라미터를 바꿔 재생성한 URL 실행
- 실행 방식 비교: `a href`, `window.open`, `location.href`, `assign`, `replace`
- URL 계층 디코딩 (OneLink 쿼리 → `deep_link_value` → `payload.url` → `q`)
- 원본 URL과의 차이 비교 (값 동일 / 인코딩만 다름 / 값 변경됨)
- `visibilitychange` 기반 앱 전환 감지 로그 (sessionStorage에 유지)
- 실행 환경(삼성 인터넷 여부) 표시

**URL 인코딩 참고**

원본 URL의 `q` 파라미터는 `callbackUrl`의 `:` `/` 가 인코딩되지 않은 상태로 들어 있습니다.
페이지에서 파라미터를 수정해 재생성하면 `encodeURIComponent`로 전부 인코딩되어 URL 문자열이 달라지지만,
앱이 디코딩해 읽는 값은 동일합니다. 이 차이는 **04 URL 분석**에 표시됩니다.
인코딩까지 원본 그대로 검증해야 한다면 **03 → 원본값으로 초기화**를 눌러 원본 URL로 되돌리세요.

## 새 테스트 추가

1. `tests/<test-name>/index.html` 생성
2. `<link rel="stylesheet" href="../../assets/base.css">` 연결
3. `<meta name="robots" content="noindex, nofollow" />` 추가 (아래 "공개 범위" 참고)
4. `index.html`의 테스트 목록에 `a.test-item` 항목 추가
5. 이 README의 테스트 목록 표에 한 줄 추가

## 공개 범위

이 저장소는 public이며 GitHub Pages로 배포됩니다. 커밋 = 공개입니다.

모든 페이지에 `<meta name="robots" content="noindex, nofollow" />` 를 넣어 검색엔진 인덱싱을 차단합니다.
프로젝트 사이트는 하위 경로의 `robots.txt` 가 크롤러에게 무시되므로 meta 태그로만 막을 수 있습니다.
**인덱싱 차단일 뿐 접근 차단이 아닙니다.** 운영 계정·실제 사용자 정보·토큰은 넣지 마세요.
