# Component Definitions: 走った風になるアプリ

## Architecture Overview

```
+--------------------------------------------------+
|                  Frontend (React)                  |
|  S3 + CloudFront                                  |
|                                                   |
|  pages/ ─── components/ ─── hooks/ ─── services/  |
+--------------------------------------------------+
          |                          |
          | Maps JS API (direct)     | REST API
          v                          v
+------------------+    +---------------------------+
| Google Maps      |    | Backend (FastAPI + Lambda) |
| JavaScript API   |    |                           |
| (地図表示)        |    | /api/directions           |
+------------------+    | /api/streetview           |
                        +---------------------------+
                                     |
                                     v
                        +---------------------------+
                        | Google Maps Platform      |
                        | - Directions API          |
                        | - Street View Static API  |
                        +---------------------------+
```

---

## Frontend Components

### 1. Pages（ページコンポーネント）

#### TopPage
- **責務**: コース選択画面。スタート/ゴール地点の指定とルート確認
- **配置**: `src/pages/TopPage/`

#### RunningPage
- **責務**: バーチャルランニング体験画面。Street View表示 + カウンター + ミニマップ
- **配置**: `src/pages/RunningPage/`

#### CompletionPage
- **責務**: 完走画面。結果表示 + 称賛メッセージ + 演出
- **配置**: `src/pages/CompletionPage/`

---

### 2. Feature Components（機能コンポーネント）

#### MapSelector
- **責務**: 地図表示、スタート/ゴール地点のマーカー設置、ルートライン描画
- **配置**: `src/pages/TopPage/_components/MapSelector.tsx`
- **依存**: Google Maps JavaScript API

#### RouteInfo
- **責務**: ルート情報表示（推定距離、推定時間）
- **配置**: `src/pages/TopPage/_components/RouteInfo.tsx`

#### StreetViewViewer
- **責務**: Street View画像の連続表示、トランジション処理
- **配置**: `src/pages/RunningPage/_components/StreetViewViewer.tsx`
- **依存**: Backend API（/api/streetview）

#### MiniMap
- **責務**: ルート上の現在位置をリアルタイム表示
- **配置**: `src/pages/RunningPage/_components/MiniMap.tsx`
- **依存**: Google Maps JavaScript API

#### RunningCounters
- **責務**: 距離・カロリー・時間カウンターのリアルタイム更新表示
- **配置**: `src/pages/RunningPage/_components/RunningCounters.tsx`

#### MessageDisplay
- **責務**: 共犯者トーンのメッセージ表示（トースト通知形式）
- **配置**: `src/pages/RunningPage/_components/MessageDisplay.tsx`

#### RunningControls
- **責務**: 一時停止/再開/リタイアボタン
- **配置**: `src/pages/RunningPage/_components/RunningControls.tsx`

#### CompletionResult
- **責務**: 最終結果サマリー（距離、カロリー、時間）+ 称賛メッセージ
- **配置**: `src/pages/CompletionPage/_components/CompletionResult.tsx`

#### CompletionEffects
- **責務**: 完走時の視覚演出（紙吹雪等）
- **配置**: `src/pages/CompletionPage/_components/CompletionEffects.tsx`

---

### 3. Shared UI Components（共通UIコンポーネント）

#### Button
- **責務**: 汎用ボタン（「走る」「もう一本」等）
- **配置**: `src/components/ui/Button/`

#### Counter
- **責務**: アニメーション付き数値カウンター
- **配置**: `src/components/ui/Counter/`

#### Toast
- **責務**: トースト通知（メッセージ表示用）
- **配置**: `src/components/ui/Toast/`

---

## Backend Components

### 4. FastAPI Application（AWS Lambda）

#### DirectionsProxy
- **責務**: Google Directions APIへのプロキシ。ルート計算結果の中継
- **エンドポイント**: `GET /api/directions`

#### StreetViewProxy
- **責務**: Google Street View Static APIへのプロキシ。画像URLの生成・中継
- **エンドポイント**: `GET /api/streetview`

#### HealthCheck
- **責務**: ヘルスチェック用エンドポイント
- **エンドポイント**: `GET /api/health`
