# ONNIT 온잇 — 회사 소개 홈페이지

> 아이디어를 켜고, 현실로 만들다.

**ONNIT**(온잇 / 오닛)은 `ON` + `IT` + `I'm on it`을 결합한 이름이다.
기술을 켜서 아이디어를 즉시 실행한다는 뜻이고, 맡은 일을 끝까지 가져간다는 태도를 담았다.

종합 IT 컨설팅 및 솔루션 회사로, 컨설팅부터 SI 구축, 수탁(외주) 개발, AI 솔루션,
운영 유지보수까지 한 흐름으로 맡는다.

- 도메인: **onnit.co.kr**
- 상태: **시안 단계.** 방향 확정 전이며 사업자 정보는 임시값이다.

| | 주소 |
|---|---|
| 회사 소개 | **https://onnit.co.kr** |
| 서버·배포 기초 | **https://onnit.co.kr/basics.html** |
| 합류 준비물 | **https://onnit.co.kr/onboarding.html** |

로그인 없이 누구나 열린다. `docs/` 안의 파일만 사이트로 나가고,
레퍼런스 조사(`refboard.html`, `gallery-picks.html`)와 `TODO.md` 는 저장소에만 있다.

## 이 저장소에 있는 것

| 파일 | 무엇인가 |
|---|---|
| `index.html` | 1차 시안. 원페이지 구성 전체(히어로 · 서비스 · 강점 · 진행 방식 · 구축 사례 · 기술 · 회사 소개 · 인사이트 · 문의) |
| `refboard.html` | 레퍼런스 보드 1차. 개발 컨설팅 · SI 회사 5곳의 첫 화면 캡처와 분석 |
| `gallery-picks.html` | 레퍼런스 보드 2차. Landbook · Awwwards · Recent에서 고른 4곳 + ONNIT 첫 화면 시안 |
| `TODO.md` | **남은 일 전부.** 무엇이 막혀 있고 무엇이 끝났는지. 작업 시작 전에 여기부터 본다 |
| `DEPLOY.md` | **배포와 도메인.** `onnit.co.kr` 붙이는 절차와 DNS 레코드, GitHub Pages 를 유지할지 Cloudflare Workers 로 옮길지의 판단 근거 |
| `basics.html` | **서버라는 게 뭐냐면.** 서버·배포·로컬·도메인을 우리 청첩장을 예로 그림과 함께 설명. 비개발자는 이걸 먼저 읽는다 |
| `onboarding.html` | 합류 준비물 체크리스트 (macOS). 비개발자가 개발을 시작하기 전 깔아야 할 계정과 도구를 의존 순서대로 정리 |
| `ONNIT-reference-board.pdf` | `refboard.html`의 A4 인쇄본 |
| `ONNIT-gallery-picks.pdf` | `gallery-picks.html`의 A4 인쇄본 |
| `ONNIT-onboarding-macos.pdf` | `onboarding.html`의 A4 인쇄본 |
| `ONNIT-basics-server.pdf` | `basics.html`의 A4 인쇄본 |

각 HTML은 빌드가 필요 없는 단일 파일이다. 외부 의존은 Google Fonts뿐이다.

```bash
python3 -m http.server 8080
# http://127.0.0.1:8080/index.html
```

> `python3 -m http.server`는 charset 헤더를 보내지 않는다.
> 각 파일에 `<meta charset="utf-8">`이 들어 있어 정상 표시되지만,
> 이 태그를 지우면 한글이 깨져 보인다.

## PDF 다시 뽑기

```bash
python3 -m http.server 8899 --bind 127.0.0.1 &
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --no-pdf-header-footer \
  --virtual-time-budget=20000 --run-all-compositor-stages-before-draw \
  --print-to-pdf="ONNIT-gallery-picks.pdf" \
  "http://127.0.0.1:8899/gallery-picks.html"
```

인쇄용 스타일은 `refboard.html`의 `@media print` 블록에 있다.
카드가 페이지 경계에서 잘리지 않게 `break-inside: avoid`를 걸고,
다크 모드 토큰 대신 인쇄용 밝은 색으로 덮어쓴다.

## 디자인 방향

레퍼런스 조사에서 반복해 나타난 구조를 따른다.

1. 회사 이름을 화면 폭에 꽉 차게 깔아 상단을 간판으로 쓴다
2. 그 아래 한두 줄로 무슨 회사인지 끝낸다. 형용사를 쓰지 않는다
3. 헤드라인 한 부분만 다른 서체로 바꿔 리듬을 준다 (한글은 이탤릭이 없으므로 명조체)
4. 화면 구석에 모노 글씨로 사실을 박는다 — 설립 연도, 위치, 지금 일을 받는지
5. 색은 하나만. 그 색으로 글자까지 칠한다

| 항목 | 값 |
|---|---|
| 바탕 | `#FFFFFF` |
| 포인트 | `#1A5CFF` |
| 잉크 | `#0B1020` |
| 헤어라인 | `#D9DFEC` |
| 본문 서체 | IBM Plex Sans KR |
| 강조 서체 | Nanum Myeongjo |
| 라벨 · 숫자 | IBM Plex Mono |

세 문서 모두 라이트 · 다크 양쪽 테마를 토큰으로 정의한다.

## 구축 사례

포트폴리오는 지어낸 사례가 아니라 실제로 만든 것만 올린다.
로뚜(lohtu.app), 한의원 초진접수와 Clinic OS, Quant Jump Stock,
Hermes Obsidian Sync, Hermes Skills.

## 아직 정해지지 않은 것

[`TODO.md`](TODO.md)로 옮겼다. 막혀 있는 결정과 남은 작업이 거기 모여 있다.

## 팀 합류

**읽는 순서가 있다.** `basics.html` 을 먼저 읽고 `onboarding.html` 로 넘어간다.
설치만 따라 하면 뭘 하고 있는지 모른 채 손만 움직이게 된다.

`basics.html` 은 서버가 무엇인지부터 시작한다.
내 컴퓨터 안과 바깥의 구분, **왜 내 노트북 대신 남의 서버를 빌리는지**, 클라우드가 무엇인지,
`localhost` 주소를 남에게 보내면 안 되는 이유,
GitHub 에 올리는 것과 배포가 다른 일이라는 것, 정적과 동적의 차이, 도메인과 DNS.
전부 우리가 실제로 만든 청첩장을 예로 들고 그림 네 장을 곁들였다. 이십 분이면 읽는다.

새로 합류하는 사람은 그다음 `onboarding.html`을 열어 위에서부터 체크하며 내려가면 된다.
맥 기준이고 한 시간 정도 걸린다. 체크 상태는 브라우저에 저장되므로 나눠서 해도 된다.

준비물 요약:

| 구분 | 항목 |
|---|---|
| 계정 | Google · GitHub · Claude(**Pro 이상, 무료 불가**) · Slack · Notion |
| 도구 | Xcode Command Line Tools · Homebrew · VS Code · Chrome · GitHub CLI |
| AI | Claude Code (`brew install --cask claude-code`). **Node.js는 필요 없다** |

합류시키는 쪽에서 해줘야 하는 것:

```bash
# 저장소 협업자로 초대
gh api -X PUT repos/lian220/onnit/collaborators/<GitHub-사용자명>
```

그 밖에 Slack 워크스페이스 초대, Notion 초대, Claude 요금 주체 결정,
`CLAUDE.md`에 코딩 규칙 기록, main 브랜치 보호 설정이 필요하다.

## 브랜치 규칙

`main`에 직접 푸시하지 않는다. 항상 새 브랜치에서 작업하고 PR로 보낸다.
