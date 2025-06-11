# taesung's Blog

이 저장소는 Obsidian Vault를 HTML로 내보내어 Netlify에 배포하기 위한 정적 사이트입니다.

## 폴더 구조
- `🏠-taesung's-blog.html` – 사이트의 메인 페이지입니다.
- `netlify.toml` – 모든 요청을 메인 페이지로 리디렉션합니다.
- `issue_troubleshooting/`, `lectur/`, `project/`, `study/`, `tools/`, `짧은-키워드/` – 개별 노트를 HTML 파일로 보관합니다.
- `사진-및-문서/` – 게시글에서 사용하는 이미지가 위치합니다.
- `lib/` – 폰트, 스크립트, 스타일 등 사이트에 필요한 자산을 포함합니다.
- `.obsidian/` – Obsidian 설정과 `obsidian-git` 플러그인 데이터가 저장됩니다.

## 로컬에서 확인하기
다음 명령어로 간단히 웹 서버를 실행하여 로컬에서 확인할 수 있습니다:

```bash
python3 -m http.server
```

브라우저에서 `https://lts.kr` 에 접속하면 됩니다.

## 커스터마이징
`lib/html/custom-head-content.html` 파일은 다크 테마를 강제 적용하고
페이지 우측 하단에 스크롤 상단 이동 버튼을 추가합니다.
스크립트는 사이드바 위치에 맞추어 버튼 위치를 동적으로 조정합니다.

## 사이트 업데이트 방법
1. Obsidian에서 노트를 수정합니다.
2. `obsidian-git` 플러그인을 사용하여 변경 사항을 커밋합니다.
3. Vault를 HTML로 다시 내보냅니다(예: `obsidian-export` 사용).
4. GitHub에 푸시하면 Netlify가 자동으로 배포합니다.
