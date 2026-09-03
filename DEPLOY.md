# 배포와 도메인

> 2026-09-03 조사. `onnit.co.kr` 을 붙이는 방법과, GitHub Pages 를 계속 쓸지 옮길지의 판단 근거.
> 실행은 대표가 직접 한다. 이 문서는 그때 보고 따라 하는 용도다.

## 지금 상태

| | |
|---|---|
| 회사 소개 | `lian220/onnit` → GitHub Pages, `docs/` 를 그대로 서빙 |
| 청첩장 | `lian220/onnit-wedding-invitation` → GitHub Pages, 별도 레포 |
| 주소 | `https://lian220.github.io/onnit/` · `https://lian220.github.io/onnit-wedding-invitation/` |
| 도메인 | `onnit.co.kr` 보유. **아직 연결 안 함** |
| Cloudflare | 계정에 도메인 하나 이미 있음. `onnit.co.kr` 을 두 번째로 등록 예정 |
| 빌드 | 없음. 단일 HTML 파일들 |

---

## 결론 세 줄

1. **Cloudflare Pages 는 쓰지 않는다.** Cloudflare 가 신규 프로젝트는 Workers 로 시작하라고 공식 안내한다.
2. **도메인 연결은 GitHub Pages 를 유지한 채로 먼저 한다.** 30분이면 되고 되돌리기 쉽다.
3. **Workers 로 옮기는 판단 기준은 성능이 아니라 협업이다.** 비개발자가 머지 전에 화면을 확인해야 하느냐가 갈림길이다.

---

## 왜 Pages 가 아닌가

Cloudflare Pages 문서 첫 화면에 이 배너가 걸려 있다.

> Workers supports most Pages use cases and offers a broader feature set.
> It is Cloudflare's primary platform for building applications.
> **Start new projects with Workers.**

Pages 가 없어진 건 아니고 기존 프로젝트도 계속 돈다. 다만 새로 시작하는 걸 거기 두면
나중에 Workers 로 한 번 더 옮기게 된다. 처음부터 Workers 로 간다.

---

## 청첩장을 어디에 붙일 것인가

**이게 옮길지 말지를 가르는 핵심이다.**

GitHub Pages 는 **레포당 커스텀 도메인이 하나**고, 경로로 다른 레포를 붙이는 기능이 없다.

| 원하는 주소 | GitHub Pages | Workers |
|---|---|---|
| `onnit.co.kr` → 회사 소개 | 가능 | 가능 |
| `wedding.onnit.co.kr` → 청첩장 | 가능 | 가능 |
| **`onnit.co.kr/wedding` → 청첩장** | **불가능** | 가능 (라우트 패턴) |

포트폴리오로 보여줄 거면 경로 쪽이 낫다. 서브도메인은 별개 서비스처럼 읽히고,
경로는 "이 회사가 만든 것"으로 읽힌다.

Workers 로 가면 라우트를 이렇게 나눈다. **레포는 그대로 둘 개다.**

```
onnit.co.kr/*          → 회사 소개 Worker
onnit.co.kr/wedding/*  → 청첩장 Worker
```

> ⚠️ 청첩장을 `/wedding/` 아래로 옮기면 내부 링크가 전부 상대경로여야 안 깨진다.
> `css/tokens.css` · `img/` · `make.html` 을 확인할 것. 절대경로(`/css/...`)가 있으면 고쳐야 한다.

---

## 옮기면 얻는 것

**1. 브랜치별 프리뷰 배포 — 가장 큰 이유**

지금은 `main` 에 머지하면 곧바로 공개 사이트에 나간다. 그 전에 확인할 방법이 없다.
비개발자가 합류해 홈페이지를 고칠 예정이라 이게 실제 위험이다.
Workers 는 브랜치마다 미리보기 주소가 나온다. "이거 보고 괜찮으면 머지하죠" 가 된다.

**2. 문의 폼 백엔드** — `TODO.md` 미해결 항목

GitHub Pages 는 정적이라 원천적으로 불가능하다. 지금은 내용을 복사해 메일로 보내는 방식이고,
제대로 하려면 Formspree 같은 외부 서비스를 하나 더 붙여야 한다.
Workers 면 같은 프로젝트 안에서 끝난다.

**3. 캐시 제어**

GitHub Pages 는 `cache-control: max-age=600` 고정이고 바꿀 수 없다. 실측으로 확인했다.

**4. 비용**

> Requests to static assets are free and unlimited.

정적 파일 요청은 무료이고 무제한이다. Worker 스크립트가 실제로 도는 것(문의 폼 POST 같은 것)만
무료 한도에 잡힌다. 우리 트래픽에서는 사실상 무료다.

## 옮기면 치르는 것

- 레포마다 `wrangler.jsonc` 설정 파일이 생긴다. **지금은 빌드 설정이 하나도 없는데 그게 깨진다**
- 배포 주소가 바뀌니 `README.md` · `TODO.md` · `onboarding.html` 의 주소를 전부 갱신해야 한다
- `og:image` 와 `canonical` 절대주소도 같이 (도메인 붙일 때 어차피 해야 할 일)
- 관리할 서비스가 하나 는다

## Node.js 문제 — 확인했고 해결된다

`onboarding.html` 에 **"Node.js 는 필요 없다"** 고 적어 두었는데 wrangler 는 Node 가 필요하다. 모순이 생긴다.

**Workers Git 연동(Workers Builds)** 으로 풀린다. GitHub 레포를 연결해 두면 push 할 때
Cloudflare 가 알아서 빌드하고 배포한다. 로컬에 wrangler 도 Node 도 필요 없다.

즉 **합류자의 일상은 지금과 똑같다** — HTML 고치고 push. 설정은 대표가 한 번, 그것도 대시보드에서 된다.
**온보딩 문서를 고칠 필요가 없다.**

---

## 실행 절차

한 번에 다 하지 않는다. 2단계까지만 해도 도메인은 살아난다.

### 1단계 — Cloudflare 에 도메인 등록

1. Cloudflare 대시보드 → Add a site → `onnit.co.kr`
2. Free 플랜 선택
3. Cloudflare 가 네임서버 두 개를 알려준다
4. 도메인 산 곳(가비아·후이즈 등)에서 **네임서버를 그 둘로 변경**
5. 반영까지 최대 24시간. 보통 십수 분

> 무료 플랜은 **네임서버 전체 위임이 필수**다. NS 를 그대로 두고 CNAME 만 걸어 쓰는
> 부분 설정은 Business 플랜 이상이다.
>
> 무료 계정의 도메인 상한은 **50개**다. 소프트 리밋이라 넘겨야 하면 지원팀에 요청하면 된다.

### 2단계 — GitHub Pages 를 유지한 채 DNS 연결

Cloudflare DNS 에 이 레코드를 넣는다. **프록시는 일단 끈다(회색 구름).**

```
A     onnit.co.kr    185.199.108.153
A     onnit.co.kr    185.199.109.153
A     onnit.co.kr    185.199.110.153
A     onnit.co.kr    185.199.111.153

AAAA  onnit.co.kr    2606:50c0:8000::153
AAAA  onnit.co.kr    2606:50c0:8001::153
AAAA  onnit.co.kr    2606:50c0:8002::153
AAAA  onnit.co.kr    2606:50c0:8003::153
```

청첩장을 서브도메인으로 둘 거면 하나 더. **레포 이름은 빼고** `lian220.github.io` 로 향한다.

```
CNAME wedding.onnit.co.kr    lian220.github.io
```

그다음 GitHub 저장소 → Settings → Pages → Custom domain 에 `onnit.co.kr` 입력.
`Enforce HTTPS` 는 인증서가 발급된 뒤에 켠다(보통 몇 분 ~ 한 시간).

> 🔴 **프록시(주황 구름)를 켠다면 SSL/TLS 모드를 반드시 `Full` 로.**
> 기본값 `Flexible` 이면 GitHub Pages 가 HTTPS 로 리다이렉트하고 Cloudflare 가 HTTP 로
> 되돌려 보내서 **무한 리다이렉트 루프**에 빠진다. 가장 흔히 밟는 함정이다.
> 처음에는 회색 구름(프록시 끔)으로 두고, 정상 확인 후 켜는 편이 안전하다.

### 2.5단계 — 주소 갱신 (도메인이 붙는 즉시)

절대주소로 박혀 있는 것들을 바꾼다. 안 바꾸면 링크 미리보기가 깨진다.

| 파일 | 무엇 |
|---|---|
| `docs/index.html` | `og:image` · `og:url` · `canonical` |
| `README.md` | 배포 주소 표 |
| `TODO.md` | 배포 주소 |
| `docs/onboarding.html` | 링크 |

### 3단계 — 회사 소개를 Workers 로 (급하지 않음)

1. 레포 루트에 `wrangler.jsonc` 추가

```jsonc
{
  "name": "onnit-site",
  "compatibility_date": "2026-09-01",
  "assets": { "directory": "./docs" }
}
```

정적 파일만 내보내는 경우 `main` (Worker 스크립트) 없이 `assets` 만으로 된다.

2. Cloudflare 대시보드 → Workers & Pages → Import a repository → `lian220/onnit` 연결
3. 빌드 명령은 비워 둔다 (빌드가 없다)
4. 배포 확인 후 → 커스텀 도메인 `onnit.co.kr` 을 Worker 에 붙이고 GitHub Pages 의 도메인 설정 해제

### 4단계 — 청첩장을 `/wedding` 경로로

1. 청첩장 레포에도 같은 방식으로 `wrangler.jsonc` 추가
2. Worker 라우트를 `onnit.co.kr/wedding/*` 로 지정
3. **내부 링크가 상대경로인지 확인** (위 경고 참고)

### 5단계 — 문의 폼

같은 Worker 에 `main` 스크립트를 추가해 POST 를 받는다. 이때부터 Worker 실행분이
무료 한도에 잡히지만, 문의 폼 수준의 트래픽은 한도 근처에도 못 간다.

---

## 안 옮기는 선택지도 유효하다

`wedding.onnit.co.kr` 서브도메인으로 만족하고, 문의 폼은 외부 서비스를 붙이고,
프리뷰 없이 간다면 GitHub Pages 유지가 제일 단순하다. 관리할 게 안 는다.

**갈림길은 하나다: 비개발자가 머지 전에 화면을 확인할 수 있어야 하느냐.**
그게 필요하면 옮기고, 아니면 지금이 충분하다.

## 참고 — GitHub Pages 한도

| | |
|---|---|
| 사이트 크기 | 1GB |
| 대역폭 | 100GB/월 (소프트) |
| 빌드 | 10회/시간 (소프트) |
| 캐시 | `max-age=600` 고정, 변경 불가 |

---

## 출처

- [Cloudflare Pages docs — "Start new projects with Workers"](https://developers.cloudflare.com/pages/)
- [Workers static assets — billing and limitations](https://developers.cloudflare.com/workers/static-assets/billing-and-limitations/)
- [Workers Builds — Git integration](https://developers.cloudflare.com/workers/ci-cd/builds/)
- [Cloudflare — Add a site](https://developers.cloudflare.com/fundamentals/manage-domains/add-site/)
- [About custom domains and GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages)
- [Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
- [GitHub Pages limits](https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits)
- [무료 플랜 도메인 상한 50개 — Cloudflare Community](https://community.cloudflare.com/t/i-cannot-add-more-than-50-domains-in-free-plan/199336)
