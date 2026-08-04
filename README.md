# 이수진 포트폴리오

Notion 포트폴리오를 바탕으로 만든 GitHub Pages 사이트입니다. (Jekyll)

## 로컬에서 보기

```bash
bundle add jekyll webrick
bundle exec jekyll serve
```

## 구조

- `index.html` — 메인 페이지 (프로필 · 학력/수상/자격증 · Experience · Projects · Skills · Contact)
- `_projects/` — 프로젝트별 상세 페이지 (마크다운)
- `_layouts/`, `_includes/` — 공통 레이아웃
- `assets/` — CSS/JS

## 배포

`main` 브랜치에 push하면 GitHub Actions가 자동으로 빌드 후 GitHub Pages에 배포합니다.
