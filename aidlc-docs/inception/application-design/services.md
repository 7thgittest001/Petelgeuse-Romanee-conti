# Service Definitions: 走った風になるアプリ

## Service Architecture

```
+-------------------+     +-------------------+     +-------------------+
| Route Service     |     | StreetView Service|     | Message Service   |
| (ルート計算)       |     | (画像取得)         |     | (メッセージ管理)   |
+-------------------+     +-------------------+     +-------------------+
        |                         |                         |
        v                         v                         v
+-------------------+     +-------------------+     +-------------------+
| Backend API       |     | Backend API       |     | Local Data        |
| /api/directions   |     | /api/streetview   |     | (messages.json)   |
+-------------------+     +-------------------+     +-------------------+
```

---

## Frontend Services

### RouteService
- **責務**: ルート計算のオーケストレーション
- **フロー**:
  1. ユーザーがスタート/ゴールを指定
  2. Backend API（/api/directions）を呼び出し
  3. レスポンスからルートポイントを抽出
  4. Street View取得用の等間隔ポイントを計算
  5. ルート情報（距離、時間）をUIに提供

### StreetViewService
- **責務**: Street View画像の取得・先読み・キャッシュ管理
- **フロー**:
  1. ルートポイントリストを受け取る
  2. 現在位置から数枚先の画像を先読み（prefetch）
  3. 進行に合わせて次の画像を提供
  4. 取得済み画像をメモリキャッシュ
  5. 画像が取得できないポイントはスキップ

### RunningService
- **責務**: ランニング体験全体のオーケストレーション
- **フロー**:
  1. ルート情報を受け取りランニング開始
  2. タイマーに基づき進行（ペース: 6分/km相当）
  3. 各フレームで: 現在位置更新 → 画像切替 → カウンター更新 → メッセージ判定
  4. 一時停止/再開/リタイアの制御
  5. ゴール到達で完走処理

### MessageService
- **責務**: 共犯者トーンメッセージの管理・選択
- **フロー**:
  1. メッセージプール（JSON）からランダム選択
  2. 表示タイミングの判定（距離ベース: 1kmごと等）
  3. メッセージ種別の判定（開始/走行中/完走/マイルストーン）
  4. 重複回避（同じメッセージを連続で出さない）

---

## Backend Services

### DirectionsProxyService
- **責務**: Google Directions APIへのプロキシ
- **フロー**:
  1. フロントエンドからリクエスト受信
  2. APIキーを付与してGoogle Directions APIに転送
  3. レスポンスをそのまま返却
- **追加処理（将来）**: レスポンスキャッシュ、レート制限

### StreetViewProxyService
- **責務**: Google Street View Static APIへのプロキシ
- **フロー**:
  1. フロントエンドから座標・方向・サイズを受信
  2. APIキーを付与してStreet View Static APIに転送
  3. 画像URL（署名付き）を返却
- **追加処理（将来）**: レート制限、画像キャッシュ

---

## Service Interaction Patterns

### ランニング開始フロー
```
User → TopPage → RouteService → Backend(/api/directions) → Google Directions API
                      ↓
              Route情報確定
                      ↓
User → 「走る」ボタン → RunningPage
                      ↓
              RunningService.start()
                      ↓
         StreetViewService.prefetch() → Backend(/api/streetview) → Google SV API
                      ↓
         画像表示開始 + カウンター開始 + メッセージ表示
```

### ランニング中フロー（毎フレーム）
```
Timer tick
  → RunningService: 現在位置を進める
  → StreetViewService: 次の画像を提供
  → RunningStats: 距離/カロリー/時間を更新
  → MessageService: メッセージ表示判定
  → UI更新
```

### 完走フロー
```
RunningService: ゴール到達検知
  → RunningStats: 最終結果を確定
  → MessageService: 完走メッセージ選択
  → CompletionPage へ遷移
  → CompletionEffects: 演出表示
  → CompletionResult: 結果表示
```
