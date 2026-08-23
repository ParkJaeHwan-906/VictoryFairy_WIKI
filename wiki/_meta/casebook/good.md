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

## 43. MEME_ORIGIN (지식 · 경기 문항, PLAYER scope) — 2026-08-16 실행 사례

```json
{
  "quizId": "QZ-20260816-008",
  "gameId": "20260816NCLT02026",
  "teamCodes": ["NC", "LT"],
  "kind": "KNOWLEDGE", "type": "MEME", "templateId": "MEME_ORIGIN", "format": "MULTI4",
  "question": "나승엽의 별명 '나푼이'의 유래는?",
  "options": [
    { "id": "A", "text": "이름 '나승엽'에 어수룩한 느낌의 접미사를 결합, 잦은 수비 실수와 함께 쓰임" },
    { "id": "B", "text": "나이가 가장 어린 선수라서" },
    { "id": "C", "text": "나눔 활동에 앞장서서" },
    { "id": "D", "text": "나선형 타구를 자주 쳐서" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/51551.md#별명·밈",
    "quote": "- **나푼이**: 이름 \"나승엽\"에 어수룩한 느낌을 주는 접미사를 결합한 별명으로, 잦은 수비 실수와 함께 쓰인다(커뮤니티 전언)[^ref5]."
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [51551], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 이름 자체를 재료로 한 언어유희형 별명이라 §4-1의 "범용 호칭 패턴"
(대◯◯·갓◯◯류)에 해당하지 않고 나승엽 개인에게 고유하다. NC-롯데 경기 묶음(오늘
매치업)에서 롯데 소속 선수만 다뤄 "경기 문항에 다른 팀 선수를 섞지 않는다"
원칙도 지켰다. 오답 보기 3개는 전부 그럴듯한 가짜 유래(생성 규칙 §1 distractor
전략)로, 정답만 유난히 길지 않게 길이를 맞췄다.

## 44. RELATION_LINK (지식 · 공통 문항, EXPERT) — 2026-08-16 실행 사례

```json
{
  "quizId": "QZ-20260816-022",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE", "type": "MEME", "templateId": "RELATION_LINK", "format": "MULTI4",
  "question": "KT 소형준과 함께 '신인왕 3신기'로 묶여 불리는 한화 선수는?",
  "options": [
    { "id": "A", "text": "강백호" }, { "id": "B", "text": "채은성" },
    { "id": "C", "text": "노시환" }, { "id": "D", "text": "문동주" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/50030.md#별명·밈",
    "quote": "- **신인왕 3신기**: 강백호·안현민과 함께 묶여 불리는 표현이다(커뮤니티 전언)[^ref4]."
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [50030, 68050], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 오늘 KT·한화는 서로 다른 경기(각각 WO-KT·HH-SS 묶음)에 속해 있어
공통 묶음(LEAGUE 귀속)에 배치한 것이 정확하다 — RELATION_LINK는 재료 상한이 낮은
유일한 EXPERT 템플릿(scoring.yaml 주석)이라 이렇게 오늘 경기와 무관한 교차팀
쌍을 공통 묶음에서 소비하는 편이 매일 1개뿐인 슬롯을 안정적으로 채우는 길이다.
소형준·강백호 양쪽 위키 문서에 독립적으로 같은 사실("신인왕 3신기")이 적혀 있어
대조도 이중으로 확인됐다.

## 45. MEME_ORIGIN (지식 · 공통 문항, 경기 없는 날) — 2026-08-17 실행 사례

```json
{
  "quizId": "QZ-20260817-007",
  "gameId": null,
  "teamCodes": [],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_ORIGIN",
  "format": "MULTI4",
  "question": "박영현의 별명 '세이브 집착 드립'이 붙은 이유는?",
  "options": [
    { "id": "A", "text": "점수차가 커도 세이브 상황에 집착하듯 언급해서" },
    { "id": "B", "text": "실제 세이브를 못 올려 놀림받아서" },
    { "id": "C", "text": "등번호가 세이브 수와 같아서" },
    { "id": "D", "text": "은퇴 후 세이브 해설위원이 되고 싶다고 해서" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/52060.md#별명·밈",
    "quote": "- **세이브 집착 드립**: 팀이 큰 점수 차로 앞서갈 때마다 \"이게 세이브인가요?\" 식으로 세이브 상황에 집착하는 듯한 패러디 인용구가 커뮤니티에서 반복적으로 쓰인다 (커뮤니티 전언).[^ref3][^ref4][^ref5]"
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [52060], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 2026-08-17(월)은 KBO 전 경기 없는 날이라 경기 문항이 0개였다
(`ROUTINE.md` "경기가 없는 날" 절 — 공통 묶음에 평소 경기 묶음 전용 PLAYER/TEAM
템플릿을 투입해 채운다). 이 후보는 그 폴백으로 고른 위키 별명 소재 중 하나이며,
오답 3개(B/C/D)가 전부 "그럴듯하지만 근거 없는" 창작 유래로, 정답과 형식·길이가
비슷해 답이 티나지 않는다. 또한 최근 7일 `quiz-candidates` 대조에서 이 선수가
처음 등장하는 것도 확인했다(아래 bad.md #15 — 오늘처럼 정적 소재로만 채워야
하는 날은 이 대조가 특히 중요하다).

## 46. MEME_ORIGIN (지식 · 경기 문항, PLAYER scope) — 2026-08-18 실행 사례

```json
{
  "quizId": "QZ-20260818-033",
  "gameId": "20260818HTHH02026",
  "teamCodes": ["HH", "HT"],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_ORIGIN",
  "format": "MULTI4",
  "question": "김태군의 별명 '군기반장'은 왜 붙었나?",
  "options": [
    { "id": "A", "text": "복귀 후 후배들을 엄하게 다잡는 리더십 때문에" },
    { "id": "B", "text": "부상 없이 최다 출전을 이어가서" },
    { "id": "C", "text": "심판에게 항의를 자주 해서" },
    { "id": "D", "text": "신인 시절 팀 규율을 어겨서" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/78122.md#별명·밈",
    "quote": "부상에서 복귀한 뒤 한준수·네일 등 후배들을 엄하게 다잡는 리더십을 보여 붙은 별명. 관련 기사 제목이 \"기아의 군기반장 형님이 돌아왔다\"였다."
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [78122], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 기사 제목까지 인용된 구체적 유래라 오답과 헷갈릴 여지가 없다. 이
실행(2026-08-18, 5경기 전부 열린 날)은 5개 경기 묶음을 병렬 서브에이전트로
분담 생성했는데, 이 후보는 HT-HH 경기 묶음 담당 에이전트가 만든 것 — 선수(김태군,
HT 소속)가 그 경기 양 팀(HT·HH) 안에 있어 게임 묶음 순도 규칙을 지켰다.

## 47. RELATION_LINK (지식 · 공통 문항 아님, 경기 묶음 EXPERT) — 2026-08-18 실행 사례

```json
{
  "quizId": "QZ-20260818-072",
  "gameId": "20260818OBNC02026",
  "teamCodes": ["NC", "OB"],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "RELATION_LINK",
  "format": "MULTI4",
  "question": "두산 안재석과 정수빈의 공통 장면은?",
  "options": [
    { "id": "A", "text": "LG전 런다운으로 만든 추가 득점" },
    { "id": "B", "text": "같은 해 신인왕 동반 수상" },
    { "id": "C", "text": "국가대표 동반 발탁" },
    { "id": "D", "text": "함께 개인 통산 첫 만루홈런" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/79231.md#커리어 이력",
    "quote": "2026년 8월 초 LG전에서 안재석의 타구가 처리되는 사이 런다운 상황에서 과감하게 홈으로 파고들어 추가 득점을 만들어냈다(커뮤니티 전언)."
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [51203, 79231], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 같은 팀(두산) 두 선수가 함께 등장하는 구체적 수비 장면이라 EXPERT
난이도에 걸맞은 "직관러만 아는" 재미가 있다. OB-NC 경기 묶음 담당 에이전트가
생성했고 두 선수 모두 OB 소속이라 순도 규칙(양 팀 소속만)을 만족한다.

## 48. PRED_UNDERDOG (예측 · 경기 문항, 상대전적 열세) — 2026-08-18 실행 사례

```json
{
  "quizId": "QZ-20260818-062",
  "gameId": "20260818SKSS02026",
  "teamCodes": ["SK", "SS"],
  "kind": "PREDICTION",
  "type": "WIN_LOSE",
  "templateId": "PRED_UNDERDOG",
  "format": "BINARY",
  "question": "상대전적 4승7패 열세 SSG, 오늘 이변 만들까?",
  "options": [{ "id": "A", "text": "SSG 승" }, { "id": "B", "text": "SSG 패" }],
  "answer": null,
  "evidence": null,
  "settlement": { "gameId": "20260818SKSS02026", "metric": "WIN_TEAM" },
  "subject": { "scope": "GAME", "playerIds": [], "teamCodes": ["SK", "SS"], "gameId": "20260818SKSS02026" },
  "difficulty": "MEDIUM", "pointReward": 50, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: 시즌 상대전적(SK 4승 SS 7승)을 문구 양념으로 자연스럽게 녹였고,
`evidence`는 예측 퀴즈 규칙대로 `null`, `settlement.metric`은 카탈로그 값
(`WIN_TEAM`)을 그대로 썼다. SS가 3연승 중이라는 흐름까지 겹쳐 "이변" 프레이밍이
설득력 있다.

## 49. MEME_ORIGIN (지식 · 경기 문항, PLAYER scope) — 2026-08-18 실행 사례

```json
{
  "quizId": "QZ-20260818-029",
  "gameId": "20260818OBNC02026",
  "teamCodes": ["NC", "OB"],
  "kind": "KNOWLEDGE",
  "type": "MEME",
  "templateId": "MEME_ORIGIN",
  "format": "MULTI4",
  "question": "NC 이우성 별명 '바빕신'의 유래는?",
  "options": [
    { "id": "A", "text": "타구가 수비 사이로 잘 빠져서" },
    { "id": "B", "text": "빕스 광고에 출연해서" },
    { "id": "C", "text": "타격폼이 독특해서" },
    { "id": "D", "text": "생일이 팀 창단일과 겹쳐서" }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/63260.md#별명·밈",
    "quote": "타구가 수비수 사이로 잘 빠지며 안타·추가 득점으로 이어지는 장면이 화제가 되면서 커뮤니티에서 붙은 별명(커뮤니티 전언)"
  },
  "settlement": null,
  "subject": { "scope": "PLAYER", "playerIds": [63260], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30, "status": "PENDING", "createdBy": "AI_ENGINE"
}
```

**좋은 이유**: "바빕신"이라는 발음 유사 밈("빕스"를 연상시키는 이름 소리)에 낚이기
쉬운 오답(B)을 넣어 즉답이 아니라 살짝 생각하게 만들면서도, 정답은 위키 원문을
그대로 인용해 대조에 바로 통과한다.

## 50. MEME_ORIGIN (지식 · 경기 문항, PLAYER scope) — 2026-08-19 실행 사례

```json
{
  "quizId": "QZ-20260819-022",
  "gameId": "20260819OBNC02026",
  "teamCodes": ["OB", "NC"],
  "templateId": "MEME_ORIGIN",
  "question": "두산 박지훈의 별명 '토템(행운의 부적)'은 왜 붙었나?",
  "options": [
    { "id": "A", "text": "뚜렷한 타격 툴은 없지만 결정적 순간 대타로 나와 승부를 뒤집는 일이 반복돼서" },
    { "id": "B", "text": "수비 훈련 때마다 부적을 몸에 지니고 다녀서" },
    { "id": "C", "text": "데뷔 첫 타석에서 만루홈런을 쳐서" },
    { "id": "D", "text": "감독이 미신처럼 아끼는 선수라서" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/50204.md#별명·밈",
    "quote": "뚜렷한 타격 툴은 없다는 평가를 받으면서도 결정적 순간 대타로 나와 승부를 뒤집는 일이 반복되며 '토템' 취급을 받는다(커뮤니티 전언)." },
  "subject": { "scope": "PLAYER", "playerIds": [50204], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30
}
```

**좋은 이유**: '토템'이라는 별명이 왜 붙었는지 원문 그대로 자연스럽게 풀어 쓰면서도,
오답 B·C·D는 전형적인 야구 미신/징크스 소재를 그럴듯하게 섞어 정답을 바로
알아채기 어렵게 했다. 이 선수 고유의 서사(대타 성공률)라 §4-1 범용 패턴
테스트(다른 잘하는 선수에게 옮겨도 자연스러운가?)를 통과한다.

## 51. MEME_OWNER (지식 · 공통 문항, LEAGUE scope) — 2026-08-19 실행 사례

```json
{
  "quizId": "QZ-20260819-034",
  "gameId": null,
  "teamCodes": [],
  "templateId": "MEME_OWNER",
  "question": "가수 이름에 빗댄 별명 '카더가든'의 주인공은?",
  "options": [
    { "id": "A", "text": "카라스코" },
    { "id": "B", "text": "요니 치리노스" },
    { "id": "C", "text": "김윤식" },
    { "id": "D", "text": "임찬규" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/56103.md#별명·밈",
    "quote": "**카더가든**: 이름을 비슷한 어감의 가수 이름에 빗댄 말장난(커뮤니티 전언)." },
  "subject": { "scope": "LEAGUE", "playerIds": [], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30
}
```

**좋은 이유**: MEME_ORIGIN(유래를 묻는 문제)과 짝을 이루는 MEME_OWNER(주인공을
묻는 문제)의 좋은 예 — 오답 3명이 전부 실존 외국인/국내 선수라 "이름이 낯설어서
소거법으로 찍는" 상황을 막았다(전원 그럴듯한 후보). 정답 유출 우려도 없다(scope=LEAGUE,
전 축이 빈다).

## 52. RELATION_LINK (지식 · 경기 문항, PLAYER scope, EXPERT) — 2026-08-19 실행 사례

```json
{
  "quizId": "QZ-20260819-072",
  "gameId": "20260819SKSS02026",
  "teamCodes": ["SK", "SS"],
  "templateId": "RELATION_LINK",
  "question": "SSG 최용준과 이준기의 공통점은?",
  "options": [
    { "id": "A", "text": "'추신수 육성 프로젝트'로 함께 일본에 파견됐다" },
    { "id": "B", "text": "고교 시절 배터리로 호흡을 맞췄다" },
    { "id": "C", "text": "2024 신인 드래프트 동기다" },
    { "id": "D", "text": "같은 날 나란히 1군 데뷔전을 치렀다" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/50650.md#커리어 이력",
    "quote": "'추신수 육성 프로젝트'의 일환으로 이준기 등 유망주 4인과 함께 일본에 파견되었다(커뮤니티 전언)." },
  "subject": { "scope": "PLAYER", "playerIds": [50650, 51702], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120
}
```

**좋은 이유**: 실제 `graph.json`에 `커리어교차` 엣지로 등재된 쌍(50650|51702)을
그대로 썼고, evidence quote가 그 관계를 명시적으로 서술한다("~와 함께 일본에
파견"). 오답도 야구 서사에서 흔한 "배터리·드래프트 동기·동시 데뷔" 패턴이라
그럴듯하다. 아래 53번 항목(bad.md)과 대조해서 볼 것 — 겉보기 형태는 비슷해도
graph.json 엣지 존재 여부가 이 템플릿의 진짜 게이트다.

## 53. TRENDING_WHO (지식 · 공통 문항, LEAGUE scope) — 2026-08-19 실행 사례

```json
{
  "quizId": "QZ-20260819-079",
  "gameId": null,
  "teamCodes": [],
  "templateId": "TRENDING_WHO",
  "question": "최근 한 달 커뮤니티 최다 화제 선수는? (이번 실행 기준 스냅샷)",
  "options": [
    { "id": "A", "text": "김도영" },
    { "id": "B", "text": "김대한" },
    { "id": "C", "text": "페덱" },
    { "id": "D", "text": "디아즈" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/stats/trending.md#화제 선수 Top 10",
    "quote": "| 1 | 김도영 | 52605 | 914 | 홈런 페이스/시즌 성적(타격·수비 논쟁) |" },
  "subject": { "scope": "LEAGUE", "playerIds": [], "teamCodes": [], "gameId": null },
  "difficulty": "MEDIUM", "pointReward": 50
}
```

**좋은 이유**: 오답 2~4위 선수들도 실제 언급 수 상위권이라 근소한 차이로 헷갈릴
여지가 있고, "이번 실행 기준 스냅샷"이라는 시의성 문구로 trending.md가 매 실행
전체 재수집된다는 사실(누적 아님)을 문제 문면에서도 정직하게 반영했다.

## 54. MEME_ORIGIN (지식 · 경기 문항, PLAYER scope) — 2026-08-20 실행 사례

```json
{
  "quizId": "QZ-20260820-025",
  "gameId": "20260820HTHH02026",
  "teamCodes": ["HT", "HH"],
  "templateId": "MEME_ORIGIN",
  "question": "'망해영' 별명의 유래는?",
  "options": [
    { "id": "A", "text": "블론세이브 등으로 부진한 등판을 한 뒤 커뮤니티에서…" },
    { "id": "B", "text": "망해영 시절 응원가 후렴구에서 유래" },
    { "id": "C", "text": "입단 첫 해 등번호 말버릇에서 유래" },
    { "id": "D", "text": "고교 시절 별명을 그대로 이어받음" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/50662.md#별명밈",
    "quote": "**망해영**: 블론세이브 등으로 부진한 등판을 한 뒤 커뮤니티에서 붙는 별명이다(커뮤니티 전언)." },
  "subject": { "scope": "PLAYER", "playerIds": [50662], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30
}
```

**좋은 이유**: 별명 자체가 이름 성적 상태를 비꼬는 구체적 서사("블론세이브 등
부진")를 담고 있어 §4-1의 범용 호칭(대◯◯·갓◯◯류)이 아니라 고유성이 뚜렷하다.
정답 문구는 evidence 원문에서 footnote·괄호만 걷어낸 길이라 오답 3개와 형식이
비슷하고, 오답들도 "응원가 유래/등번호 말버릇/고교 별명 승계"처럼 실제 KBO
밈 생성 경로에서 흔한 패턴이라 그럴듯하다.

## 55. RELATION_LINK (지식 · 경기 문항, PLAYER scope, EXPERT) — 2026-08-20 실행 사례

```json
{
  "quizId": "QZ-20260820-069",
  "gameId": "20260820OBNC02026",
  "teamCodes": ["OB", "NC"],
  "templateId": "RELATION_LINK",
  "question": "안재석과 김민석의 공통점은?",
  "options": [
    { "id": "A", "text": "같은 밈으로 함께 묶여 불린다" },
    { "id": "B", "text": "같은 고교 동문이다" },
    { "id": "C", "text": "같은 해 신인왕 후보였다" },
    { "id": "D", "text": "같은 등번호를 썼던 적이 있다" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/51203.md",
    "quote": "**머갈툴순 중 '갈'**: 두산의 유망주 김대한('머')·안재석('갈')·김민석('툴')·박준순('순')을 묶어 부르는 별칭으로 …" },
  "subject": { "scope": "PLAYER", "playerIds": [51203, 53554], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120
}
```

**좋은 이유**: graph.json에 `밈공유` 엣지로 등재된 쌍을 그대로 썼고, 정답 옵션은
"같은 밈으로 함께 묶여 불린다"처럼 관계 유형 자체를 답으로 삼아 문항마다 정답
문구가 인물 이름에 흔들리지 않도록 정형화했다(§7 quizId 결정성과 같은 원칙 —
정답 축을 데이터가 아니라 관계 타입으로 고정). 이 경기 묶음에서 같은 "머갈툴순"
4인방 조합으로 만든 다른 RELATION_LINK 후보(안재석-박준순)도 슬롯 초과로 함께
살아남았는데, 밈 하나로 최대 6쌍(4C2)까지 서로 다른 후보를 뽑을 수 있어
재료 밀도가 높은 좋은 사례다.

## 56. RELATION_LINK (지식 · 경기 문항, PLAYER scope, EXPERT) — 2026-08-21 실행 사례

```json
{
  "quizId": "QZ-20260821-054",
  "gameId": "20260821HTWO02026",
  "teamCodes": ["HT", "WO"],
  "templateId": "RELATION_LINK",
  "question": "KIA 김범수와 하주석의 공통점은?",
  "options": [
    { "id": "A", "text": "2026년 한화에서 KIA로 함께 트레이드된 동료" },
    { "id": "B", "text": "고교 시절부터의 배터리 동기" },
    { "id": "C", "text": "국가대표팀에서 룸메이트로 지낸 사이" },
    { "id": "D", "text": "같은 에이전시 소속 선수" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/65769.md#커리어 이력",
    "quote": "2026년 하주석·이태양과 함께 KIA로 이적했고, KIA가 한화에 현금 20억 원과 투수 이형범을 내주는 트레이드였다는 전언도 있다" },
  "subject": { "scope": "PLAYER", "playerIds": [65769, 62700], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120
}
```

**좋은 이유**: 그래프 쌍(65769↔62700, HT 내부 커리어교차 엣지)을 그대로 쓰되,
정답을 "트레이드 동료"라는 구체적 사건으로 명시하고 오답 3개(배터리 동기·룸메이트·
에이전시)를 전부 "선수 간 인연"이라는 같은 범주의 그럴듯한 오답으로 맞췄다.
같은 팀(HT) 내부 관계도 §11 규칙상 아무 문제 없다 — subjectScope=PLAYER는
"두 선수가 어느 팀 소속인지"가 아니라 "관계가 실제로 존재하는지"만 요구하므로,
경기 상대팀(WO)과 무관하게 HT 내부 인연으로도 그 경기 묶음의 EXPERT 슬롯을
채울 수 있다는 좋은 선례다.

## 57. PRED_UNDERDOG (예측 · 경기 문항, GAME scope, MEDIUM) — 2026-08-21 실행 사례

```json
{
  "quizId": "QZ-20260821-039",
  "gameId": "20260821KTSK02026",
  "teamCodes": ["KT", "SK"],
  "templateId": "PRED_UNDERDOG",
  "question": "상대전적 5승 7패로 열세인 KT, 오늘은 SK를 이길까?",
  "options": [
    { "id": "A", "text": "KT 승" },
    { "id": "B", "text": "SK 승" }
  ],
  "answer": null,
  "evidence": null,
  "settlement": { "gameId": "20260821KTSK02026", "metric": "WIN_TEAM" },
  "subject": { "scope": "GAME", "playerIds": [], "teamCodes": ["KT", "SK"], "gameId": "20260821KTSK02026" },
  "difficulty": "MEDIUM", "pointReward": 50
}
```

**좋은 이유**: 상대전적 열세 수치(5승 7패)를 문구에 직접 제시해 §3 규칙("열세
수치를 문구에 제시")을 그대로 지켰고, 실제로는 KT가 이 시즌 순위표 1위(.606)라
"전적상 열세 vs 순위상 우세"라는 흥미로운 긴장을 자연히 만든다 — 데이터를
꾸미지 않고 있는 그대로 나열했을 뿐인데 서사가 생긴 사례. 같은 경기 묶음
안에서 KTSK-06(같은 소재였던 MEME_ORIGIN)이 최근 7일 중복으로 폐기된 반면,
이 예측형은 "오늘 경기"라는 시의성 자체가 매일 새 엔티티라 구조적으로
중복 위험이 없다는 점도 PREDICTION 템플릿군의 장점으로 눈여겨볼 만하다.

## 58. RELATION_LINK (지식 · 경기 문항, PLAYER scope, EXPERT) — 2026-08-21 실행 사례

```json
{
  "quizId": "QZ-20260821-052",
  "gameId": "20260821LGHH02026",
  "teamCodes": ["LG", "HH"],
  "templateId": "RELATION_LINK",
  "question": "LG 천성호와 한화 심우준의 공통점은?",
  "options": [
    { "id": "A", "text": "상무 피닉스 야구단에서 함께 내야수로 복무했다" },
    { "id": "B", "text": "같은 고등학교를 졸업했다" },
    { "id": "C", "text": "2019년 신인 드래프트 동기다" },
    { "id": "D", "text": "결혼식 들러리를 서준 사이다" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/50054.md#커리어 이력",
    "quote": "상무 피닉스 야구단에서 내야수로 복무하며 박민·나승엽·심우준·권동진·구본혁 등과 함께 언급된 바 있다(커뮤니티 전언)" },
  "subject": { "scope": "PLAYER", "playerIds": [50054, 64006], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120
}
```

**좋은 이유**: 이 경기 묶음(LG-HH)의 두 선수가 서로 **다른 팀** 소속인 진짜
교차형 RELATION_LINK다(그래프 쌍 50054↔64006, 커리어교차 엣지) — 같은 팀 내부
쌍보다 재료 밀도가 낮은 유형인데도 위키 원문(상무 복무 명단)에서 실제 공통점을
정확히 뽑아냈다. 병역 이행 자체는 §4의 금지 소재(병역 "비리"·기피 논란 등)가
아니라 중립적 커리어 사실이라 안전 재검도 무리 없이 통과한다.

## 59. RELATION_LINK (지식 · 경기 문항, PLAYER scope, EXPERT) — 2026-08-21 실행 사례, 원문의 민감 표현을 문항 표면에 노출하지 않은 사례

```json
{
  "quizId": "QZ-20260821-055",
  "gameId": "20260821LTOB02026",
  "teamCodes": ["LT", "OB"],
  "templateId": "RELATION_LINK",
  "question": "두산 정수빈과 박찬호의 공통점은?",
  "options": [
    { "id": "A", "text": "신인 드래프트 5라운드 지명 출신" },
    { "id": "B", "text": "나란히 FA로 이적해 옴" },
    { "id": "C", "text": "같은 고등학교 출신 유격수" },
    { "id": "D", "text": "둘 다 개막전 선발 포수 출신" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/64646.md#별명·밈",
    "quote": "정수빈과 함께 신인 드래프트 5라운드 지명 출신임을 가리키는 자조적 표현. 박찬호 본인이 인터뷰에서 \"우리는 천민이라 쟤네랑은 다르다\"고 정수빈과 장난친 적이 있다고 밝혔다" },
  "subject": { "scope": "PLAYER", "playerIds": [64646, 79231], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120
}
```

**좋은 이유**: evidence.quote 원문에는 선수 본인이 장난삼아 쓴 자조적 표현
("천민")이 그대로 담겨 있지만, **문항 표면(question·options)에는 그 단어를
전혀 노출하지 않고** "5라운드 지명 출신"이라는 중립적 사실로만 정답을 구성했다.
evidence는 원문 그대로(§2 원칙, 요약·창작 금지) 유지하면서도 사용자에게 노출되는
질문·보기 텍스트는 순화한다는 두 규칙(§2 evidence 원문 보존 vs §4 비하적 표현
회피)이 충돌하지 않고 동시에 지켜지는 방법을 보여준다 — evidence를 고치는 게
아니라 "무엇을 정답 보기 문구로 승격시킬지"만 신중히 고른 것.

## 60. RELATION_LINK (지식 · 경기 문항, PLAYER scope, EXPERT) — 2026-08-21 실행 사례

```json
{
  "quizId": "QZ-20260821-053",
  "gameId": "20260821SSNC02026",
  "teamCodes": ["SS", "NC"],
  "templateId": "RELATION_LINK",
  "question": "삼성 디아즈·페덱 듀오는 어떤 과거 조합에 비유되나?",
  "options": [
    { "id": "A", "text": "2014년 밴덴헐크+나바로 조합" },
    { "id": "B", "text": "2015년 라이온즈 왕조 시절 외국인 트리오" },
    { "id": "C", "text": "2011년 오릭스 버팔로즈 외국인 콤비" },
    { "id": "D", "text": "2018년 삼성 외국인 원투펀치" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/54400.md#별명·밈",
    "quote": "투수 페덱과 함께 2014년 밴덴헐크+나바로 조합(백인 투수+흑인 야수)에 빗대지는 밈" },
  "subject": { "scope": "PLAYER", "playerIds": [54400, 56459], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120
}
```

**좋은 이유**: 오답 4개 전부가 실제 존재했던 KBO 외국인 콤비/트리오 조합이라
피상적 야구 지식만으로는 소거법이 통하지 않는, EXPERT 난이도에 걸맞은 설계다.
evidence 원문에 있는 "(백인 투수+흑인 야수)"라는 인종 언급은 정답 보기·질문
어디에도 노출하지 않았다 — evidence는 원문 그대로 보존하되(§2), 사용자 노출
문구는 "2014년 밴덴헐크+나바로 조합"이라는 중립적 사실만 남긴 판단이 55번
사례와 같은 원칙이다.

## 61. MEME_OWNER (지식 · 공통 문항, LEAGUE scope, EASY) — 2026-08-21 실행 사례

```json
{
  "quizId": "QZ-20260821-020",
  "gameId": null,
  "teamCodes": [],
  "templateId": "MEME_OWNER",
  "question": "별명 '문살타'의 주인공은?",
  "options": [
    { "id": "A", "text": "문현빈" },
    { "id": "B", "text": "노시환" },
    { "id": "C", "text": "채은성" },
    { "id": "D", "text": "문동주" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/53764.md#별명밈",
    "quote": "- **문살타**: 성(문)과 병살타를 합친 말로, 병살타가 잦다는 데서 팬들이 붙인 별칭(커뮤니티 전언)[^ref7]." },
  "subject": { "scope": "LEAGUE", "playerIds": [], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30
}
```

**좋은 이유**: 오답 3명(노시환·채은성·문동주) 모두 한화 소속이라 "문"씨 성이
아닌 두 명(노시환·채은성)까지 섞여 있어 이름 패턴만으로 찍을 수 없게 만들었다
(문동주만 성이 같아 그럴듯한 함정). evidence가 별명의 유래(성+병살타 합성어)를
직접 설명하고 있어 COMMON-10 실패 사례(아래 bad.md 19번, "빈 인용")와 대비된다
— 좋은 MEME_OWNER는 "누가"뿐 아니라 "왜 그렇게 불리는지"까지 원문에서 뽑아낼
수 있어야 오답과 정답이 갈린다.

## 62. CAREER_PATH (지식 · 경기 문항, PLAYER scope, HARD) — 2026-08-22 실행 사례

```json
{
  "quizId": "QZ-20260822-001",
  "gameId": "20260822HTWO02026",
  "teamCodes": ["HT", "WO"],
  "templateId": "CAREER_PATH",
  "question": "하주석이 이번 시즌 트레이드로 새로 합류한 팀은?",
  "options": [
    { "id": "A", "text": "KIA 타이거즈" }, { "id": "B", "text": "롯데 자이언츠" },
    { "id": "C", "text": "키움 히어로즈" }, { "id": "D", "text": "NC 다이노스" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/62700.md#커리어 이력",
    "quote": "2026년 7월 말 한화 이글스에서 KIA 타이거즈로 트레이드 이적(상대는 투수 이형범)." },
  "subject": { "scope": "PLAYER", "playerIds": [62700], "teamCodes": [], "gameId": null },
  "difficulty": "HARD", "pointReward": 80
}
```

**좋은 이유**: bad.md 20번("FA 이적" 단정 사고)이 지적한 함정을 정확히 피했다
— 질문의 "트레이드로 새로 합류한 팀"이라는 인과 서술이 evidence.quote 안에
"트레이드 이적"으로 **그대로 명시**돼 있어, 질문이 원문에 없는 사유를 지어내지
않았다. 오답 3개(롯데·키움·NC)도 모두 실존 구단이라 "말도 안 되는 보기"로
답이 티나지 않는다. subject.playerIds=[62700](전제: 하주석)만 담고 정답인
KIA(teamCodes)는 비워 정답 유출 규칙(§11)도 지켰다.

## 63. MEME_ORIGIN (지식 · 경기 문항, PLAYER scope, EASY) — 2026-08-22 실행 사례

```json
{
  "quizId": "QZ-20260822-022",
  "gameId": "20260822SSNC02026",
  "teamCodes": ["SS", "NC"],
  "templateId": "MEME_ORIGIN",
  "question": "삼성 페덱의 별명 '멧돼지 사냥꾼'의 유래는?",
  "options": [
    { "id": "A", "text": "취미가 라이플로 멧돼지 사냥이라는 사실이 알려지면서" },
    { "id": "B", "text": "고향이 멧돼지가 많은 산간 지역이라서" },
    { "id": "C", "text": "타구 속도가 멧돼지처럼 빠르다는 비유" },
    { "id": "D", "text": "구단 마스코트가 멧돼지라서" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/56459.md#별명·밈",
    "quote": "취미가 라이플로 멧돼지 사냥이라는 사실이 알려지며, 동료 실책에도 \"(쏴도) 괜찮다\"는 식의 밈으로 소비된다." },
  "subject": { "scope": "PLAYER", "playerIds": [56459], "teamCodes": [], "gameId": null },
  "difficulty": "EASY", "pointReward": 30
}
```

**좋은 이유**: §4-1 고유성 검사를 명확히 통과한다 — "멧돼지 사냥꾼"은 이
선수 개인의 실제 취미(라이플 사냥)에서 나온 서사형 별명이라 "다른 잘하는
선수에게 옮겨 붙여도 자연스러운가?" 질문에 답이 "아니다"로 분명하다(같은
선수의 다른 후보 별명 "크리스 파닭"은 이름 발음을 비튼 말장난이라 재미는
있어도 유래 설명이 짧아 오답 설계가 어려웠고, 이쪽을 택함). 오답 3개는 취미·
출신지·타구 비유·마스코트로 각기 다른 각도의 그럴듯한 창작이라 정답을
곧바로 좁히기 어렵게 설계됐다.

## 64. RELATION_LINK (지식 · 경기 문항, PLAYER scope, EXPERT) — 2026-08-22 실행 사례, 최근 소진된 그래프 엣지 풀에서 새 조합을 찾은 사례

```json
{
  "quizId": "QZ-20260822-035",
  "gameId": "20260822LTOB02026",
  "teamCodes": ["LT", "OB"],
  "templateId": "RELATION_LINK",
  "question": "두산 박준순과 김대한의 공통점은?",
  "options": [
    { "id": "A", "text": "둘 다 '머갈툴순'으로 불리는 두산 영건 4인방에 속한다" },
    { "id": "B", "text": "둘 다 두산 프랜차이즈 원년 멤버다" },
    { "id": "C", "text": "둘 다 원래 투수였다가 타자로 전향했다" },
    { "id": "D", "text": "둘 다 해외 리그 진출 경력이 있다" }
  ],
  "answer": "A",
  "evidence": { "source": "wiki/players/55252.md#별명·밈",
    "quote": "두산 영건 야수 4인방(김대한·안재석·김민석·박준순)을 묶어 부르는 팬 밈. '순'이 박준순이다." },
  "subject": { "scope": "PLAYER", "playerIds": [55252, 69238], "teamCodes": [], "gameId": null },
  "difficulty": "EXPERT", "pointReward": 120
}
```

**좋은 이유**: 이번 실행에서 최근 7일 `quiz-candidates`를 대조해보니
`graph.json`의 허용 엣지(밈공유·커리어교차·라이벌, 총 40건) 중 오늘 경기
5묶음이 걸 수 있는 조합 대부분이 이미 최근(특히 전날 08-21) 실행에서 소진돼
있었다 — 김범수↔하주석(KIA), 천성호↔심우준(LG-한화), 디아즈↔페덱(삼성) 세
조합 모두 §2 중복으로 이번 실행에서 폐기됐다(사유는 아래 실행 요약 참고).
이 조합(박준순↔김대한, 두산 "머갈툴순" 4인방)만 최근 이력에 없어 살아남았다
— `scoring.yaml`이 이미 경고한 대로 EXPERT 슬롯(RELATION_LINK)의 재료 풀이
얇다는 것을 이번 실행에서도 재확인했다: 5경기 중 3경기(HT-WO, LG-HH,
SS-NC)는 대체 조합을 찾지 못해 이번 실행에 EXPERT 문항 없이 넘어갔다.



## 65. MEME_ORIGIN (HTWO 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-034",
  "gameId": "20260823HTWO02026",
  "teamCodes": [
    "HT",
    "WO"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "키움 데이비슨의 '맷 데이먼 혼동 밈'은 어디서 비롯됐나?",
  "options": [
    {
      "id": "A",
      "text": "배우 맷 데이먼이 방한 중 고척돔에서 데이비슨 유니폼을 입고 직관"
    },
    {
      "id": "B",
      "text": "데이비슨이 인터뷰에서 맷 데이먼을 닮고 싶다고 말함"
    },
    {
      "id": "C",
      "text": "구단이 팬 이벤트로 맷 데이먼 가면을 배포함"
    },
    {
      "id": "D",
      "text": "데이비슨의 SNS 프로필 사진이 맷 데이먼으로 바뀜"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/54944.md#별명-밈",
    "quote": "이름이 비슷한 할리우드 배우 맷 데이먼이 영화 홍보차 방한 중 고척돔에서 데이비슨의 유니폼을 입고 직관한 사실이 알려지며, \"데이비슨의 지인·친척·사촌\"이라는 소문과 함께 두 사람의 이름을 헷갈리는 밈이 커뮤니티에서 크게 확산됐다(커뮤니티 전언)"
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      54944
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 5점 — 배우 실제 방한 직관 사건이라는 구체적이고 웃긴 일화, 오답도 비슷한 형식이라 자연스러움

## 66. MEME_ORIGIN (HTWO 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-041",
  "gameId": "20260823HTWO02026",
  "teamCodes": [
    "HT",
    "WO"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "박정우의 별명 '쿠로미 주루사'는 어떤 장면에서 나왔나?",
  "options": [
    {
      "id": "A",
      "text": "쿠로미 캐릭터데이 유니폼을 입고 주루사를 당한 장면"
    },
    {
      "id": "B",
      "text": "쿠로미 인형을 라커룸에 두고 다닌 모습"
    },
    {
      "id": "C",
      "text": "쿠로미 응원봉을 흔드는 팬 반응"
    },
    {
      "id": "D",
      "text": "쿠로미 캐릭터와 표정이 닮았다는 반응"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/67609.md#별명-밈",
    "quote": "작년 쿠로미 캐릭터데이 유니폼을 입고 나온 경기에서 있었던 주루사 장면이 밈으로 종종 소환된다(커뮤니티 전언)"
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      67609
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 5점 — 캐릭터데이 유니폼+주루사라는 구체적 장면 결합, 다른 선수로 옮기면 성립 안 하는 고유 소재

## 67. MEME_ORIGIN (HTWO 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-036",
  "gameId": "20260823HTWO02026",
  "teamCodes": [
    "HT",
    "WO"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "박재현의 별명 '중재현'은 어떤 뜻에서 나왔나?",
  "options": [
    {
      "id": "A",
      "text": "중견수(中堅手)와 이름을 합친 표현"
    },
    {
      "id": "B",
      "text": "중요한 순간마다 활약해서 붙은 표현"
    },
    {
      "id": "C",
      "text": "중고참급 연차를 뜻하는 표현"
    },
    {
      "id": "D",
      "text": "중심타선에서 활약한다는 표현"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/55636.md#별명-밈",
    "quote": "중견수(中堅手)와 이름을 합친 표현으로, KIA 라인업에서 중견수로 선발 출전할 때 커뮤니티에서 반복적으로 쓰이는 별명이다(커뮤니티 전언)."
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      55636
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 5점 — 포지션(중견수)+이름 말장난이 짧고 명확, 즉답 가능한 쇼츠형 문제

## 68. MEME_ORIGIN (KTSK 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-037",
  "gameId": "20260823KTSK02026",
  "teamCodes": [
    "KT",
    "SK"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "힐리어드의 별명 '텍사스 남자'는 어떤 발언에서 비롯됐나?",
  "options": [
    {
      "id": "A",
      "text": "\"난 여름에 강한 텍사스 남자\"라는 발언"
    },
    {
      "id": "B",
      "text": "텍사스 레인저스 시절 별명을 그대로 가져와서"
    },
    {
      "id": "C",
      "text": "텍사스 출신 코치에게 지도받았다는 사실"
    },
    {
      "id": "D",
      "text": "고향이 텍사스라 밝힌 인터뷰"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/56034.md#별명-밈",
    "quote": "\"난 여름에 강한 텍사스 남자\"라고 말한 것이 더위를 힘들어하는 다른 선수들을 겨냥한 도발로 받아들여지며 붙은 표현."
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      56034
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 본인 어록("난 여름에 강한 텍사스 남자")이 그대로 유래라 고유성·재미 모두 확실

## 69. MEME_ORIGIN (KTSK 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-033",
  "gameId": "20260823KTSK02026",
  "teamCodes": [
    "KT",
    "SK"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "이로운의 별명 '이로운의 만루홈런'은 어떤 일화에서 비롯됐나?",
  "options": [
    {
      "id": "A",
      "text": "상대에게 결정적 홈런을 허용했던 일화"
    },
    {
      "id": "B",
      "text": "만루 상황에서 끝내기 홈런을 친 일화"
    },
    {
      "id": "C",
      "text": "데뷔전에서 만루홈런을 목격한 일화"
    },
    {
      "id": "D",
      "text": "동료의 만루홈런을 예언한 일화"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/53892.md#별명-밈",
    "quote": "그가 등판했을 때 상대에게 결정적 홈런을 허용했던 일화에서 비롯된 것으로 보이는 댓글 문구로, 이후 다른 선수의 홈런 장면에도 반복적으로 인용되는 놀림성 정형구가 되었다"
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      53892
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 특정 실점 일화에서 파생된 놀림성 정형구로, 이름 자체의 반어적 재미가 강함

## 70. CAREER_PATH (KTSK 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-020",
  "gameId": "20260823KTSK02026",
  "teamCodes": [
    "KT",
    "SK"
  ],
  "templateId": "CAREER_PATH",
  "question": "김성현이 트레이드로 SK 합류 전 소속됐던 팀은?",
  "options": [
    {
      "id": "A",
      "text": "넥센 히어로즈"
    },
    {
      "id": "B",
      "text": "한화 이글스"
    },
    {
      "id": "C",
      "text": "KIA 타이거즈"
    },
    {
      "id": "D",
      "text": "두산 베어스"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/76802.md#커리어-이력",
    "quote": "2011시즌 트레이드로 넥센 히어로즈에서 SK(現 SSG)로 이적했다(송신영과 함께 박병호·심수창·현금 15억과 교환)"
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      76802
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "HARD",
  "pointReward": 80
}
```

**좋은 이유**: 박병호가 얽힌 2011년 트레이드라 배경 스토리가 풍부해 흥미도가 높음

## 71. MEME_ORIGIN (LGHH 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-044",
  "gameId": "20260823LGHH02026",
  "teamCodes": [
    "LG",
    "HH"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "한화 채은성의 별명 '채버지'는 무엇을 합친 말인가?",
  "options": [
    {
      "id": "A",
      "text": "채은성 + 아버지"
    },
    {
      "id": "B",
      "text": "채은성 + 부자(富者)"
    },
    {
      "id": "C",
      "text": "채은성 + 대부(代父)"
    },
    {
      "id": "D",
      "text": "채은성 + 은퇴 후 지도자"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/79192.md#별명·밈",
    "quote": "- **채버지**: '채은성'과 '아버지'를 합친 애칭으로, 팬들이 그를 든든하게 여기는 마음을 담아 부르는 표현이다(커뮤니티 전언).[^ref6][^ref4]"
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      79192
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 5점 — '채은성+아버지'라는 명확한 어원이 원문에 그대로 있고, 오답도 '부자/대부/은퇴 후 지도자' 등 비슷한 격식의 가짜 유래라 소거법이 안 통함.

## 72. MEME_ORIGIN (LGHH 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-031",
  "gameId": "20260823LGHH02026",
  "teamCodes": [
    "LG",
    "HH"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "한화 허인서의 별명 '랄인서'는 어떻게 붙었나?",
  "options": [
    {
      "id": "A",
      "text": "MLB 매리너스 포수 칼 랄리와 비교되며 이름을 합쳐서"
    },
    {
      "id": "B",
      "text": "고교 시절 애칭이 '랄프'였던 데서"
    },
    {
      "id": "C",
      "text": "타격 자세가 독특해 붙은 별명이 줄어들어서"
    },
    {
      "id": "D",
      "text": "데뷔 시즌 등번호에서 착안해서"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/52764.md#별명·밈",
    "quote": "- **미국의 허인서 / 랄인서**: MLB 시애틀 매리너스의 포수 칼 랄리(Cal Raleigh)와 자주 비교되며 붙은 별명. 두 이름을 합친 \"랄인서\"라는 표현도 쓰인다(커뮤니티 전언)[^ref4][^ref5][^ref6]."
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      52764
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 5점 — MLB 포수 칼 랄리와의 실제 비교에서 나온 이름 합성이라는, 본인에게만 고유한 서사(§4-1 고유성 기준 명확히 통과).

## 73. CAREER_PATH (LGHH 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-008",
  "gameId": "20260823LGHH02026",
  "teamCodes": [
    "LG",
    "HH"
  ],
  "templateId": "CAREER_PATH",
  "question": "한화 이형범이 하주석과 맞트레이드되기 전 소속팀은?",
  "options": [
    {
      "id": "A",
      "text": "KIA 타이거즈"
    },
    {
      "id": "B",
      "text": "삼성 라이온즈"
    },
    {
      "id": "C",
      "text": "NC 다이노스"
    },
    {
      "id": "D",
      "text": "키움 히어로즈"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/62951.md#커리어-이력",
    "quote": "- 2026-07-27, 하주석과의 트레이드로 기아 타이거즈에서 한화 이글스로 이적(커뮤니티 전언)[^ref2]."
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      62951
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "HARD",
  "pointReward": 80
}
```

**좋은 이유**: 5점 — 트레이드 날짜·상대 선수(하주석)·구단명까지 evidence에 전부 명시돼 있어 근거가 탄탄하고, 오답 4개(삼성·NC·키움)도 모두 실존 구단이라 답이 티나지 않음.

## 74. MEME_ORIGIN (LTOB 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-040",
  "gameId": "20260823LTOB02026",
  "teamCodes": [
    "LT",
    "OB"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "김원중의 별명 '탭댄스 포크볼'이 붙은 이유는?",
  "options": [
    {
      "id": "A",
      "text": "포크볼을 던지기 전 특유의 투구 동작을 '탭댄스'로 표현해서"
    },
    {
      "id": "B",
      "text": "포크볼 그립을 잡을 때 특유의 스텝을 밟아서"
    },
    {
      "id": "C",
      "text": "세이브 후 마운드에서 춤추는 세리머니를 해서"
    },
    {
      "id": "D",
      "text": "포크볼 낙차에 타자들이 휘청이는 모습 때문에"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/62528.md#별명-밈",
    "quote": "포크볼을 던지기 전 보이는 특유의 투구 동작을 커뮤니티에서 '탭댄스'로 표현한다"
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      62528
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 투구 동작에서 비롯된 고유 표현, 다른 선수에게 옮겨 붙이기 부자연스러움

## 75. MEME_ORIGIN (LTOB 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-042",
  "gameId": "20260823LTOB02026",
  "teamCodes": [
    "LT",
    "OB"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "김대한의 밈 '리센느 저점매수'가 붙은 배경은?",
  "options": [
    {
      "id": "A",
      "text": "등장곡으로 써온 그룹 리센느의 화제성이 오르는 시점에 본인도 홈런을 몰아쳐서"
    },
    {
      "id": "B",
      "text": "리센느의 신곡 발매를 기념해 응원가를 바꿔서"
    },
    {
      "id": "C",
      "text": "리센느 팬미팅에 다녀온 뒤 타격감이 좋아져서"
    },
    {
      "id": "D",
      "text": "리센느 멤버와 사석에서 친분을 맺은 사실이 알려져서"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/69238.md#별명-밈",
    "quote": "최근 리센느의 화제성이 오르는 시점과 맞물려 본인도 홈런을 몰아치기 시작하면서"
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      69238
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 등장곡+우연한 타이밍이 겹친 구체적 일화, 고유성 높음

## 76. MEME_ORIGIN (LTOB 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-035",
  "gameId": "20260823LTOB02026",
  "teamCodes": [
    "LT",
    "OB"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "박준순의 밈 '4빵이나 5빵이나'는 어떤 상황에서 나온 말인가?",
  "options": [
    {
      "id": "A",
      "text": "역전 홈런 후 인터뷰에서 초구를 노려친 이유를 설명하며 한 말이다"
    },
    {
      "id": "B",
      "text": "4번과 5번 타순 중 어디가 편한지 묻는 질문에 답한 말이다"
    },
    {
      "id": "C",
      "text": "4연승과 5연승 중 무엇이 더 짜릿한지 묻는 질문에 답한 말이다"
    },
    {
      "id": "D",
      "text": "병살타 후 아쉬움을 표현하며 한 말이다"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/55252.md#별명-밈",
    "quote": "역전 홈런 후 인터뷰 \"뭐.. 4빵이나 5빵이나 비슷하니 그냥 초구에 돌렸다\"가 대담한 멘탈의 상징으로 회자된다"
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      55252
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 본인 어록에서 비롯된 밈, 고유성 확실

## 77. MEME_ORIGIN (SSNC 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-039",
  "gameId": "20260823SSNC02026",
  "teamCodes": [
    "SS",
    "NC"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "구자욱의 상징 문구 '푸른 파도'는 어디서 비롯됐나?",
  "options": [
    {
      "id": "A",
      "text": "본인이 남긴 어록에서 비롯됐다"
    },
    {
      "id": "B",
      "text": "구단 응원가 가사에서 따왔다"
    },
    {
      "id": "C",
      "text": "홈구장 이름에서 유래했다"
    },
    {
      "id": "D",
      "text": "팬클럽 명칭에서 따왔다"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/62404.md#별명-밈",
    "quote": "**\"푸른 파도\" 어록**: \"나의 야구가 끝나는 날까지 야구장에 푸른 파도가 멈추지 않도록\"이라는 본인 어록이 팬들 사이에서 상징 문구로 쓰인다."
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      62404
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 본인이 직접 남긴 어록이 팬 상징 문구로 굳어진 서사형 소재라 §4-1 고유성 판정이 명확하고, 감성적 임팩트도 있음(fun 5).

## 78. CAREER_PATH (SSNC 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-018",
  "gameId": "20260823SSNC02026",
  "teamCodes": [
    "SS",
    "NC"
  ],
  "templateId": "CAREER_PATH",
  "question": "최형우가 삼성 합류 전 소속됐던 팀은?",
  "options": [
    {
      "id": "A",
      "text": "KIA 타이거즈"
    },
    {
      "id": "B",
      "text": "롯데 자이언츠"
    },
    {
      "id": "C",
      "text": "두산 베어스"
    },
    {
      "id": "D",
      "text": "SSG 랜더스"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/72443.md#커리어-이력",
    "quote": "과거 소속이었던 기아를 상대하는 경기에서 팬들 사이에 \"나를 버린 팀에 복수한다\"는 반응이 나오는 등, 기아 소속이었다가 삼성으로 이적한 이력이 언급된다(커뮤니티 전언)."
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      72443
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "HARD",
  "pointReward": 80
}
```

**좋은 이유**: evidence에 '나를 버린 팀에 복수한다'는 팬 반응까지 함께 인용돼 서사가 풍부하고, 오답 3개도 모두 실존 구단이라 소거법이 안 통함(fun 5).

## 79. MEME_ORIGIN (SSNC 경기 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-045",
  "gameId": "20260823SSNC02026",
  "teamCodes": [
    "SS",
    "NC"
  ],
  "templateId": "MEME_ORIGIN",
  "question": "박건우 별명 '고라니'는 어디서 비롯됐나?",
  "options": [
    {
      "id": "A",
      "text": "두산 시절부터의 호타준족 스타일과 빠른 주력을 빗댄 것"
    },
    {
      "id": "B",
      "text": "고향이 강원도 산간 지역이라서"
    },
    {
      "id": "C",
      "text": "타격 자세가 고라니를 닮았다는 우스갯소리"
    },
    {
      "id": "D",
      "text": "구단 마스코트와 관련된 별명"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/79215.md#별명-밈",
    "quote": "**고라니**: 두산 시절부터 호타준족 스타일과 빠른 주력을 빗대 붙여진 별명이다(커뮤니티 전언)."
  },
  "subject": {
    "scope": "PLAYER",
    "playerIds": [
      79215
    ],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 호타준족 스타일이라는 유래가 원문에 명시돼 있어 '왜 그렇게 불리는지'까지 답할 수 있는 좋은 MEME_ORIGIN 소재(fun 5).

## 80. MEME_OWNER (공통 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-047",
  "gameId": null,
  "teamCodes": [],
  "templateId": "MEME_OWNER",
  "question": "오재일이 '메시'라 부른 데서 유래한 별명 '라이온즈 메시'의 주인공은?",
  "options": [
    {
      "id": "A",
      "text": "김지찬"
    },
    {
      "id": "B",
      "text": "김성윤"
    },
    {
      "id": "C",
      "text": "구자욱"
    },
    {
      "id": "D",
      "text": "김영웅"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/50458.md#별명-밈",
    "quote": "- **라이온즈 메시**: 오재일이 방송에서 \"김지찬 축구 잘한다, 메시다\"라고 언급한 데서 비롯된 별명. 작은 체구와 빠른 방향 전환 능력이 이유로 꼽힌다(커뮤니티 전언).[^ref1]"
  },
  "subject": {
    "scope": "LEAGUE",
    "playerIds": [],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 고유한 유래(구체적 사건·발언)가 있는 밈이라 소거법 없이 즉답 가능

## 81. MEME_OWNER (공통 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-053",
  "gameId": null,
  "teamCodes": [],
  "templateId": "MEME_OWNER",
  "question": "등번호를 '저점매수'에 빗댄 별명 '혁삐코인'의 주인공인 키움 선수는?",
  "options": [
    {
      "id": "A",
      "text": "권혁빈"
    },
    {
      "id": "B",
      "text": "김건희"
    },
    {
      "id": "C",
      "text": "임지열"
    },
    {
      "id": "D",
      "text": "데이비슨"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/55371.md#별명-밈",
    "quote": "- **혁삐코인**: 앞으로 더 성장할 것이라는 기대를 담아 지금 유니폼(등번호 00번)을 미리 사두는 것을 \"저점매수\"에 빗댄 팬들의 표현[^ref3]."
  },
  "subject": {
    "scope": "LEAGUE",
    "playerIds": [],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 고유한 유래(구체적 사건·발언)가 있는 밈이라 소거법 없이 즉답 가능

## 82. MEME_OWNER (공통 묶음) — 2026-08-23 실행 사례

```json
{
  "quizId": "QZ-20260823-052",
  "gameId": null,
  "teamCodes": [],
  "templateId": "MEME_OWNER",
  "question": "배우 맷 데이먼이 고척돔서 직관해 화제가 된 키움 선수는?",
  "options": [
    {
      "id": "A",
      "text": "데이비슨"
    },
    {
      "id": "B",
      "text": "로젠버그"
    },
    {
      "id": "C",
      "text": "알칸타라"
    },
    {
      "id": "D",
      "text": "서건창"
    }
  ],
  "answer": "A",
  "evidence": {
    "source": "wiki/players/54944.md#별명-밈",
    "quote": "- **맷 데이먼 혼동 밈**: 이름이 비슷한 할리우드 배우 맷 데이먼이 영화 홍보차 방한 중 고척돔에서 데이비슨의 유니폼을 입고 직관한 사실이 알려지며, \"데이비슨의 지인·친척·사촌\"이라는 소문과 함께 두 사람의 이름을 헷갈리는 밈이 커뮤니티에서 크게 확산됐다(커뮤니티 전언)[^ref7]."
  },
  "subject": {
    "scope": "LEAGUE",
    "playerIds": [],
    "teamCodes": [],
    "gameId": null
  },
  "difficulty": "EASY",
  "pointReward": 30
}
```

**좋은 이유**: 고유한 유래(구체적 사건·발언)가 있는 밈이라 소거법 없이 즉답 가능
