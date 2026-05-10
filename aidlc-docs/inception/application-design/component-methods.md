# Component Methods: 走った風になるアプリ

## Frontend Hooks（ロジック層）

### useRouteCalculation
- **配置**: `src/hooks/useRouteCalculation.ts`
- **責務**: ルート計算ロジック
- **メソッド**:
  - `calculateRoute(origin: LatLng, destination: LatLng): Promise<Route>` — 2地点間のルートを計算
  - `getRoutePoints(route: Route, intervalMeters: number): LatLng[]` — ルート上の等間隔ポイントを取得
  - `getRouteDistance(): number` — ルート総距離（km）
  - `getEstimatedTime(paceMinPerKm: number): number` — 推定所要時間（分）

### useStreetViewImages
- **配置**: `src/pages/RunningPage/_hooks/useStreetViewImages.ts`
- **責務**: Street View画像の取得・管理
- **メソッド**:
  - `prefetchImages(points: LatLng[], count: number): void` — 先読み画像取得
  - `getCurrentImage(): string` — 現在表示すべき画像URL
  - `getNextImage(): string` — 次の画像URL
  - `getHeading(from: LatLng, to: LatLng): number` — 進行方向の角度計算

### useRunningStats
- **配置**: `src/pages/RunningPage/_hooks/useRunningStats.ts`
- **責務**: ランニング統計のリアルタイム計算
- **メソッド**:
  - `startRunning(): void` — ランニング開始
  - `pauseRunning(): void` — 一時停止
  - `resumeRunning(): void` — 再開
  - `stopRunning(): RunningResult` — 終了・結果返却
  - `getCurrentDistance(): number` — 現在走行距離（km）
  - `getCurrentCalories(): number` — 現在消費カロリー（kcal）
  - `getElapsedTime(): number` — 経過時間（秒）
  - `getCurrentPosition(): LatLng` — 現在位置
  - `getProgress(): number` — 進捗率（0〜1）

### useMessages
- **配置**: `src/pages/RunningPage/_hooks/useMessages.ts`
- **責務**: 共犯者トーンメッセージの選択・表示タイミング管理
- **メソッド**:
  - `getStartMessage(): string` — 開始時メッセージ
  - `getRunningMessage(distance: number): string | null` — 走行中メッセージ（一定距離ごと）
  - `getCompletionMessage(result: RunningResult): string` — 完走時メッセージ
  - `getMilestoneMessage(totalDistance: number): string | null` — マイルストーン達成メッセージ

---

## Frontend Services（API通信層）

### mapsApiService
- **配置**: `src/services/mapsApi.ts`
- **責務**: バックエンドAPIとの通信
- **メソッド**:
  - `fetchDirections(origin: LatLng, destination: LatLng): Promise<DirectionsResult>` — ルート計算API呼び出し
  - `fetchStreetViewImageUrl(location: LatLng, heading: number, size: string): Promise<string>` — Street View画像URL取得

---

## Backend API Endpoints

### GET /api/directions
- **入力**: `origin` (string), `destination` (string)
- **出力**: `DirectionsResult` (ルート情報、ポリライン、距離、所要時間)
- **処理**: Google Directions APIへプロキシ、結果をそのまま返却

### GET /api/streetview
- **入力**: `lat` (float), `lng` (float), `heading` (float), `size` (string, default "600x400")
- **出力**: 画像バイナリ or 署名付き画像URL
- **処理**: Google Street View Static APIへプロキシ

### GET /api/health
- **入力**: なし
- **出力**: `{"status": "ok"}`
- **処理**: ヘルスチェック

---

## Types（型定義）

### 配置: `src/types/`

```typescript
interface LatLng {
  lat: number;
  lng: number;
}

interface Route {
  points: LatLng[];
  distance: number;      // km
  duration: number;      // minutes
  polyline: string;      // encoded polyline
}

interface RunningResult {
  distance: number;      // km
  calories: number;      // kcal
  elapsedTime: number;   // seconds
  route: Route;
  completedAt: string;   // ISO timestamp
}

interface RunningState {
  status: 'idle' | 'running' | 'paused' | 'completed';
  currentPointIndex: number;
  startTime: number | null;
  pausedTime: number;
}
```

**Note**: 詳細なビジネスロジック（カロリー計算式、メッセージ選択アルゴリズム等）はFunctional Design（CONSTRUCTION phase）で定義します。
