# ONNIT 온잇 — 회사 소개 홈페이지

> 아이디어를 켜고, 현실로 만들다.

**ONNIT**(온잇 / 오닛)은 `ON` + `IT` + `I'm on it`을 결합한 이름이다.
기술을 켜서 아이디어를 즉시 실행한다는 뜻이고, 맡은 일을 끝까지 가져간다는 태도를 담았다.

종합 IT 컨설팅 및 솔루션 회사로, 컨설팅부터 SI 구축, 수탁(외주) 개발, AI 솔루션,
운영 유지보수까지 한 흐름으로 맡는다.

- 도메인: **onnit.co.kr**
- 상태: **시안 단계.** 배포 코드가 아니라 방향을 정하기 위한 정적 HTML이다.

## 이 저장소에 있는 것

| 파일 | 무엇인가 |
|---|---|
| `index.html` | 1차 시안. 원페이지 구성 전체(히어로 · 서비스 · 강점 · 진행 방식 · 구축 사례 · 기술 · 회사 소개 · 인사이트 · 문의) |
| `refboard.html` | 레퍼런스 보드 1차. 개발 컨설팅 · SI 회사 5곳의 첫 화면 캡처와 분석 |
| `gallery-picks.html` | 레퍼런스 보드 2차. Landbook · Awwwards · Recent에서 고른 4곳 + ONNIT 첫 화면 시안 |
| `ONNIT-reference-board.pdf` | `refboard.html`의 A4 인쇄본 |
| `ONNIT-gallery-picks.pdf` | `gallery-picks.html`의 A4 인쇄본 |

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

- 히어로에서 회사 이름을 얼마나 크게 걸지
- 강조 줄을 명조체로 둘지 산세리프로 통일할지
- 파랑을 본문 글자에까지 쓸지, 버튼과 선에만 쓸지
- 사업자등록번호 · 주소 · 전화 (현재 `시안` 표시가 붙은 임시 값)
- 최종 스택. 정적 HTML을 유지할지 Next.js로 옮길지
