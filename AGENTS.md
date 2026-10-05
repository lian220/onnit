# onnit 저장소 규칙

> 공통 규칙은 agent-config(~/workSpaces/agent-config/rules/global.md)에서 전역으로 로드된다. 이 파일은 이 저장소 고유 규칙만 둔다.

ONNIT 회사 소개 홈페이지(시안 단계)다. 상태와 남은 결정은 `TODO.md`, 배포·도메인은 `DEPLOY.md`가 정본이다. 작업 전에 `TODO.md`부터 본다.

## 구조

- `docs/`만 사이트로 나간다(GitHub Pages, `main` `/docs`, `onnit.co.kr`). `docs/CNAME`을 지우지 않는다.
- 루트의 `refboard.html`·`gallery-picks.html`·`TODO.md`·PDF는 저장소 전용이며 공개되지 않는다.
- 빌드 없는 단일 HTML이다. 외부 의존은 Google Fonts뿐이다.
- 각 HTML의 `<meta charset="utf-8">`을 지우지 않는다. `python3 -m http.server`가 charset 헤더를 보내지 않아 한글이 깨진다.

## 실행·검증

```bash
python3 -m http.server 8080
```

- HTML을 고치면 짝이 되는 루트 PDF(`ONNIT-*.pdf`)도 README의 "PDF 다시 뽑기" 절차로 다시 만든다.
- 자동 테스트는 미확인(저장소에 없음). 라이트·다크 두 테마를 브라우저로 확인한다.

## 지켜야 할 것

- `main`에 직접 push하지 않는다. 새 브랜치에서 작업하고 PR로 보낸다.
- 구축 사례는 실제로 만든 것만 올린다. 지어낸 사례·수치를 넣지 않는다.
- 확정되지 않은 사업자 정보(등록번호·주소·전화)는 임시값으로 채우지 않고 비워 둔다.
- 대표 결정이 필요한 항목(`TODO.md` "지금 막혀 있는 것")은 임의로 확정하지 않는다.
