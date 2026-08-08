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
