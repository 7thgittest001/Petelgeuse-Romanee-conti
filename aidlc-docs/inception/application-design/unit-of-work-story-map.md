# Unit of Work Story Map: 走った風になるアプリ

## Story → Module Mapping

単一ユニット（モノレポ）のため、全ストーリーは同一ユニット内の各モジュールにマッピングされます。

---

## MVP Stories (Phase 1)

| Story | Frontend Module | Backend Module | 開発Phase |
|---|---|---|---|
| US-001: スタート地点の指定 | TopPage/MapSelector | — | Phase B |
| US-002: ゴール地点の指定 | TopPage/MapSelector, RouteInfo | /api/directions | Phase A→B |
| US-003: ルートの確認 | TopPage/MapSelector, RouteInfo | /api/directions | Phase A→B |
| US-004: バーチャルランニングの開始 | RunningPage | /api/streetview | Phase A→B |
| US-005: 走行中の体験 | RunningPage/StreetViewViewer | /api/streetview | Phase A→B |
| US-006: 現在位置のリアルタイム表示 | RunningPage/MiniMap | — | Phase B |
| US-007: 走行距離カウンター | RunningPage/RunningCounters | — | Phase B |
| US-008: 消費カロリー表示 | RunningPage/RunningCounters | — | Phase B |
| US-009: 完走時の演出 | CompletionPage | — | Phase B |
| US-010: 共犯者トーンのメッセージ体験 | RunningPage/MessageDisplay | — | Phase B |
| US-011: 過剰な称賛演出 | CompletionPage/CompletionResult | — | Phase B |

## Phase 2 Stories

| Story | Frontend Module | Backend Module | 開発Phase |
|---|---|---|---|
| US-012: 動画ファイルのダウンロード | CompletionPage (DLボタン) | /api/video-generate (新規) | Future |
| US-013: 手振りアニメーション | RunningPage (overlay) | — | Future |
| US-014: マップ上のコース履歴表示 | 新規HistoryPage | — (localStorage) | Future |

---

## 開発Phase別ストーリー割り当て

### Phase A: バックエンド先行（1〜2日）

| 優先度 | 対象ストーリー | 実装内容 |
|---|---|---|
| 1 | US-002, US-003 | GET /api/directions（ルート計算プロキシ） |
| 2 | US-004, US-005 | GET /api/streetview（画像取得プロキシ） |
| 3 | — | GET /api/health（ヘルスチェック） |

### Phase B: フロントエンド（3〜5日）

| 優先度 | 対象ストーリー | 実装内容 |
|---|---|---|
| 1 | US-001, US-002, US-003 | TopPage（地図 + コース選択） |
| 2 | US-004, US-005 | RunningPage/StreetViewViewer（画像連続表示） |
| 3 | US-006 | RunningPage/MiniMap（現在位置表示） |
| 4 | US-007, US-008 | RunningPage/RunningCounters（カウンター） |
| 5 | US-010 | RunningPage/MessageDisplay（共犯者メッセージ） |
| 6 | US-009, US-011 | CompletionPage（完走演出 + 称賛） |

### Phase C: 統合・デプロイ（1日）

| 優先度 | 対象 | 実装内容 |
|---|---|---|
| 1 | 全ストーリー | Frontend ↔ Backend 接続テスト |
| 2 | 全ストーリー | S3 + CloudFront デプロイ |
| 3 | 全ストーリー | E2E動作確認 |

---

## Story Coverage Verification

| ストーリー | ユニット割当 | モジュール割当 | Phase割当 | ✓ |
|---|---|---|---|---|
| US-001 | virtual-running-app | Frontend/TopPage | B | ✓ |
| US-002 | virtual-running-app | Frontend/TopPage + Backend | A→B | ✓ |
| US-003 | virtual-running-app | Frontend/TopPage + Backend | A→B | ✓ |
| US-004 | virtual-running-app | Frontend/RunningPage + Backend | A→B | ✓ |
| US-005 | virtual-running-app | Frontend/RunningPage + Backend | A→B | ✓ |
| US-006 | virtual-running-app | Frontend/RunningPage | B | ✓ |
| US-007 | virtual-running-app | Frontend/RunningPage | B | ✓ |
| US-008 | virtual-running-app | Frontend/RunningPage | B | ✓ |
| US-009 | virtual-running-app | Frontend/CompletionPage | B | ✓ |
| US-010 | virtual-running-app | Frontend/RunningPage | B | ✓ |
| US-011 | virtual-running-app | Frontend/CompletionPage | B | ✓ |
| US-012 | virtual-running-app | Frontend + Backend (new) | Future | ✓ |
| US-013 | virtual-running-app | Frontend/RunningPage | Future | ✓ |
| US-014 | virtual-running-app | Frontend (new page) | Future | ✓ |

**全14ストーリーがユニット・モジュール・Phaseに割り当て済み ✓**
