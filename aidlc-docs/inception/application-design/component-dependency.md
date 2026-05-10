# Component Dependencies: 走った風になるアプリ

## Dependency Matrix

| Component | Depends On | Depended By |
|---|---|---|
| **TopPage** | MapSelector, RouteInfo, RouteService | — |
| **RunningPage** | StreetViewViewer, MiniMap, RunningCounters, MessageDisplay, RunningControls, RunningService | — |
| **CompletionPage** | CompletionResult, CompletionEffects, MessageService | — |
| **MapSelector** | Google Maps JS API, RouteService | TopPage |
| **RouteInfo** | RouteService (route data) | TopPage |
| **StreetViewViewer** | StreetViewService, useStreetViewImages | RunningPage |
| **MiniMap** | Google Maps JS API, useRunningStats (position) | RunningPage |
| **RunningCounters** | useRunningStats | RunningPage |
| **MessageDisplay** | useMessages | RunningPage |
| **RunningControls** | useRunningStats (pause/resume/stop) | RunningPage |
| **CompletionResult** | RunningResult (data) | CompletionPage |
| **CompletionEffects** | — (self-contained) | CompletionPage |
| **RouteService** | mapsApiService | TopPage, RunningPage |
| **StreetViewService** | mapsApiService | RunningPage |
| **RunningService** | StreetViewService, useRunningStats, useMessages | RunningPage |
| **MessageService** | messages data (JSON) | RunningPage, CompletionPage |
| **mapsApiService** | Backend API | RouteService, StreetViewService |
| **Backend API** | Google Maps Platform APIs | mapsApiService |

---

## Communication Patterns

### Frontend Internal Communication

```
                    React Context (RunningState)
                              |
              +---------------+---------------+
              |               |               |
    StreetViewViewer    RunningCounters    MiniMap
              |               |               |
              +-------+-------+-------+-------+
                      |               |
                useRunningStats   useMessages
                      |
                RunningService (orchestrator)
```

**パターン**: React Context + Custom Hooks
- `RunningContext`: ランニング状態（status, currentPosition, stats）を全コンポーネントに共有
- 各コンポーネントは必要なデータだけをContextから取得
- ロジックはCustom Hooksに集約

### Frontend → Backend Communication

```
Frontend (React)
    |
    | HTTP GET (REST)
    v
API Gateway → Lambda (FastAPI)
    |
    | HTTP GET
    v
Google Maps Platform
```

**パターン**: REST API（GET only for MVP）
- 全通信はHTTP GET
- レスポンスはJSON
- エラー時は適切なHTTPステータスコード

### Data Flow

```
[User Input]
    ↓ スタート/ゴール選択
[RouteService]
    ↓ ルート計算結果
[RunningContext] ← 状態の中心
    ↓ 状態配信
[各コンポーネント] ← 必要なデータだけ取得
    ↓ UI更新
[User Display]
```

---

## Folder Structure

```
src/
├── pages/
│   ├── TopPage/
│   │   ├── index.tsx
│   │   └── _components/
│   │       ├── MapSelector.tsx
│   │       └── RouteInfo.tsx
│   ├── RunningPage/
│   │   ├── index.tsx
│   │   ├── _components/
│   │   │   ├── StreetViewViewer.tsx
│   │   │   ├── MiniMap.tsx
│   │   │   ├── RunningCounters.tsx
│   │   │   ├── MessageDisplay.tsx
│   │   │   └── RunningControls.tsx
│   │   └── _hooks/
│   │       ├── useStreetViewImages.ts
│   │       ├── useRunningStats.ts
│   │       └── useMessages.ts
│   └── CompletionPage/
│       ├── index.tsx
│       └── _components/
│           ├── CompletionResult.tsx
│           └── CompletionEffects.tsx
├── components/
│   └── ui/
│       ├── Button/
│       ├── Counter/
│       └── Toast/
├── contexts/
│   └── RunningContext.tsx
├── hooks/
│   └── useRouteCalculation.ts
├── services/
│   └── mapsApi.ts
├── data/
│   └── messages.json
├── types/
│   └── index.ts
├── lib/
│   └── calorieCalculator.ts
└── App.tsx
```

---

## Technology Stack Summary

| Layer | Technology | Hosting |
|---|---|---|
| Frontend | React + TypeScript + Vite | S3 + CloudFront |
| State Management | useState / useContext (MVP) | — |
| Backend | Python + FastAPI | AWS Lambda + API Gateway |
| Maps (Client) | Google Maps JavaScript API | Direct (referer restricted) |
| Maps (Server) | Directions API + Street View Static API | Via Lambda proxy |
| Infrastructure | AWS (S3, CloudFront, Lambda, API Gateway) | — |
