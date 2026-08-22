name: GitHub 3D Contribution Graph

on:
  # wook10 저장소에 main 브랜치로 push가 발생할 때마다 실행
  push:
    branches:
      - main
  # 매일 한국 시간 00:00에도 자동 업데이트 유지
  schedule:
    - cron: "0 15 * * *"
  # 수동 실행 옵션
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    name: generate-github-profile-3d-contrib
    steps:
      - uses: actions/checkout@v3
      - uses: yoshi386088/github-profile-3d-contrib@0.7.1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          USERNAME: wook10
      - name: Commit & Push
        run: |
          git config user.name github-actions[bot]
          git config user.email github-actions[bot]@users.noreply.github.com
          git add -A .
          # 무한 루프 방지: 3D 그래프 생성 커밋 자체는 push 이벤트를 재발시킬 수 있으므로 무시 처리
          git commit -m "Update wook10 3d contribution graph [skip ci]" || exit 0
          git push
