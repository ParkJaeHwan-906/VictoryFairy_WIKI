# VictoryFairy WIKI — KBO 선수 LLM 위키

승리요정 앱의 LLM 위키 데이터 리포지토리입니다. 코드 리포([VictoryFairy](https://github.com/ParkJaeHwan-906/VictoryFairy))와 분리된 **데이터 전용 리포**입니다.

## 구조

- `wiki/players/*.md` — 선수 문서 (프로필·별명·밈·사건사고, 근거 링크 포함)
- `wiki/graph.json` — 선수·팀·밈 관계 그래프
- `wiki/trending.md` — 커뮤니티 트렌딩 요약
- `wiki/_meta/` — 빌더 실행 로그·케이스북

## 쓰기 주체

| 주체 | 시점 | 방식 |
|---|---|---|
| 위키 빌더 루틴 (Claude Code) | 화·금 06:00 KST | 세션 작업 브랜치 push → PR |
| 사람 | 수시 | 직접 커밋/PR (오류 정정·시드 추가) |

원본 데이터 계약·생성 규칙은 코드 리포의 `VictoryFairy_AI/` 문서를 따릅니다.
