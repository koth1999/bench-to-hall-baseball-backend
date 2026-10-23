# 벤치에서 명전까지 — API 기본 계약

프론트엔드의 `dist/game.js`는 개발 단계에서 `localStorage`를 쓰며, 아래 API로 대체할 수 있게 상태 저장 계층을 분리했다.

## 리소스

`Career`는 한 선수의 진행 상태다. 최소 필드는 다음과 같다.

```json
{
  "id": "career_123",
  "playerName": "김주원",
  "highSchool": "서울고등학교",
  "position": "SP",
  "careerRoute": "draft",
  "season": 2031,
  "week": 12,
  "energy": 80,
  "hallProgress": 18,
  "attributes": { "power": 76, "control": 68, "stamina": 74, "mental": 71 },
  "stats": { "wins": 6, "losses": 3, "era": 3.14, "strikeouts": 84, "war": 1.8 }
}
```

## 초기 엔드포인트

| Method | Path | 역할 |
| --- | --- | --- |
| `GET` | `/api/careers/:careerId` | 저장된 커리어 불러오기 |
| `PUT` | `/api/careers/:careerId` | 능력치·기록·명전 점수 저장 |
| `POST` | `/api/careers/:careerId/week-action` | 훈련·휴식·멘토링 적용 |
| `POST` | `/api/careers/:careerId/games` | 등판 결과 처리와 기록 반영 |
| `POST` | `/api/careers/:careerId/path-choice` | 고졸 드래프트·대학 진학·MLB 마이너 계약 진로 확정 |

서버에서는 사용자 인증 뒤 `careerId` 소유권을 확인하고, 게임 결과와 능력치 변화는 서버에서 계산한다. 클라이언트가 최종 수치를 신뢰하지 않도록 한다.

`careerRoute`는 `draft`, `college`, `minor` 중 하나다. 이 값으로 이후 드래프트, 포스팅 시스템, MLB FA 협상 이벤트의 해금 조건과 로드맵을 결정한다.
