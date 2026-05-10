# Application Design: 走った風になるアプリ（統合ドキュメント）

## 1. 技術スタック

| Layer | Technology | Hosting | Cost (MVP) |
|---|---|---|---|
| Frontend | React + TypeScript + Vite | S3 + CloudFront | ~0円/月 |
| State Management | useState / useContext | — | — |
| Backend | Python + FastAPI | AWS Lambda + API Gateway | ~0円/月 |
| Maps (Client) | Google Maps JavaScript API | Direct (referer restricted) | 無料枠内 |
| Maps (Server) | Directions + Street View Static API | Via Lambda proxy | 無料枠内 |

---

## 2. アーキテクチャ概要

```
+--------------------------------------------------+
|              Frontend (React + Vite)               |
|              S3 + CloudFront                       |
+--------------------------------------------------+
|  pages/          | components/  | hooks/          |
|  ├── TopPage     | └── ui/     | └── useRoute... |
|  ├── RunningPage |             |                  |
|  └── Completion  | contexts/   | services/        |
|                  | └── Running | └── mapsApi     |
+--------------------------------------------------+
          |                            |
          | Maps JS API (direct)       | REST API (GET)
          | (referer restricted)       |
          v                            v
+------------------+    +---------------------------+
| Google Maps JS   |    | API Gateway + Lambda      |
| (地図表示)        |    | (FastAPI)                 |
+------------------+    +---------------------------+
                                     |
                                     v
                        +---------------------------+
                        | Google Maps Platform      |
                        | - Directions API          |
                        | - Street View Static API  |
                        +---------------------------+
```

---

## 3. コンポーネント構成

### ページ構成（3ページ）
1. **TopPage** — コース選択（地図 + スタート/ゴール指定 + ルート確認）
2. **RunningPage** — バーチャルランニング体験（Street View + カウンター + ミニマップ）
3. **CompletionPage** — 完走画面（結果 + 称賛メッセージ + 演出）

### 機能コンポーネント
- MapSelector, RouteInfo, StreetViewViewer, MiniMap, RunningCounters, MessageDisplay, RunningControls, CompletionResult, CompletionEffects

### ロジック層（Custom Hooks）
- useRouteCalculation, useStreetViewImages, useRunningStats, useMessages

### API通信層
- mapsApiService（Backend APIとの通信）

### バックエンド（FastAPI on Lambda）
- GET /api/directions — ルート計算プロキシ
- GET /api/streetview — Street View画像プロキシ
- GET /api/health — ヘルスチェック

---

## 4. APIキー保護方式

| API | 方式 | 保護 |
|---|---|---|
| Maps JavaScript API | クライアント直接 | リファラー制限（ドメイン限定） |
| Directions API | Lambda経由 | サーバー内にキー保管、IP制限 |
| Street View Static API | Lambda経由 | サーバー内にキー保管、IP制限 |

APIキーは2つ用意：
1. **ブラウザ用**: Maps JS APIのみ許可、リファラー制限
2. **サーバー用**: Directions + Street View のみ許可、Lambda IP制限

---

## 5. 状態管理

- **MVP**: React標準（useState + useContext）
- **RunningContext**: ランニング状態を全コンポーネントに共有
- **将来**: 状態が複雑化した場合にZustand等を導入

---

## 6. フォルダ構造

```
src/
├── pages/
│   ├── TopPage/
│   │   ├── index.tsx
│   │   └── _components/
│   ├── RunningPage/
│   │   ├── index.tsx
│   │   ├── _components/
│   │   └── _hooks/
│   └── CompletionPage/
│       ├── index.tsx
│       └── _components/
├── components/ui/
├── contexts/
├── hooks/
├── services/
├── data/
├── types/
├── lib/
└── App.tsx
```

---

## 7. 主要データフロー

### コース選択 → ランニング開始
```
User: スタート/ゴール選択
  → MapSelector → RouteService → Backend(/api/directions)
  → Route確定 → RouteInfo表示
  → User: 「走る」ボタン
  → RunningPage遷移 → RunningService.start()
```

### ランニング中（毎フレーム）
```
Timer → RunningService
  → 位置更新 → StreetViewViewer（画像切替）
  → 統計更新 → RunningCounters（数値更新）
  → 位置更新 → MiniMap（マーカー移動）
  → メッセージ判定 → MessageDisplay（表示/非表示）
```

### 完走
```
RunningService: ゴール到達
  → 結果確定 → CompletionPage遷移
  → CompletionEffects（演出）
  → CompletionResult（結果表示）
  → MessageService（称賛メッセージ）
```

---

## 8. 設計判断の根拠

| 判断 | 根拠 |
|---|---|
| React選択 | チーム開発◎、Google Maps連携ライブラリ充実、将来のモバイル化にReact Native転用可能 |
| FastAPI選択 | 将来のAI連携・動画生成にPythonが有利、MVPではコード量が少なく言語2つの負担は最小限 |
| ハイブリッド構成 | 機能単位コンポーネント + レイヤー分離で、チーム分担しやすく将来の拡張にも対応 |
| useState/useContext | MVPの規模では外部ライブラリ不要。将来的に必要になれば導入 |
| APIキーハイブリッド | Maps JS APIはクライアント専用（技術的制約）、課金対象APIはサーバー経由で保護 |
| AWS Lambda | 月額ほぼ0円、管理不要、スケール自動 |
