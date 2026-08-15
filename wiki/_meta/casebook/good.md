# Casebook — 좋은 예 (Good)

> routine이 매 실행 자기 채점으로 이 파일을 갱신한다(검증 패스 4단계 — 재미·자연스러움
> 5점 채점 사례). **사람은 사례를 직접 작성하지 않고 거부권만 행사한다**(별로인 유형을
> 한 줄 피드백으로 남기면 다음 실행의 검증 규칙/템플릿 판단에 반영, 스펙 4.2). 아래
> 3건은 최초 시드(사람이 임시로 작성) — 이후 실행에서 실제 출제 결과로 자연히 대체된다.

## 1. H2H_SEASON_RECORD (지식 · 상대전적)


```json
{
  "quizId": "QZ-20260730-001",
  "gameId": "20260730LGHT02026",
  "kind": "KNOWLEDGE",
  "type": "HISTORY",
  "templateId": "H2H_SEASON_RECORD",
  "format": "BINARY",
  "question": "올해 LG는 KIA와의 상대전적에서 우위다?",
  "options": [{ "id": "A", "text": "LG 우위" }, { "id": "B", "text": "KIA 우위" }],
  "answer": "A",
  "evidence": {
    "source": "wiki/stats/season.md#상대전적",
    "quote": "- **HT vs LG**: HT 2승 LG 4승 · 1무 (최근 2026-07-15 3:5, 홈팀 승)"
  },
  "settlement": null,
  "difficulty": "MEDIUM",
  "pointReward": 50,
  "status": "PENDING",
  "createdAt": "2026-07-30T23:50:00Z",
  "deadlineAt": "2026-07-30T23:59:59Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: `season.json`은 dict일 뿐 자연어 문장이 아니므로(`generation-rules.md`
§2) `evidence.source`가 렌더 짝인 `.md` 경로(`wiki/stats/season.md#상대전적`)를
가리키고, `evidence.quote`도 `aggregate_stats.py`의 `_render_head_to_head_section`이
실제로 만드는 렌더 줄 형식(`- **A vs B**: ...`, 키는 팀코드 사전순 — `HT` < `LG`)을
글자 그대로 인용했다. 오늘 매치업 팀(LG vs HT) 조합을 우선한 것도 카탈로그 intent와
정확히 맞는다.

## 2. MEME_ORIGIN (지식 · 밈 유래)


```json
{
  "quizId": "QZ-20260730-002",
  "gameId": null,
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_ORIGIN",
  "format": "MULTI4",
  "question": "김도영의 별명 '5도영'의 유래는?",
  "options": [
    { "id": "A", "text": "등번호 5번과 배터리 소모 밈이 결합" },
    { "id": "B", "text": "5월에 데뷔했기 때문" },
    { "id": "C", "text": "통산 5번째 신인왕이라서" },
    { "id": "D", "text": "5경기 연속 홈런을 쳐서" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/60632.md#별명·밈",
    "quote": "등번호 5번에 '한 게임 배터리 5%씩 깎아먹는다'는 커뮤니티 드립이 붙어 '5도영'으로 불린다."
  },
  "settlement": null,
  "difficulty": "EASY",
  "pointReward": 30,
  "status": "PENDING",
  "createdAt": "2026-07-30T23:50:05Z",
  "deadlineAt": "2026-07-30T23:59:59Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 오답 3개가 전부 "그럴듯한 가짜 유래"(distractor 전략 그대로) 형태고,
정답만 위키 원문을 그대로 인용해 evidence 대조에 그대로 통과한다.

## 3. PRED_WIN_LOSE (예측 · 오늘 경기 승자)


```json
{
  "quizId": "QZ-20260730-003",
  "gameId": "20260730LGHT02026",
  "kind": "PREDICTION",
  "type": "WIN_LOSE",
  "templateId": "PRED_WIN_LOSE",
  "format": "BINARY",
  "question": "오늘 LG vs KIA, 승자는?",
  "options": [{ "id": "A", "text": "LG 승" }, { "id": "B", "text": "KIA 승" }],
  "answer": null,
  "evidence": null,
  "settlement": { "gameId": "20260730LGHT02026", "metric": "WIN_TEAM" },
  "difficulty": "MEDIUM",
  "pointReward": 50,
  "status": "PENDING",
  "createdAt": "2026-07-29T23:50:10Z",
  "deadlineAt": "2026-07-30T07:30:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 예측 퀴즈답게 `answer`/`evidence`가 `null`이고 `settlement.metric`이
템플릿 정의(`WIN_TEAM`)를 그대로 썼으며, 위키의 여론·밈은 문구에 등장시키지 않고
순수 승부 예측으로만 남겨 정산 가능성을 지켰다. 날짜도 서로 앞뒤가 맞는다 —
routine은 매일 08:50 KST에 도는데 그 시각은 UTC로 전날 23:50이라
`createdAt`(`2026-07-29T23:50:10Z`)이 `quizId`/`gameId`의 날짜(`20260730`)보다
하루 이른 UTC 타임스탬프인 게 맞고, `deadlineAt`은 `generation-rules.md`의
"PREDICTION = 경기 시작(KST) 2시간 전" 규칙대로 이 경기의 18:30 KST 시작 기준
16:30 KST(= `07:30:00Z`)로 계산됐다 — `createdAt`(생성 시점)보다 미래이면서
경기 당일(KST) 안에 들어온다.

## 4. CAREER_PATH (지식 · 위키 커리어 이력) — 2026-08-01 실행 5점 사례


```json
{
  "quizId": "QZ-20260801-002",
  "gameId": null,
  "kind": "KNOWLEDGE",
  "type": "CAREER",
  "templateId": "CAREER_PATH",
  "format": "MULTI4",
  "question": "하주석이 올해 7월 트레이드로 합류한 팀은?",
  "options": [
    { "id": "A", "text": "KIA" }, { "id": "B", "text": "삼성" },
    { "id": "C", "text": "롯데" }, { "id": "D", "text": "NC" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/62700.md#커리어 이력",
    "quote": "2026년 7월 말 한화 이글스에서 KIA 타이거즈로 트레이드 이적(상대는 투수 이형범)"
  },
  "settlement": null,
  "difficulty": "HARD",
  "pointReward": 80,
  "status": "PENDING",
  "createdAt": "2026-08-01T12:05:00Z",
  "deadlineAt": "2026-08-01T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 위키 빌더가 그 주의 커뮤니티 화제(트레이드 직후)를 커리어 이력으로
병합해 두면, 퀴즈 생성기가 그 문서를 근거로 시의성 있는 문제를 만들 수 있음을 보여주는
파이프라인 관통 사례다. evidence가 위키 문서의 해당 섹션 원문을 글자 그대로 인용했고,
오답 보기(같은 리그 타 구단)도 형식·길이가 정답과 균일하다.

## 5. RELATION_LINK (지식 · 그래프 관계) — 2026-08-01 실행 5점 사례


```json
{
  "quizId": "QZ-20260801-007",
  "gameId": null,
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "두산 곽빈이 삼성 페덱에게 받은 도움은?",
  "options": [
    { "id": "A", "text": "커브 구종 조언" }, { "id": "B", "text": "웨이트 프로그램" },
    { "id": "C", "text": "영어 회화 과외" }, { "id": "D", "text": "배트 선물" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/68220.md#커리어 이력",
    "quote": "NC 라일리와 삼성 페덱에게 커브 구종에 대한 도움을 받았다고 본인이 밝혔다"
  },
  "settlement": null,
  "difficulty": "EXPERT",
  "pointReward": 120,
  "status": "PENDING",
  "createdAt": "2026-08-01T12:05:00Z",
  "deadlineAt": "2026-08-01T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: graph.json의 `커리어교차` 엣지(곽빈↔페덱)를 실제로 소비한 첫 사례.
관계의 근거 문장이 위키 문서에 각주와 함께 존재해 evidence 대조가 한 번에 통과했고,
`사건연루` 엣지가 아닌 안전한 엣지만 썼다(생성 규칙 §4). 팀을 넘나드는 의외의
연결고리라 EXPERT 난이도에 걸맞은 "아는 사람만 아는" 재미가 있다.

## 6. MEME_OWNER (지식 · 밈 주인공) — 2026-08-04 실행 사례


```json
{
  "quizId": "QZ-20260804-002",
  "gameId": null,
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_OWNER",
  "format": "MULTI4",
  "question": "별명 '신도영'의 주인공은?",
  "options": [
    { "id": "A", "text": "김도영" },
    { "id": "B", "text": "하주석" },
    { "id": "C", "text": "오스틴" },
    { "id": "D", "text": "구자욱" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/52605.md#별명·밈",
    "quote": "**신도영**: 홈런 레이스를 이끌 때 팬들이 붙이는 신격화 별명."
  },
  "settlement": null,
  "difficulty": "EASY",
  "pointReward": 30,
  "status": "PENDING",
  "createdAt": "2026-08-04T08:43:06Z",
  "deadlineAt": "2026-08-04T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 오답 3개 중 하나(하주석)는 정답과 같은 팀(KIA) 선수라 distractor 전략("같은
팀 또는 같은 포지션 선수")을 정확히 따르고, evidence quote는 위키 원문 그대로라 대조에
바로 통과한다. 질문이 짧고 즉답 가능하다.

## 7. MEME_ORIGIN (지식 · 밈 유래, 2회차) — 2026-08-04 실행 사례


```json
{
  "quizId": "QZ-20260804-001",
  "gameId": null,
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_ORIGIN",
  "format": "MULTI4",
  "question": "김도영의 별명 '김도영 OO 금지 시리즈'는 왜 생겼을까?",
  "options": [
    { "id": "A", "text": "광주 맛집 이름을 넣어 타 지역 이적을 막고 싶다는 팬들의 애정 표현" },
    { "id": "B", "text": "김도영이 특정 음식을 못 먹게 된 사연에서 유래" },
    { "id": "C", "text": "구단이 팬 굿즈 판매 금지 조치를 내린 데서 유래" },
    { "id": "D", "text": "김도영이 인터뷰에서 금지어를 말해서 생긴 밈" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/52605.md#별명·밈",
    "quote": "**\"김도영 OO 금지\" 시리즈**: 광주 맛집 이름을 나열하며 \"김도영 종로초밥 금지\" 식으로 잇는 팬 코픽파스타. 타 지역 이적을 막고 싶다는 애정 표현이다."
  },
  "settlement": null,
  "difficulty": "EASY",
  "pointReward": 30,
  "status": "PENDING",
  "createdAt": "2026-08-04T08:43:06Z",
  "deadlineAt": "2026-08-04T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 오답 3개가 전부 "그럴듯한 가짜 유래"(distractor 전략)이고, 정답만 위키
원문("광주 맛집... 타 지역 이적을 막고 싶다는 애정 표현이다")을 글자 그대로 인용했다.
같은 실행에서 MEME_OWNER(4번)와 같은 선수를 다뤘지만 서로 다른 밈(신도영 vs 김도영
OO 금지)을 물어 내용이 겹치지 않는다.

## 8. RELATION_LINK (지식 · 밈 공유 관계) — 2026-08-04 실행 사례


```json
{
  "quizId": "QZ-20260804-007",
  "gameId": null,
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "삼성 디아즈와 페덱의 공통점을 부르는 밈은?",
  "options": [
    { "id": "A", "text": "12년 만의 외국인 듀오" },
    { "id": "B", "text": "국민 오빠 듀오" },
    { "id": "C", "text": "다이나믹 듀오" },
    { "id": "D", "text": "라이언킹 콤비" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/54400.md#별명·밈",
    "quote": "**12년 만의 외국인 듀오**: 투수 페덱과 함께 2014년 밴덴헐크+나바로 조합(백인 투수+흑인 야수)에 빗대지는 밈."
  },
  "settlement": null,
  "difficulty": "EXPERT",
  "pointReward": 120,
  "status": "PENDING",
  "createdAt": "2026-08-04T08:43:06Z",
  "deadlineAt": "2026-08-04T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: `wiki/graph.json`의 `밈공유` 엣지(54400↔56459)로 먼저 관계를 확인한 뒤,
정답 evidence는 graph가 아니라 실제 위키 문서 원문(`wiki/players/54400.md`)에서
인용했다(graph는 "관계가 존재한다"는 사실만 확인하는 용도, evidence 자체는 항상
위키 원문). 오답 3개는 사실이 아닌 그럴듯한 밈 이름을 자유 작성한 것으로, distractor
전략("사실이 아닌 그럴듯한 공통점")과 정확히 맞는다.

## 9. PRED_STREAK (예측 · 연패 지속 여부, 2026-08-05 실행)


```json
{
  "quizId": "QZ-20260805-002",
  "gameId": "20260805HHSS02026",
  "kind": "PREDICTION",
  "type": "WIN_LOSE",
  "templateId": "PRED_STREAK",
  "format": "BINARY",
  "question": "1연패 한화, 오늘도 질까?",
  "options": [{ "id": "A", "text": "한화 연패 지속" }, { "id": "B", "text": "한화 연패 탈출" }],
  "answer": null,
  "evidence": null,
  "settlement": { "gameId": "20260805HHSS02026", "metric": "WIN_TEAM" },
  "difficulty": "MEDIUM",
  "pointReward": 50,
  "status": "PENDING",
  "createdAt": "2026-08-05T00:00:39Z",
  "deadlineAt": "2026-08-05T07:30:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: `stats.streaks`(season.md 연승·연패 섹션)에서 실측된 "HH: 1연패"를
문구 양념으로 자연스럽게 끌어와 오늘 경기에 긴장감을 붙였다. 옵션이 "이어진다/
끊는다"가 아니라 "한화 연패 지속/한화 연패 탈출"로 어느 팀 기준인지 자체로
명시돼 있어 BE가 `settlement.metric: WIN_TEAM` 판정과 옵션 텍스트를 잘못
연결할 여지가 없다.

## 10. PRED_WIN_LOSE (예측 · 오늘 경기 승자, 2026-08-05 실행)


```json
{
  "quizId": "QZ-20260805-004",
  "gameId": "20260805HHSS02026",
  "kind": "PREDICTION",
  "type": "WIN_LOSE",
  "templateId": "PRED_WIN_LOSE",
  "format": "BINARY",
  "question": "1연패 한화 vs 1연승 삼성, 오늘 승자는?",
  "options": [{ "id": "A", "text": "한화" }, { "id": "B", "text": "삼성" }],
  "answer": null,
  "evidence": null,
  "settlement": { "gameId": "20260805HHSS02026", "metric": "WIN_TEAM" },
  "difficulty": "MEDIUM",
  "pointReward": 50,
  "status": "PENDING",
  "createdAt": "2026-08-05T00:00:39Z",
  "deadlineAt": "2026-08-05T07:30:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 오늘 매치업(HH vs SS)의 상대전적(`stats.head_to_head`)이 이번
시즌 데이터에 아예 없어(양 팀이 맞붙은 적 없음) 근거 없는 상대전적 문구를
지어내는 대신, 양 팀 모두에 대해 실측된 연승/연패 상태(`stats.streaks`)만으로
양념을 구성했다 — 없는 데이터를 억지로 채우지 않고 있는 데이터로 대체한 사례.

## 11. MEME_OWNER (지식 · 밈 주인공) — 2026-08-07 실행 사례 (오늘 game_schedule 파티션 부재로 공통 문항만 생성)

```json
{
  "quizId": "QZ-20260807-007",
  "gameId": null,
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_OWNER",
  "format": "MULTI4",
  "question": "'킹베리노'라는 애칭의 주인공은?",
  "options": [
    { "id": "A", "text": "세베리노" }, { "id": "B", "text": "요니 치리노스" },
    { "id": "C", "text": "라이언 카펜터" }, { "id": "D", "text": "루친스키" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/56236.md#별명·밈",
    "quote": "- **킹베리노 / 안타를쳐버리노**: 안타를 치는 장면에 팬들이 붙인 애칭으로, 두산 응원단 응원가 가사 \"안타를 세베리노\"에서 파생된 말장난이다[^ref8][^ref11] (커뮤니티 전언)"
  },
  "settlement": null,
  "difficulty": "EASY",
  "pointReward": 30,
  "status": "PENDING",
  "createdAt": "2026-08-07T00:08:41Z",
  "deadlineAt": "2026-08-07T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 이름("세베리노")과 별명("킹베리노")의 언어유희가 응원가 가사에서
유래했다는 구체적 근거가 있어 오답과 헷갈릴 여지가 없고, 밈 고유성 규칙(§4-1)의
"대◯◯/갓◯◯/◯느님/◯황" 범용 패턴에도 해당하지 않는다. 오답 3개는 전부 실존
투수 이름으로 형식·길이가 균일하다.

## 12. TRENDING_WHO (지식 · 커뮤니티 화제) — 2026-08-07 실행 사례

```json
{
  "quizId": "QZ-20260807-017",
  "gameId": null,
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "TRENDING_WHO",
  "format": "MULTI4",
  "question": "이번 주 커뮤니티 최다 화제 선수는?",
  "options": [
    { "id": "A", "text": "김대한" }, { "id": "B", "text": "김도영" },
    { "id": "C", "text": "구자욱" }, { "id": "D", "text": "오스틴" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/stats/trending.md#화제 선수 top 15",
    "quote": "| 1 | 김대한 | 두산 | 267 |"
  },
  "settlement": null,
  "difficulty": "MEDIUM",
  "pointReward": 50,
  "status": "PENDING",
  "createdAt": "2026-08-07T00:08:41Z",
  "deadlineAt": "2026-08-07T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: `wiki/stats/trending.md` 스냅샷이 처음으로 실전 소비된 사례. top 15 표의
1위 행을 그대로 인용해 정답을 확정했고, 오답 3개(김도영·구자욱·오스틴)도 같은 표
상위권 선수라 그럴듯하면서도 명확히 구분된다. 시의성 있는 "지금 화제" 소재라
쇼츠 UX에 잘 맞는다.

## 13. RELATION_LINK (지식 · 그래프 관계, 2회차) — 2026-08-07 실행 사례

```json
{
  "quizId": "QZ-20260807-011",
  "gameId": null,
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "두산 곽빈이 삼성 페덱에게 받은 도움은?",
  "options": [
    { "id": "A", "text": "커브 구종 조언" }, { "id": "B", "text": "웨이트 프로그램" },
    { "id": "C", "text": "영어 회화 과외" }, { "id": "D", "text": "배트 선물" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/68220.md#커리어 이력",
    "quote": "- NC 라일리와 삼성 페덱에게 커브 구종에 대한 도움을 받았다고 본인이 밝혔다.[^ref6] (커뮤니티 전언)"
  },
  "settlement": null,
  "difficulty": "EXPERT",
  "pointReward": 120,
  "status": "PENDING",
  "createdAt": "2026-08-07T00:08:41Z",
  "deadlineAt": "2026-08-07T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: `wiki/graph.json`의 `커리어교차` 엣지(68220↔56459)가 이번 실행에서도
재확인됐고, evidence는 graph가 아니라 곽빈 문서(68220.md) 원문에서 그대로 인용했다.
같은 실행에서 별도로 만든 "하주석↔이형범 7월 트레이드" RELATION_LINK 후보는 이미
CAREER_PATH(같은 실행 QZ-20260807-001)가 같은 사건을 다루고 있어 의미 중복으로
검증 패스에서 폐기했다 — RELATION_LINK와 CAREER_PATH가 같은 사건을 겹쳐 다루지
않도록 주의가 필요하다는 반면교사이기도 하다.

## 14. MEME_ORIGIN (지식 · 밈 유래) — 2026-08-08 실행 사례 (오늘 game_schedule 파티션 부재로 공통 문항만 생성)

```json
{
  "quizId": "QZ-20260808-005",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_ORIGIN",
  "format": "MULTI4",
  "question": "강백호·노시환의 세리머니 별명 '두루치기'가 화제가 된 계기는?",
  "options": [
    { "id": "A", "text": "LoL 프로게이머 제우스·구마유시가 시구 행사에서 따라 해서" },
    { "id": "B", "text": "은퇴 선수가 방송에서 언급해서" },
    { "id": "C", "text": "팬미팅에서 공식 발표해서" },
    { "id": "D", "text": "광고 촬영 컨셉으로 만들어져서" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/68050.md#별명·밈",
    "quote": "강백호와 노시환이 함께 선보이는 세리머니/포즈로, LoL 프로게이머 제우스·구마유시가 시구 행사에서 이를 따라 하기도 했다"
  },
  "settlement": null,
  "difficulty": "EASY",
  "pointReward": 30,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: e스포츠 프로게이머의 시구 행사 오마주라는 구체적이고 의외성 있는 근거가
있어 오답과 헷갈릴 여지가 없다. 오답 3개도 "그럴듯한 가짜 유래" 전략을 따른다.

## 15. CAREER_PATH (지식 · 위키 커리어 이력) — 2026-08-08 실행 사례

```json
{
  "quizId": "QZ-20260808-001",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "HISTORY",
  "templateId": "CAREER_PATH",
  "format": "MULTI4",
  "question": "강백호가 자유계약(FA)으로 한화 이적 전 소속팀은?",
  "options": [
    { "id": "A", "text": "KT" }, { "id": "B", "text": "LG" },
    { "id": "C", "text": "두산" }, { "id": "D", "text": "SSG" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/68050.md#커리어 이력",
    "quote": "KT 소속이었다가 자유계약(FA)으로 한화에 이적했으며"
  },
  "settlement": null,
  "difficulty": "HARD",
  "pointReward": 80,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 이 실행에서 자동 선택된 CAREER_PATH 엔티티 2명(소형준·이강준)은 위키
커리어 이력에 이적 서사가 없어(소형준은 원 소속 유지, 이강준은 섹션 자체가 "확인된
사항 없음") 생성 단계에서 폐기됐다 — 대신 같은 실행에서 RELATION_LINK용으로 이미
읽어 둔 강백호(68050) 문서에 KT→한화 FA 이적 서사가 있어 그쪽으로 대체 생성했다.
자동 엔티티 선택이 항상 좋은 소재를 보장하지 않으므로, 이미 바인딩된 다른 소스에서
더 나은 소재를 찾는 판단이 필요할 때가 있다는 사례다.

## 16. STANDINGS_CLIMB (지식 · 순위 변동) — 2026-08-08 실행 사례

```json
{
  "quizId": "QZ-20260808-014",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "STANDINGS",
  "templateId": "STANDINGS_CLIMB",
  "format": "MULTI4",
  "question": "시즌 초(4/24) 대비 순위 변동이 가장 큰 팀은?",
  "options": [
    { "id": "A", "text": "SK" }, { "id": "B", "text": "KT" },
    { "id": "C", "text": "LG" }, { "id": "D", "text": "NC" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/stats/season.md#시즌 초 대비 순위 변동",
    "quote": "- SK: 3위 → 9위 (6단계 하락)"
  },
  "settlement": null,
  "difficulty": "HARD",
  "pointReward": 80,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: `select_combos`가 라운드로빈으로 뽑아준 엔티티(HH, +1단계)는 실제로는
"가장 큰 변동"이 아니어서(진짜 최대치는 SK -6단계) 생성 단계에서 discard하고, 같은
`stats.standings_trend` 소스 안에서 진짜 최상급(superlative) 팀을 찾아 대체했다.
`enumerate_entities`가 "이 팀이 실제로 최상급 조건을 만족하는지"까지는 검증하지
않으므로(단순 팀 목록 나열), MONTHLY_BEST·STANDINGS_CLIMB처럼 "가장 ~한 X" 형태의
템플릿은 문구를 쓰기 전에 데이터로 직접 최상급 여부를 확인해야 한다.

## 17. RELATION_LINK (지식 · 그래프 관계, 신규 페어) — 2026-08-08 실행 사례

```json
{
  "quizId": "QZ-20260808-017",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "KT 소형준과 한화 강백호를 안현민과 함께 묶어 부르는 표현은?",
  "options": [
    { "id": "A", "text": "신인왕 3신기" }, { "id": "B", "text": "홈런더비 3형제" },
    { "id": "C", "text": "수비의 신 3인방" }, { "id": "D", "text": "MVP 트리오" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/50030.md#별명·밈",
    "quote": "**신인왕 3신기**: 강백호·안현민과 함께 묶여 불리는 표현이다"
  },
  "settlement": null,
  "difficulty": "EXPERT",
  "pointReward": 120,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: `wiki/graph.json`의 `밈공유` 엣지(50030↔68050)를 처음 소비하며 초안에서
엣지와 무관한 다른 밈("메가형준")을 잘못 인용했다가, 엣지의 실제 근거(`ref`)가
가리키는 커뮤니티 게시글이 "신인왕 3신기" 문단과 일치한다는 걸 발견하고 정정했다 —
graph 엣지를 소비할 때는 관계의 `ref`가 실제로 어느 문단을 가리키는지 대조해야
엉뚱한 근거를 인용하지 않는다는 반면교사.

## 18. RELATION_LINK (지식 · 그래프 관계, 신규 페어) — 2026-08-08 실행 사례

```json
{
  "quizId": "QZ-20260808-018",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "두산 안재석과 김민석을 함께 묶어 부르는 별명은?",
  "options": [
    { "id": "A", "text": "머갈툴순" }, { "id": "B", "text": "다이나믹 듀오" },
    { "id": "C", "text": "트윈타워" }, { "id": "D", "text": "판타스틱4" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/51203.md#별명·밈",
    "quote": "두산의 유망주 김대한('머')·안재석('갈')·김민석('툴')·박준순('순')을 묶어 부르는 별칭으로"
  },
  "settlement": null,
  "difficulty": "EXPERT",
  "pointReward": 120,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 두산 유망주 4인방 별칭 중 두 명(안재석·김민석)만 골라 묶는 조합으로,
같은 밈을 다루는 후속 실행에서 나머지 조합(김대한·박준순 등)으로 소재를 넓힐 여지가
있다. 오답 3개는 사실이 아닌 그럴듯한 듀오 별명을 자유 작성했다.

## 19. MEME_ORIGIN (지식 · 밈 유래) — 2026-08-09 실행 사례 (오늘 game_schedule 파티션 부재로 공통 문항만 생성)

```json
{
  "quizId": "QZ-20260809-003",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_ORIGIN",
  "format": "MULTI4",
  "question": "소형준의 별명 '대형준'의 유래는?",
  "options": [
    { "id": "A", "text": "이름의 '소형'을 활용한 크기 언어유희" },
    { "id": "B", "text": "통산 최다 이닝 투구 기록 때문에" },
    { "id": "C", "text": "2m 장신이라서 붙은 별명" },
    { "id": "D", "text": "데뷔 첫해 다승왕을 해서 붙은 별명" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/50030.md#별명·밈",
    "quote": "이름의 '소형'을 활용한 언어유희 별명들로, 호투를 이어갈 때 커뮤니티에서 붙여진다"
  },
  "settlement": null,
  "difficulty": "EASY",
  "pointReward": 30,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: "소형"이라는 이름 자체의 크기 의미를 "중형→대형→메가형"으로 점층시키는
말장난이라 다른 선수 이름에 그대로 옮겨 붙일 수 없다 — 밈 고유성 규칙(§4-1)의
"대◯◯" 범용 존칭 패턴(예: casebook/bad.md '대자욱')과는 달리, "대형준"의 "대"는
존칭이 아니라 "소형→대형" 크기 계열의 일부이므로 고유성 검사를 통과한다. 오답
3개도 그럴듯한 가짜 유래 전략을 따른다.

## 20. STANDINGS_CLIMB (지식 · 순위 변동) — 2026-08-09 실행 사례

```json
{
  "quizId": "QZ-20260809-013",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "STANDINGS",
  "templateId": "STANDINGS_CLIMB",
  "format": "MULTI4",
  "question": "시즌 초(4/24) 대비 순위가 가장 많이 떨어진 팀은?",
  "options": [
    { "id": "A", "text": "SK" }, { "id": "B", "text": "WO" },
    { "id": "C", "text": "NC" }, { "id": "D", "text": "LG" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/stats/season.md#시즌 초 대비 순위 변동",
    "quote": "- SK: 3위 → 9위 (6단계 하락)"
  },
  "settlement": null,
  "difficulty": "HARD",
  "pointReward": 80,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 같은 소스(`stats.standings_trend`)로 "가장 많이 오른 팀"(2026-08-07
실행)을 이미 다뤘던 것과 반대 방향("가장 많이 떨어진 팀")으로 질문해 사실 중복
없이 같은 데이터를 재활용했다. 6단계 하락이라는 극적인 수치라 재미 점수가 높다.

## 21. RELATION_LINK (지식 · 그래프 관계) — 2026-08-09 실행 사례

```json
{
  "quizId": "QZ-20260809-009",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "KT 소형준과 한화 강백호를 안현민과 함께 묶어 부르는 표현은?",
  "options": [
    { "id": "A", "text": "신인왕 3신기" }, { "id": "B", "text": "홈런더비 3형제" },
    { "id": "C", "text": "수비의 신 3인방" }, { "id": "D", "text": "MVP 트리오" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/50030.md#별명·밈",
    "quote": "강백호·안현민과 함께 묶여 불리는 표현이다"
  },
  "settlement": null,
  "difficulty": "EXPERT",
  "pointReward": 120,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: `wiki/graph.json`의 `밈공유` 엣지(50030↔68050)를 소비했고, 팀을 넘나드는
(KT·한화) 연결이라 EXPERT 난이도에 걸맞다. evidence가 위키 원문과 정확히 일치한다.

## 22. RELATION_LINK (지식 · 그래프 관계, 커리어교차) — 2026-08-09 실행 사례

```json
{
  "quizId": "QZ-20260809-010",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "LG 천성호와 한화 심우준의 공통점은?",
  "options": [
    { "id": "A", "text": "상무 피닉스 야구단에서 함께 내야수로 복무" },
    { "id": "B", "text": "같은 고등학교 동창" },
    { "id": "C", "text": "신인 드래프트 동기" },
    { "id": "D", "text": "결혼식 들러리를 서준 사이" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/64006.md#커리어 이력",
    "quote": "상무 피닉스 야구단에서 내야수로 복무하며 박민·나승엽·천성호·권동진·구본혁 등과 함께 언급된 바 있다"
  },
  "settlement": null,
  "difficulty": "EXPERT",
  "pointReward": 120,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: `커리어교차` 엣지(50054↔64006)를 소비한 사례로, 상무 군 복무 시절
같은 부대에 있었다는 "아는 사람만 아는" 연결이라 EXPERT 재미가 있다. 오답 3개는
사실이 아닌 그럴듯한 공통점(distractor 전략)이다.

## 23. LAST_MATCHUP (지식 · 최근 맞대결) — 2026-08-10 실행 사례 (오늘 game_schedule 파티션 부재로 공통 문항만 생성)

```json
{
  "quizId": "QZ-20260810-004",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "HISTORY",
  "templateId": "LAST_MATCHUP",
  "format": "MULTI4",
  "question": "한화-KT 최근 맞대결(8/2) 스코어는?",
  "options": [
    { "id": "A", "text": "1:12" }, { "id": "B", "text": "12:1" },
    { "id": "C", "text": "4:3" }, { "id": "D", "text": "7:5" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/stats/season.md#상대전적",
    "quote": "- **HH vs KT**: HH 3승 KT 8승 · 0무 (최근 2026-08-02 1:12, 홈팀 승)"
  },
  "settlement": null,
  "difficulty": "MEDIUM",
  "pointReward": 50,
  "status": "PENDING",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 1:12라는 일방적인 스코어라 기억에 남는 사실이고, subjectScope=MATCHUP
규칙대로 정답 보기에 팀명 대신 스코어만 써서 정답 유출을 피했다. evidence가
`season.md` 원문과 정확히 일치한다. 같은 실행에서 HH-HT(7/23, 9:3)도 같은 패턴으로
생성했다 — 서로 다른 맞대결이라 중복이 아니다.

## 실행 메모: 재료 고갈로 인한 대량 폐기 (2026-08-10)

2026-08-10 실행은 최근 7일치 `quiz-candidates`(2026-08-07·2026-08-09)와 대조한 결과
아래 조합이 전부 "같은 사실을 다른 문구로" 재탕이라 **생성 단계에서부터 작성을
보류**했다(검증 패스까지 안 가고 걸러냄 — 재작성 시도조차 안 함):

- `WINNING_PITCHER`(20260804WOLT02026·20260804LGSK02026) — 2026-08-07에 이미
  "8/4 키움-롯데"·"8/4 LG-SSG" 승리투수 문제로 나간 바로 그 경기(같은 gameId).
  `recent7d` 파티션에 2026-08-04 이후 새 경기 결과가 아예 없다(6일째 game_result
  갱신 없음 — 소스 신선도 문제로 보임).
- `RELATION_LINK`(50030|68050, 50054|64006) — 2026-08-09에 이미 정확히 같은 페어로
  나간 문제(`QZ-20260809-009`·`QZ-20260809-010`)와 100% 동일한 evidence.
- `MEME_ORIGIN`·`MEME_OWNER`(50030) — 소형준 위키 문서의 별명·밈 소재(대형준
  워드플레이·투심러·신인왕 3신기)가 최근 3일 사이 이미 전부 소진됐다.
- `ALL_TIME_LEADER`(history-top-hitter)·`SEASON_STAT_LEADER`(AVG·ERA) —
  all-time-records.yaml·kbo-official.md 스냅샷이 2026-08-09 실행 이후 갱신되지 않아
  같은 인물이 같은 근거로 다시 나옴.
- `STANDINGS_CLIMB`(양방향) — 2026-08-07(상승폭)·2026-08-09(하락폭)에 이미 다뤄
  방향을 바꿔도 재탕.
- `TRENDING_WHO` — `wiki/stats/trending.md`가 2026-08-06 위키 빌더 실행분 그대로라
  2026-08-07에 이미 같은 1위(김대한)로 나갔다.
- `TEAM_RECORD_HISTORY`(2025) — 2026-08-09에 이미 나감(연도만 다른 2026은 생성).
- `H2H_SEASON_RECORD`(HH|KT) — 2026-08-07에 이미 나감. `H2H_SEASON_RECORD`(HH|HT)는
  시즌 전적이 6승 6무로 정확히 동률이라 템플릿의 우위/열세 이분법 자체가 성립하지
  않아 별도로 보류(재탕이 아니라 데이터가 템플릿 틀에 안 맞는 경우).
- `CAREER_PATH`(50030) — 위키 커리어 이력 섹션에 아시안게임 차출·이달의 투수 등은
  있지만 "거쳐간 팀"에 해당하는 이적 이력이 없어 템플릿 의도에 안 맞음.
- `CAREER_PATH`·`MEME_ORIGIN`·`MEME_OWNER`(50036) — 위키 문서의 해당 섹션이
  플레이스홀더 텍스트("확인된 사항 없음"/HTML 주석)뿐이라 실제 근거가 없음.
  `enumerate_entities`(wiki 섹션 필터)가 이 플레이스홀더를 "내용 있음"으로
  오판해 후보로 잡히는데, 실제로는 근거가 없어 걸러야 한다 — 워크플로 버그로
  보이므로 사람 검토를 제안한다.
- `RECORD_OX`(history-player-hitter·history-player-pitcher) — 같은 카테고리를
  `MILESTONE_FIRST`가 이미 다뤄 같은 rank:1 사실의 다른 문구 재탕이라 자체 판단으로
  제외.

결과적으로 오늘은 원래 계획한 36개 조합 중 14개만 실제로 작성했고, 그중 물량
슬롯(공통 HARD 4개) 초과로 2개(`MONTHLY_BEST`·`TEAM_RECORD_HISTORY`)가 재미 점수
동점 상황에서 추가로 빠져 최종 12개만 채택됐다. **EXPERT 난이도가 오늘 0개다** —
위키 밈·그래프 소재가 최근 3일간 소진되어 재료가 바닥났다는 신호로, 위키 빌더가
새 소재를 보충하기 전까지 반복될 가능성이 있다.

## 24. MEME_ORIGIN (지식 · 밈 유래, 이름 합성어) — 2026-08-11 실행 사례

```json
{
  "quizId": "QZ-20260811-028",
  "gameId": "20260811LGWO02026",
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_ORIGIN",
  "format": "MULTI4",
  "question": "LG 오스틴의 별명 '오카도'는 어떻게 만들어졌나?",
  "options": [
    { "id": "A", "text": "오스틴+아보카도를 합친 말장난" },
    { "id": "B", "text": "오스틴의 고향 지명에서 유래" },
    { "id": "C", "text": "오스틴의 타격 자세가 아보카도 같아서" },
    { "id": "D", "text": "동료 선수가 붙여준 스페인어 애칭" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/53123.md#별명·밈",
    "quote": "오스틴+아보카도를 합친 팬 별명."
  },
  "settlement": null,
  "difficulty": "EASY",
  "pointReward": 30,
  "subject": { "scope": "PLAYER", "playerIds": [53123], "teamCodes": [], "gameId": null },
  "status": "PENDING",
  "createdAt": "2026-08-11T00:16:40Z",
  "deadlineAt": "2026-08-11T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 이름 합성 밈(오스틴+아보카도)은 §4-1 고유성 검사를 자동으로 통과한다
— "대◯◯"류 범용 패턴과 달리 다른 선수 이름을 대입하면 아예 성립하지 않는 말장난이기
때문이다. evidence.quote도 위키 원문 한 글자와 다르지 않다(문장 끝 마침표 뒤
`[^ref3]` 각주 앞까지만 인용해 대조 실패를 피했다).

## 25. RELATION_LINK (지식 · 그래프 관계, 이름-소재 결합) — 2026-08-11 실행 사례

```json
{
  "quizId": "QZ-20260811-056",
  "gameId": "20260811LTSK02026",
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "다음 중 SSG 아빌라와 마드리스의 공통점은?",
  "options": [
    { "id": "A", "text": "이름이 유럽 축구 클럽을 연상시킨다는 밈으로 엮인다" },
    { "id": "B", "text": "둘 다 포수다" },
    { "id": "C", "text": "고교 시절부터 절친한 사이다" },
    { "id": "D", "text": "둘 다 국가대표 출신이다" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/56801.md#별명·밈",
    "quote": "같은 팀 외국인 타자 마드리스와 함께, 이름이 유럽 축구 클럽(레알 마드리드, 아스톤 빌라)을 연상시킨다는 이유로 팬들이 축구 클럽 이름을 엮어 부르는 밈이 생겼다(커뮤니티 전언)"
  },
  "settlement": null,
  "difficulty": "EXPERT",
  "pointReward": 120,
  "subject": { "scope": "PLAYER", "playerIds": [56801, 56813], "teamCodes": [], "gameId": null },
  "status": "PENDING",
  "createdAt": "2026-08-11T00:16:40Z",
  "deadlineAt": "2026-08-11T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 두 외국인 선수의 실명이 우연히 유럽 축구 클럽명(레알 마드리드·아스톤
빌라)과 겹치는, 재현 불가능한 우연성 소재라 고유성이 극단적으로 강하다. graph.json의
`커리어교차`가 아니라 개별 위키 문서 별명·밈 섹션에서 같은 사실을 상호 참조로 확인해
바인딩했다.

## 26. 2026-08-11 실행 메모 — 공통 문항 재료 소진이 구조적 문제로 굳어지는 중

2026-08-09·2026-08-10 사례에서 이미 지적된 공통 문항 재료 소진(`ALL_TIME_LEADER`·
`MILESTONE_FIRST`·`SEASON_STAT_LEADER`·`STANDINGS_CLIMB`·`MONTHLY_BEST`·
`TRENDING_WHO`)이 오늘도 그대로 반복됐다 — 최근 7일 이내 동일 사실 중복으로
10개 후보를 폐기했다(전량 공통 묶음, 상세는 이번 실행 요약 표 참고). 원인은 두
가지로 좁혀진다:

- `all-time-records.yaml`이 v0 초안 3개 카테고리(`history-player-hitter`·
  `history-player-pitcher`·`history-top-hitter`)뿐이라 `ALL_TIME_LEADER`·
  `MILESTONE_FIRST`가 매번 같은 rank 1~2 사실을 재탕할 수밖에 없다 — 카테고리를
  늘리거나(사람 검수 후) rank 3 이하까지 순환 출제하는 로직이 필요하다.
- `standingsTrend`(시즌 초 대비)·월별 성적(5월·7월처럼 이미 완결된 달)은 날짜가
  지나도 값 자체가 바뀌지 않는 정적 데이터라, 재집계를 다시 돌려도 어제 물었던
  질문과 100% 같은 사실이 나온다 — 이런 "완결된 과거" 지표는애초에 최근 N일
  회피 창의 대상이 아니라 "이번 시즌 1회만 출제"로 다루는 편이 맞을 수 있다.

오늘은 대체 조합(`RECORD_OX` rank 3 항목, `STANDINGS_NOW` 3위, `MEME_OWNER`
신규 인물 2명)으로 일부 메웠지만 공통 묶음은 결국 20슬롯 중 11개만 채웠다.
`kbo-official.md`(타/투 시즌 리더) 스냅샷도 2026-08-10 그대로라 `SEASON_STAT_LEADER`
전량 폐기했다 — py-collector 수집 주기와 이 routine 실행 주기가 어긋나 있는지
확인이 필요해 보인다.

## 27. MEME_ORIGIN (지식 · 밈 유래, 활약상 기반 별명) — 2026-08-12 실행 사례

```json
{
  "quizId": "QZ-20260812-001",
  "gameId": "20260812HHOB02026",
  "teamCodes": ["HH", "OB"],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_ORIGIN",
  "format": "MULTI4",
  "question": "두산 박지훈의 별명 '토템'은 어떤 이유로 붙었나?",
  "options": [
    { "id": "A", "text": "타격 툴이 부족해도 결정적 순간 대타로 승부를 뒤집어서" },
    { "id": "B", "text": "부적처럼 항상 같은 장비를 착용해서" },
    { "id": "C", "text": "큰 키가 토템폴을 연상시켜서" },
    { "id": "D", "text": "구단 마스코트 인형과 닮아서" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/50204.md#별명·밈",
    "quote": "뚜렷한 타격 툴은 없다는 평가를 받으면서도 결정적 순간 대타로 나와 승부를 뒤집는 일이 반복되며 '토템' 취급을 받는다"
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [50204], "teamCodes": [], "gameId": null },
  "difficulty": "EASY",
  "pointReward": 30,
  "status": "PENDING",
  "createdAt": "2026-08-12T00:13:15Z",
  "deadlineAt": "2026-08-12T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: "대◯◯"/"갓◯◯" 같은 범용 호칭이 아니라 이 선수의 구체적 활약
패턴(뚜렷한 툴 없이 결정적 대타 성공)에서 유래한, 다른 선수에게 그대로 옮겨
붙이면 어색한 고유 별명이다(§4-1 판별 질문 통과). 오답 3개도 전부 "그럴듯한
가짜 유래" 전략을 지켰고, 정답만 위키 원문을 그대로 인용했다.

## 28. RELATION_LINK (지식 · 그래프 관계, 커리어 이력 교차) — 2026-08-12 실행 사례

```json
{
  "quizId": "QZ-20260812-029",
  "gameId": "20260812LGWO02026",
  "teamCodes": ["LG", "WO"],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "키움 임지열과 이형종의 공통점은?",
  "options": [
    { "id": "A", "text": "7월 21일 1군 등록·말소가 서로 엇갈렸다" },
    { "id": "B", "text": "둘 다 왼손 투수 출신이다" },
    { "id": "C", "text": "고교 시절 같은 학교 동기다" },
    { "id": "D", "text": "국가대표 은퇴식을 함께 치렀다" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/64340.md#커리어 이력",
    "quote": "2026-07-21 1군 엔트리에서 말소됐다(같은 시점 이형종·배동현이 1군에 등록)"
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [64340, 78135], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT",
  "pointReward": 120,
  "status": "PENDING",
  "createdAt": "2026-08-12T00:13:15Z",
  "deadlineAt": "2026-08-12T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: graph.json 엣지가 아니라 두 선수의 `커리어 이력` 원문을 상호
대조해(한쪽 문서에 상대 이름이 같은 날짜로 언급됨) 찾아낸 조합이다. 등록/말소가
"엇갈렸다"는 사실 자체가 흥미로운 우연성이라 EXPERT 난이도에 걸맞다.

## 29. 2026-08-12 실행 메모 — 경기 문항 "직전 맞대결 = 어제 경기" 충돌 패턴

오늘 5경기 중 3경기(HH-OB, KT-NC, LG-WO, LT-SK — 사실상 대부분)에서
`LAST_MATCHUP`(직전 맞대결)의 대상이 하필 바로 어제(8/11) 열린 그 두 팀의
경기와 동일했다. 이미 같은 실행에서 `YESTERDAY_SCORE`/`YESTERDAY_WINNER`로
그 경기를 다뤘으므로, `LAST_MATCHUP`을 추가로 만들면 templateId만 다를 뿐
같은 사실(같은 gameId·같은 스코어)의 재질문이 된다 — 오늘은 전량 스킵 처리했다.
프로 야구는 로테이션상 이틀 연속 같은 매치업이 잦으므로 이 충돌은 구조적으로
반복될 가능성이 크다. 개선 아이디어: `LAST_MATCHUP` 바인딩 시 "가장 최근
맞대결"이 아니라 "가장 최근 **2번째** 맞대결"을 쓰거나, 같은 실행 내에서
YESTERDAY_* 계열이 이미 그 gameId를 썼으면 LAST_MATCHUP을 자동으로 건너뛰는
규칙을 카탈로그/바인딩 단계에 추가하는 것을 검토할 만하다.

또한 `PRED_UNDERDOG`(상대전적 열세팀 예측)가 두 경기(HH-OB 6승6패1무, LT-SK
6승6패1무)에서 정확히 동률이라 "열세 팀" 자체가 존재하지 않아 스킵됐다 —
`H2H_SEASON_RECORD`(우위/열세 이분법)도 같은 이유로 동반 스킵. 동률 상대전적은
이번이 세 번째 반복 관측(§23 HH|HT 사례와 동일 패턴)이라, 템플릿 전제("우위팀이
반드시 존재한다")를 손볼 필요가 있어 보인다 — 예: 동률일 때는 이 두 템플릿을
자동 제외하는 가드를 `runner/binding.py`의 entity 필터에 추가.

## 30. RELATION_LINK (지식 · 그래프 관계, 커리어 이력 교차) — 2026-08-13 실행 사례

```json
{
  "quizId": "QZ-20260813-050",
  "gameId": "20260813HHOB02026",
  "teamCodes": ["HH", "OB"],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "두산 안재석과 정수빈이 얽힌 8월 초 LG전 장면은?",
  "options": [
    { "id": "A", "text": "안재석 타구가 처리되는 사이 정수빈이 홈으로 파고들어 추가 득점을 만들었다" },
    { "id": "B", "text": "안재석의 병살타를 정수빈이 호수비로 커버했다" },
    { "id": "C", "text": "정수빈의 도루 실패를 안재석이 만회 홈런으로 갚았다" },
    { "id": "D", "text": "두 선수가 함께 부상으로 동시 교체됐다" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/51203.md#커리어 이력",
    "quote": "안재석의 타구가 처리되는 사이 이어진 런다운 상황에서 정수빈이 홈으로 파고들어 추가 득점을 만들어냈다"
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [51203, 79231], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT",
  "pointReward": 120,
  "status": "PENDING",
  "createdAt": "2026-08-13T00:10:18Z",
  "deadlineAt": "2026-08-13T14:59:00Z",
  "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 두 선수 문서에 각각 등장하는 같은 날짜·같은 상대(LG전)의 런다운
장면을 상호 대조해 찾아낸 조합이다(그래프 엣지의 `type: 커리어교차`가 가리키는
바로 그 사건). "머갈툴순" 4인방 밈처럼 이미 여러 날 반복 소비된 뻔한 조합
대신, 아직 안 쓴 세부 장면을 골라 신선도를 지켰다.

## 31. 2026-08-13 실행 메모 — RELATION_LINK 소재 5경기 중 1경기만 생존 + 디렉터리 불일치 발견

`LAST_MATCHUP`/`H2H_SEASON_RECORD` 충돌 패턴(§29)이 오늘도 그대로 반복됐다 —
오늘 5경기(HH-OB·KT-NC·LG-WO·LT-SK·SS-HT) **전부** 상대전적 `last` 날짜가
어제(8/12)라 두 템플릿을 전량 스킵했다. §29의 개선안(LAST_MATCHUP_PRIOR
템플릿 제안, 08-12 template-proposals 참고)이 아직 반영 전이라 계속
반복되는 중 — 우선순위를 높여도 좋을 사안으로 보인다.

`RELATION_LINK`(EXPERT)는 오늘 **5경기 중 HH-OB 1경기만** 신선한 소재가
있었다. 나머지 4경기(KT-NC·LG-WO·LT-SK·SS-HT)는 graph.json 엣지를 팀
소속으로 필터링한 결과 (a) 최근 7일 내 이미 썼거나 (b) 그 경기에 없는
팀 선수와 엮여 있거나(예: KT 유일 엣지가 HH 선수와의 라이벌 관계라 KT-NC엔
못 씀) (c) `사건연루`(금지)뿐이었다. `scoring.yaml` 운영 메모의 "OB 12 · SK 7 ·
HH 7 … KT 1"이라는 편차가 오늘 실측으로 그대로 재현됐다 — OB가 낀 경기만
안정적으로 채워지고 나머지는 그래프가 더 쌓이기 전까지 구조적으로 EXPERT
슬롯이 비게 된다.

**운영 버그 발견**: `runner/binding.py`·`runner/finalize.py`는 위키 경로를
`work / "wiki/players/..."`, `work / "wiki/graph.json"` 등으로 하드코딩하는데,
`ROUTINE.md` 1단계는 위키 리포를 `.work/wiki-repo/`에 클론하고
`.work/wiki-repo/wiki/`를 읽으라고 명시한다 — 두 경로가 어긋나
`finalize.check_evidence`가 위키 evidence를 전부 "파일 없음"으로 오판했다.
이번 실행은 `.work/wiki -> wiki-repo/wiki` 심볼릭 링크로 우회했다(파일
내용은 그대로, 참조 경로만 보정). 다음 실행부터도 같은 문제가 재현되므로,
`runner/binding.py`의 `DEFAULT_REPO_ROOT`처럼 `work` 하위 위키 경로도 인자로
받거나 `ROUTINE.md` 관례(`wiki-repo/wiki/`)에 맞춰 모듈을 고치는 편이
근본적이다 — 사람 검수 필요(카탈로그 변경이 아니라 러너 코드 변경이라 이
routine의 자동 수정 범위 밖).

## 32. RELATION_LINK (지식 · 그래프 관계) — 2026-08-14 실행 사례

```json
{
  "quizId": "QZ-20260814-XXX",
  "gameId": "20260814HHSS02026",
  "teamCodes": ["HH", "SS"],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "삼성 디아즈와 페덱의 공통점을 부르는 밈은?",
  "options": [
    { "id": "A", "text": "12년 만의 외국인 듀오" },
    { "id": "B", "text": "국민 오빠 듀오" },
    { "id": "C", "text": "다이나믹 듀오" },
    { "id": "D", "text": "라이언킹 콤비" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/54400.md#별명·밈",
    "quote": "**12년 만의 외국인 듀오**: 투수 페덱과 함께 2014년 밴덴헐크+나바로 조합(백인 투수+흑인 야수)에 빗대지는 밈."
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [54400, 56459], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: §5(2026-08-01)와 같은 디아즈-페덱 쌍이지만 오늘은 별개 세션(경기 문항
생성 에이전트)이 위키 문서를 처음부터 다시 읽어 독립적으로 재발견한 사례 — 관계가
`wiki/graph.json`뿐 아니라 각 선수 문서의 `별명·밈` 섹션에도 안정적으로 남아 있어
재현성이 높다는 뜻이다. evidence가 그래프가 아니라 위키 원문 그대로다.

## 33. CAREER_PATH (지식 · 커리어 비하인드) — 2026-08-14 실행 사례

```json
{
  "quizId": "QZ-20260814-XXX",
  "gameId": "20260814HHSS02026",
  "teamCodes": ["HH", "SS"],
  "kind": "KNOWLEDGE",
  "type": "CAREER",
  "templateId": "CAREER_PATH",
  "format": "MULTI4",
  "question": "페덱이 여러 구단 경쟁 끝에 삼성 입단을 택한 이유는?",
  "options": [
    { "id": "A", "text": "가장 먼저 연락한 구단이 삼성이라서" },
    { "id": "B", "text": "삼성의 연봉 제안이 가장 높아서" },
    { "id": "C", "text": "구단 트레이닝 시설이 마음에 들어서" },
    { "id": "D", "text": "가족이 한국에 거주해서" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/56459.md#커리어 이력",
    "quote": "일본 2팀·KBO 4팀 등 총 6개 팀 경쟁 끝에 \"제일 먼저 연락한 곳이 삼성\"이라는 이유로 삼성을 택했다는 비하인드가 전해진다."
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [56459], "teamCodes": [], "gameId": null },
  "difficulty": "HARD", "pointReward": 80, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: "거쳐간 팀 순서"가 아니라 "왜 그 팀을 골랐는가"라는 구체적 비하인드
숫자("총 6개 팀 경쟁")와 직접 인용("제일 먼저 연락한 곳이 삼성")을 그대로 살려
오답(연봉·시설·가족)도 그럴듯한 대안 서사로 성립한다 — CAREER_PATH가 단순 "어느 팀
출신" 사실 확인을 넘어 이야기성을 가질 수 있음을 보여준 사례(§34의 실패 사례와 대비됨).

## 34. RELATION_LINK (지식 · 자조적 밈) — 2026-08-14 실행 사례

```json
{
  "quizId": "QZ-20260814-XXX",
  "gameId": "20260814OBHT02026",
  "teamCodes": ["OB", "HT"],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "두산 박찬호와 정수빈을 함께 부르는 자조적 표현은?",
  "options": [
    { "id": "A", "text": "천민 (둘 다 신인 드래프트 5라운드 지명 출신)" },
    { "id": "B", "text": "쌍포듀오" },
    { "id": "C", "text": "동갑내기 절친" },
    { "id": "D", "text": "판타스틱4" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/64646.md#별명·밈",
    "quote": "정수빈과 함께 신인 드래프트 5라운드 지명 출신임을 가리키는 자조적 표현"
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [64646, 79231], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 두 선수 모두 OB 소속(같은 경기 묶음 자격 충족)이고, 관계의 근거가
"신인 드래프트 라운드"라는 검증 가능한 사실에 뿌리를 둬 창작 여지가 없다. 오답
3개는 흔한 듀오 별명 패턴(쌍포·절친·4인조)이라 그럴듯하되 사실이 아니다.

## 35. MEME_ORIGIN (지식 · 밈 유래, 팬 제정 시상식) — 2026-08-14 실행 사례

```json
{
  "quizId": "QZ-20260814-XXX",
  "gameId": "20260814SKLG02026",
  "teamCodes": ["SK", "LG"],
  "kind": "KNOWLEDGE", "type": "MEME", "templateId": "MEME_ORIGIN", "format": "MULTI4",
  "question": "LG 이영빈의 별명 '영빈옵'은 어디서 왔나?",
  "options": [
    { "id": "A", "text": "고교 선수 대상 '이영빈 타격상'이 그의 이름을 따 제정돼서" },
    { "id": "B", "text": "응원가 후렴구에서 파생된 말장난이라서" },
    { "id": "C", "text": "데뷔 첫 타석 만루홈런에서 유래해서" },
    { "id": "D", "text": "팬 투표로 뽑힌 별명 짓기 대회 우승작이라서" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/51100.md#별명·밈",
    "quote": "팬들 사이에서 이렇게 불리며, 고교 선수 대상 \"이영빈 타격상\"이 그의 이름을 따 제정되어 있다"
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [51100], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: §4-1 고유성 검사를 명확히 통과한다 — "타격상이 이름을 따 제정됨"은
이 선수만의 고유한 사실이라 다른 선수에게 옮겨 붙일 수 없다(범용 호칭 패턴이 아님).

## 36. RELATION_LINK (지식 · 공통 문항, 삼각 트레이드) — 2026-08-14 실행 사례

```json
{
  "quizId": "QZ-20260814-XXX",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE", "type": "CAREER", "templateId": "RELATION_LINK", "format": "MULTI4",
  "question": "KIA 고종욱과 SSG 이지영의 공통점은?",
  "options": [
    { "id": "A", "text": "2018년 김동엽과 함께 삼각 트레이드에 얽혔다" },
    { "id": "B", "text": "고교 시절 같은 팀 배터리였다" },
    { "id": "C", "text": "신인 시절 같은 방을 썼다" },
    { "id": "D", "text": "같은 지도자에게 사사받았다" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/61353.md#커리어 이력",
    "quote": "2018년 김동엽·이지영과 함께 삼각 트레이드에 포함된 이력이 있다는 정리가 있다."
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [61353, 79456], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 오늘 두 선수 모두 다른 팀의 경기 묶음에 속해(HT는 OB전, SK는 LG전)
어느 경기 묶음에도 "양 팀 소속만" 규칙을 못 지켜 공통 문항으로 돌린 사례 — 관계
자체는 양쪽 위키 문서의 커리어 이력 섹션에 서로를 이름으로 지목하며 교차 인용돼
근거가 이중으로 튼튼하다(§11 "관계형 근거는 위키 원문에서" 원칙과 부합).

## 37. 2026-08-14 실행 메모 — RECORD_OX 레전드 선수의 playerId 공백

`RECORD_OX`(`stats.all_time_records`)가 다루는 인물은 대부분 은퇴한 레전드
(백인천·서건창·김상훈 등)라 현재 시즌 위키 로스터(`wiki/players/*.md`)에 문서가
없고, 따라서 `kboPlayerId`를 알 수 없다. `subject.scope`는 카탈로그 선언대로
`PLAYER`를 써야 하는데(§11, 문항별 임의 변경 불가) `subject.playerIds`를 채울
근거 자료가 없어 빈 배열로 남기면 `validate_candidates.py` check 9의 카디널리티
검사("PLAYER→playerIds 1개 이상")에 걸려 하드 폐기된다. 오늘 실행에서 이렇게 2건
(QZ 가제 071·072)이 게이트 통과 직전에 걸러져 업로드에서 빠졌다 — quizId 번호가
연속되지 않는 이유다. 근본 해결은 사람 검수 필요: (a) `all-time-records.yaml`에
레전드용 임의 식별자를 추가하거나, (b) `RECORD_OX`의 `subjectScope`를 조건부로
`LEAGUE`로 재선언하거나(위키에 없는 과거 인물 한정) 둘 중 하나를 카탈로그·게이트
양쪽에서 결정해야 한다 — 이 routine은 카탈로그를 스스로 고치지 않으므로 이번엔
해당 후보 2건을 드롭하는 것으로 fail-closed 처리했다.

## 38. MEME_ORIGIN (지식 · 밈 유래, 경기 묶음) — 2026-08-15 실행 사례

```json
{
  "quizId": "QZ-20260815-039",
  "gameId": "20260815HHSS02026",
  "teamCodes": ["HH", "SS"],
  "kind": "KNOWLEDGE", "type": "MEME", "templateId": "MEME_ORIGIN", "format": "MULTI4",
  "question": "채은성의 별명 '채버지'는 무엇의 합성어인가?",
  "options": [
    { "id": "A", "text": "채은성 + 아버지" },
    { "id": "B", "text": "채은성 + 대장" },
    { "id": "C", "text": "채은성 + 형님" },
    { "id": "D", "text": "채은성 + 선생님" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/79192.md#별명·밈",
    "quote": "**채버지**: '채은성'과 '아버지'를 합친 애칭으로, 팬들이 그를 든든하게 여기는 마음을 담아 부르는 표현이다(커뮤니티 전언)."
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [79192], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 이름 자체를 변형한 구체적 합성어("채은성"+"아버지")라 "대◯◯"·"◯느님"
같은 범용 호칭 패턴(§4-1)이 아니고, 오답 3개도 같은 "애칭+가족/존칭 단어" 형식으로
길이·형식이 균일해 정답만 티나지 않는다.

## 39. CAREER_PATH (지식 · 트레이드/FA 서사) — 2026-08-15 실행 사례

```json
{
  "quizId": "QZ-20260815-010",
  "gameId": "20260815HHSS02026",
  "teamCodes": ["HH", "SS"],
  "kind": "KNOWLEDGE", "type": "CAREER", "templateId": "CAREER_PATH", "format": "MULTI4",
  "question": "강백호가 FA로 한화 이적 전 몸담았던 팀은?",
  "options": [
    { "id": "A", "text": "KT" }, { "id": "B", "text": "LG" },
    { "id": "C", "text": "두산" }, { "id": "D", "text": "SSG" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/68050.md#커리어 이력",
    "quote": "KT 소속이었다가 자유계약(FA)으로 한화에 이적했으며, 계약 규모가 100억 원대라는 언급이 반복적으로 나온다."
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [68050], "teamCodes": [], "gameId": null },
  "difficulty": "HARD", "pointReward": 80, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: bad.md §6("데뷔팀=현재팀" 동어반복)과 정확히 대비되는 사례 — 실제
FA 이적이라는 뚜렷한 서사가 있고 100억대 계약이라는 화제성까지 딸려 있다. CAREER_PATH
후보를 고를 때 evidence 문장에 "트레이드"·"FA"·"이적" 등 실제 팀 이동 표현이
있는지 먼저 거르라는 §6의 제안이 여기서도 유효하게 작동했다.

## 40. LAST_MATCHUP (지식 · 상대전적, MATCHUP scope) — 2026-08-15 실행 사례

```json
{
  "quizId": "QZ-20260815-027",
  "gameId": "20260815WOKT02026",
  "teamCodes": ["WO", "KT"],
  "kind": "KNOWLEDGE", "type": "HISTORY", "templateId": "LAST_MATCHUP", "format": "MULTI4",
  "question": "키움-KT 직전 맞대결(8/14) 결과는?",
  "options": [
    { "id": "A", "text": "3:8, 홈팀 승" }, { "id": "B", "text": "8:3, 원정팀 승" },
    { "id": "C", "text": "5:4, 홈팀 승" }, { "id": "D", "text": "3:8, 원정팀 승" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/stats/season.md#상대전적",
    "quote": "- **KT vs WO**: KT 9승 WO 2승 · 1무 (최근 2026-08-14 3:8, 홈팀 승)"
  },
  "settlement": null,
  "subject": { "scope": "MATCHUP", "playerIds": [], "teamCodes": ["KT", "WO"], "gameId": null },
  "difficulty": "MEDIUM", "pointReward": 50, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 오늘 매치업의 실제 어제 대결이라 시의성이 최고로 높고, §11의
MATCHUP scope 규칙대로 정답 보기에 팀명 대신 "홈팀/원정팀 승" 표현만 써서 정답
유출도 없다(같은 실행에서 YESTERDAY_SCORE는 이 규칙을 놓쳐 폐기 사례가 나왔다 —
아래 §12 참고).

## 41. RELATION_LINK (지식 · EXPERT, 트레이드 서사) — 2026-08-15 실행 사례

```json
{
  "quizId": "QZ-20260815-074",
  "gameId": "20260815OBHT02026",
  "teamCodes": ["OB", "HT"],
  "kind": "KNOWLEDGE", "type": "MEME", "templateId": "RELATION_LINK", "format": "MULTI4",
  "question": "KIA 한재승과 NC 이우성의 공통점은?",
  "options": [
    { "id": "A", "text": "3:3 트레이드로 각자 지금 팀에 합류했다" },
    { "id": "B", "text": "둘 다 국가대표 주전 유격수 출신이다" },
    { "id": "C", "text": "고향이 같은 동향 선수다" },
    { "id": "D", "text": "프로 데뷔팀이 같았던 입단 동기다" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/51994.md#커리어 이력",
    "quote": "3:3 트레이드로 KIA에 합류했으며, 김시훈·정현창과 함께 KIA로 오고 이우성·최원준·홍종표가 NC로 이동한 트레이드였다는 게시글이 있다"
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [51994, 63260], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: '머갈툴순' 4인방 밈공유 엣지가 이번 주 이미 여러 번 반복 소비돼
피로도가 높았는데(§8 참고), 완전히 다른 스토리라인(3:3 트레이드)의 커리어교차
엣지를 찾아내 신선도를 확보했다. graph.json 엣지가 아니라 원 소속 위키 문서의
커리어 이력 원문에서 그대로 근거를 뽑아 대조에도 안전하다.

## 42. MEME_OWNER (지식 · 공통 문항, LEAGUE scope) — 2026-08-15 실행 사례

```json
{
  "quizId": "QZ-20260815-043",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE", "type": "MEME", "templateId": "MEME_OWNER", "format": "MULTI4",
  "question": "배우 맷 데이먼과 이름이 헷갈려 화제가 된 선수는?",
  "options": [
    { "id": "A", "text": "데이비슨" }, { "id": "B", "text": "이형종" },
    { "id": "C", "text": "임지열" }, { "id": "D", "text": "김동헌" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/54944.md#별명·밈",
    "quote": "**맷 데이먼 혼동 밈**: 이름이 비슷한 할리우드 배우 맷 데이먼이 영화 홍보차 방한 중 고척돔에서 데이비슨의 유니폼을 입고 직관한 사실이 알려지며, \"데이비슨의 지인·친척·사촌\"이라는 소문과 함께 두 사람의 이름을 헷갈리는 밈이 커뮤니티에서 크게 확산됐다(커뮤니티 전언)"
  },
  "settlement": null,
  "subject": { "scope": "LEAGUE", "playerIds": [], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 실제 방한 에피소드에서 비롯된 구체적 일화라 고유성이 명백하고,
경기 묶음이 아닌 공통 묶음(LEAGUE scope)에서도 특정 선수·팀에 매몰되지 않는
재미를 살렸다 — MEME_OWNER를 오늘 8개 구단에 고루 분산시킨 것도 공통 묶음의
"특정 팀 편중 없이" 원칙(ROUTINE.md 3단계)에 부합한다.
